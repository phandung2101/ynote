# Intention
**Builder** is a creational design pattern that lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code.

# Problems
When object have a lot of data you will need construct a lot of unused data.
```java
final var program = new Program(  
        "Use this only",  
        null,  
        null,  
        null,  
        null,  
        null,  
        null,  
        null,  
        null  
);
```
Or may be can use setter like this but field like not very clearly and we don't know what actually should happen when create object step by step
```java
final var program = new Program();  
program.setConfigA("fix");
```
# Solution
```java
static class ProgramBuilder {  
    private String configA;  
    private String configB;  
    private String configC;  
    private String configD;  
    private String configE;  
    private String configF;  
    private String configJ;  
    private String configK;  
    private String configM;  
  
    protected void setConfigA(final String configA) {  
        this.configA = configA;  
    }  
  
    protected void setConfigB(final String configB) {  
        this.configB = configB;  
    }  
  
    protected void setConfigC(final String configC) {  
        this.configC = configC;  
    }  
  
    protected void setConfigD(final String configD) {  
        this.configD = configD;  
    }  
  
    protected void setConfigE(final String configE) {  
        this.configE = configE;  
    }  
  
    protected void setConfigF(final String configF) {  
        this.configF = configF;  
    }  
  
    protected void setConfigJ(final String configJ) {  
        this.configJ = configJ;  
    }  
  
    protected void setConfigK(final String configK) {  
        this.configK = configK;  
    }  
  
    protected void setConfigM(final String configM) {  
        this.configM = configM;  
    }  
    public Program build() {  
        return new Program(  
                this.configA,  
                this.configB,  
                this.configC,  
                this.configD,  
                this.configE,  
                this.configF,  
                this.configJ,  
                this.configK,  
                this.configM  
        );  
    }  
    }  

// Client code
public static void main(String[] args) {  
    final var programBuilder = new ProgramBuilder();  
    programBuilder.setConfigA("configA");  
    programBuilder.setConfigC("configC");  
    programBuilder.setConfigD("configD");  
    final var program = programBuilder.build();  
}
```

Now we can hide complex thing from client code. We can make it more clearly with `chainning method`
```java
// Client code
public static void main(String[] args) {  
    final var programBuilder = new ProgramBuilder();  
    final var program = programBuilder  
            .setConfigA("configA")  
            .setConfigC("configC")  
            .setConfigD("configD")  
            .build();  
}
```
# Application
- Use the Builder pattern to get rid of a “telescoping constructor”.
```
class Pizza {
    Pizza(int size) { ... }
    Pizza(int size, boolean cheese) { ... }
    Pizza(int size, boolean cheese, boolean pepperoni) { ... }
    // ...
```
- Use the Builder pattern when you want your code to be able to create different representations of some product (for example, stone and wooden houses).
-  Use the Builder to construct [Composite](https://refactoring.guru/design-patterns/composite) trees or other complex objects.