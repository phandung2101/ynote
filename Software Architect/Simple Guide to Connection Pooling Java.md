## **1. Overview**

Connection pooling is a well-known data access pattern. Its main purpose is to reduce the overhead involved in performing database connections and read/write database operations.

**At the most basic level,** **a connection pool is a database connection cache implementation** that can be configured to suit specific requirements.

In this tutorial, we'll discuss a few popular connection pooling frameworks. Then we'll learn how to implement our own connection pool from scratch.

## **2. Why Connection Pooling?**

Of course, this question is rhetorical.

If we analyze the sequence of steps involved in a typical database connection life cycle, we'll understand why:

1. Opening a connection to the database using the database driver
2. Opening a [TCP socket](https://en.wikipedia.org/wiki/Network_socket) for reading/writing data
3. Reading / writing data over the socket
4. Closing the connection
5. Closing the socket

It becomes evident that **database connections are fairly expensive operations**, and as such, should be reduced to a minimum in every possible use case (in edge cases, just avoided).

Here's where connection pooling implementations come into play.

By just simply implementing a database connection container, which allows us to reuse a number of existing connections, we can effectively save the cost of performing a huge number of expensive database trips. This boosts the overall performance of our database-driven applications.

## **3. JDBC Connection Pooling Frameworks**

From a pragmatic perspective, implementing a connection pool from the ground up is pointless considering the number of “enterprise-ready” connection pooling frameworks already available.

From a didactic perspective, which is the goal of this article, it's not.

Even so, before we learn how to implement a basic connection pool, we'll first showcase a few popular connection pooling frameworks.

### **3.1. Apache Commons DBCP**

Let's start with [Apache Commons DBCP Component](https://commons.apache.org/proper/commons-dbcp/download_dbcp.cgi), a full-featured connection pooling JDBC framework:

```java
public class DBCPDataSource {

    private static BasicDataSource ds = new BasicDataSource();

    static {
        ds.setUrl("jdbc:h2:mem:test");
        ds.setUsername("user");
        ds.setPassword("password");
        ds.setMinIdle(5);
        ds.setMaxIdle(10);
        ds.setMaxOpenPreparedStatements(100);
    }

    public static Connection getConnection() throws SQLException {
        return ds.getConnection();
    }

    private DBCPDataSource(){ }
}

```

In this case, we used a wrapper class with a static block to easily configure DBCP's properties.

And here's how to get a pooled connection with the *DBCPDataSource* class:

```java
Connection con = DBCPDataSource.getConnection();

```

### **3.2. HikariCP**

Now let's look at [HikariCP](https://github.com/brettwooldridge/HikariCP), a lightning-fast JDBC connection pooling framework created by [Brett Wooldridge](https://github.com/brettwooldridge) (for the full details on how to configure and get the most out of HikariCP, please check out [this article](https://www.baeldung.com/hikaricp)):

```java
public class HikariCPDataSource {

    private static HikariConfig config = new HikariConfig();
    private static HikariDataSource ds;

    static {
        config.setJdbcUrl("jdbc:h2:mem:test");
        config.setUsername("user");
        config.setPassword("password");
        config.addDataSourceProperty("cachePrepStmts", "true");
        config.addDataSourceProperty("prepStmtCacheSize", "250");
        config.addDataSourceProperty("prepStmtCacheSqlLimit", "2048");
        ds = new HikariDataSource(config);
    }

    public static Connection getConnection() throws SQLException {
        return ds.getConnection();
    }

    private HikariCPDataSource(){}
}

```

Similarly, here's how to get a pooled connection with the *HikariCPDataSource* class:

```java
Connection con = HikariCPDataSource.getConnection();

```

### **3.3. C3P0**

Last in this review is [C3P0](https://www.mchange.com/projects/c3p0/), a powerful JDBC4 connection and statement pooling framework developed by Steve Waldman:

```java
public class C3p0DataSource {

    private static ComboPooledDataSource cpds = new ComboPooledDataSource();

    static {
        try {
            cpds.setDriverClass("org.h2.Driver");
            cpds.setJdbcUrl("jdbc:h2:mem:test");
            cpds.setUser("user");
            cpds.setPassword("password");
        } catch (PropertyVetoException e) {
            // handle the exception
        }
    }

    public static Connection getConnection() throws SQLException {
        return cpds.getConnection();
    }

    private C3p0DataSource(){}
}

```

As expected, getting a pooled connection with the *C3p0DataSource* class is similar to the previous examples:

```java
Connection con = C3p0DataSource.getConnection();

```

## **4. A Simple Implementation**

To better understand the underlying logic of connection pooling, let's create a simple implementation.

We'll start out with a loosely coupled design based on just one single interface:

```java
public interface ConnectionPool {
    Connection getConnection();
    boolean releaseConnection(Connection connection);
    String getUrl();
    String getUser();
    String getPassword();
}

```

The *ConnectionPool* interface defines the public API of a basic connection pool.

Now let's create an implementation that provides some basic functionality, including getting and releasing a pooled connection:

```java
public class BasicConnectionPool
  implements ConnectionPool {

    private String url;
    private String user;
    private String password;
    private List<Connection> connectionPool;
    private List<Connection> usedConnections = new ArrayList<>();
    private static int INITIAL_POOL_SIZE = 10;

    public static BasicConnectionPool create(
      String url, String user,
      String password) throws SQLException {

        List<Connection> pool = new ArrayList<>(INITIAL_POOL_SIZE);
        for (int i = 0; i < INITIAL_POOL_SIZE; i++) {
            pool.add(createConnection(url, user, password));
        }
        return new BasicConnectionPool(url, user, password, pool);
    }

    // standard constructors

    @Override
    public Connection getConnection() {
        Connection connection = connectionPool
          .remove(connectionPool.size() - 1);
        usedConnections.add(connection);
        return connection;
    }

    @Override
    public boolean releaseConnection(Connection connection) {
        connectionPool.add(connection);
        return usedConnections.remove(connection);
    }

    private static Connection createConnection(
      String url, String user, String password)
      throws SQLException {
        return DriverManager.getConnection(url, user, password);
    }

    public int getSize() {
        return connectionPool.size() + usedConnections.size();
    }

    // standard getters
}

```

While pretty naive, the *BasicConnectionPool* class provides the minimal functionality that we'd expect from a typical connection pooling implementation.

In a nutshell, the class initializes a connection pool based on an *ArrayList* that stores 10 connections, which can be easily reused.

**It's also possible to create JDBC connections with the [*DriverManager* class](https://docs.oracle.com/en/java/javase/11/docs/api/java.sql/java/sql/DriverManager.html) and [Datasource](https://docs.oracle.com/en/java/javase/11/docs/api/java.sql/javax/sql/DataSource.html) implementations**.

As it's much better to keep the creation of connection databases agnostic, we used the former within the *create()* static factory method.

In this case, we placed the method within the *BasicConnectionPool* because this is the only implementation of the interface.

In a more complex design, with multiple *ConnectionPool* implementations, it would be preferable to place it in the interface, thus getting a more flexible design and greater level of cohesion.

The most relevant point to stress here is that once the pool is created, **connections are fetched from the pool, so there's no need to create new ones**.

Furthermore, **when a connection is released, it's actually returned back to the pool, so other clients can reuse it**.

There's no further interaction with the underlying database, such as an explicit call to the *Connection's close()* method.

## **5. Using the *BasicConnectionPool* Class**

As expected, using our *BasicConnectionPool* class is straightforward.

Let's create a simple unit test and get a pooled in-memory [H2](http://www.h2database.com/html/main.html) connection:

```java
@Test
public whenCalledgetConnection_thenCorrect() {
    ConnectionPool connectionPool = BasicConnectionPool
      .create("jdbc:h2:mem:test", "user", "password");

    assertTrue(connectionPool.getConnection().isValid(1));
}

```

## **6. Further Improvements and Refactoring**

Of course, there's plenty of room to tweak/extend the current functionality of our connection pooling implementation.

For instance, we could refactor the *getConnection()* method and add support for maximum pool size. If all available connections are taken, and the current pool size is less than the configured maximum, the method will create a new connection.

We could also verify whether the connection obtained from the pool is still alive before passing it to the client:

```java
@Override
public Connection getConnection() throws SQLException {
    if (connectionPool.isEmpty()) {
        if (usedConnections.size() < MAX_POOL_SIZE) {
            connectionPool.add(createConnection(url, user, password));
        } else {
            throw new RuntimeException(
              "Maximum pool size reached, no available connections!");
        }
    }

    Connection connection = connectionPool
      .remove(connectionPool.size() - 1);

    if(!connection.isValid(MAX_TIMEOUT)){
        connection = createConnection(url, user, password);
    }

    usedConnections.add(connection);
    return connection;
}

```

Note that the method now throws *SQLException*, meaning we'll have to update the interface signature as well.

Or we could add a method to gracefully shut down our connection pool instance:

```java
public void shutdown() throws SQLException {
    usedConnections.forEach(this::releaseConnection);
    for (Connection c : connectionPool) {
        c.close();
    }
    connectionPool.clear();
}

```

In production-ready implementations, a connection pool should provide a bunch of extra features, such as the ability to track the connections that are currently in use, support for prepared statement pooling, and so forth.

In order to keep this article simple, we'll omit how to implement these additional features, and keep the implementation non-thread-safe for the sake of clarity.

## **7. Conclusion**

In this article, we took an in-depth look at what connection pooling is, and learned how to roll our own connection pooling implementation.

Of course, we don't have to start from scratch every time we want to add a full-featured connection pooling layer to our applications.

That's why we started by exploring some of the most popular connection pool frameworks, so we have a clear idea of how to work with them and pick the one that best suits our requirements.

As usual, all the code samples shown in this article are available [over on GitHub](https://github.com/eugenp/tutorials/tree/master/persistence-modules/core-java-persistence).