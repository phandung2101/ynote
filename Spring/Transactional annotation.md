# Properties
These are some configuration like:
- the `Propagation Type` of the transaction
- the `Isolation Level` of the transaction
- a `Timeout` for the operation wrapped by the transaction
- a `readOnly` flag - a hint for persistence provider that the transaction should be read only
- the `Rollback` rules for the transaction
> **The checked exception does not trigger a rollback**. But we can configure it by using `rollbackFor` and `noRollbackFor`.

# How to work with it
## Specifying transaction propagation 
By default, `@Transactional` uses `Propagation.REQUIRED` and `Isolation.DEFAULT`.
```java
@Service
public class OrderService {

    private final InventoryService inventoryService;

    public OrderService(InventoryService inventoryService) {
        this.inventoryService = inventoryService;
    }

    @Transactional(propagation = Propagation.REQUIRED)
    public void placeOrder(Order order) {
        // Save order to database
        saveOrder(order);
        
        // Update inventory (calls another transactional method)
        inventoryService.updateInventory(order.getProductId(), order.getQuantity());
    }

    private void saveOrder(Order order) {
        // Assume JPA or JDBC operation to persist order
    }
}

@Service
class InventoryService {

    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void updateInventory(Long productId, int quantity) {
        // Update inventory stock in database
        // This runs in a new transaction, independent of the calling method
    }
}
```
Explaination:
- `placeOrder` uses `Propagation.REQUIRED`, joining an existing transaction or creating a new one.
- `updateInventory` uses `Propagation.REQUIRES_NEW`, suspending the current transaction and starting a new one.
- If `updateInventory` fails, its transaction rolls back independently, but `placeOrder`'s transaction can still commit (or roll back separately).
## Handling rollback
```java
@Service
public class PaymentService {

    @Transactional(rollbackOn = PaymentProcessingException.class)
    public void processPayment(Long userId, double amount) throws PaymentProcessingException {
        // Deduct balance
        updateUserBalance(userId, amount);
        
        // Process payment via external service
        boolean paymentSuccess = callPaymentGateway(amount);
        if (!paymentSuccess) {
            throw new PaymentProcessingException("Payment gateway failed");
        }
        
        // Log transaction
        logTransaction(userId, amount);
    }

    private void updateUserBalance(Long userId, double amount) {
        // Update user balance in database
    }

    private boolean callPaymentGateway(double amount) {
        // Simulate external payment gateway call
        return false; // Simulating failure
    }

    private void logTransaction(Long userId, double amount) {
        // Log transaction in database
    }
}

class PaymentProcessingException extends Exception {
    public PaymentProcessingException(String message) {
        super(message);
    }
}
```
**Explanation**:
- `rollbackOn = PaymentProcessingException.class` ensures the transaction rolls back only if `PaymentProcessingException` is thrown.
- By default, Spring rolls back on **unchecked exceptions** (RuntimeException and its subclasses) but not on checked exceptions unless specified via `rollbackOn`.
- If the payment gateway fails, the balance update and any other changes are rolled back.
## Readonly transactional
```java
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;
import java.util.List;

@Service
public class ReportService {

    @PersistenceContext
    private EntityManager entityManager;

    @Transactional(readOnly = true)
    public List<User> getActiveUsers() {
        return entityManager.createQuery("SELECT u FROM User u WHERE u.active = true", User.class)
                           .getResultList();
    }
}
```

Explaination:
- `readOnly = true` work like a hint for Hibernate and database that transaction is read-only
- using to fetch data only
- increase performance of query
# Advance Property Config
## Isolation
```java
@Transactional(isolation = Isolation.DEFAULT)
public void someMethod() {
    // Database default isolation applies
}
```
### 1. DEFAULT
Let database and hibernate decide behavior of transaction. Most database default to `READ_COMMITED` and `REPEATABLE_READ`
### 2. READ_UNCOMMITTED
Allow transactions to read uncommited change (dirty works) made by other transactions. This is least strict isolation level
**Concurrency Issues:**
- Dirty reads: read from uncommit data or may be rolled back
- Non-repeatable reads: data read from single transaction may be differ if another transaction commit save
- Phantom reads: new row should appear in data set that inserted from another transaction
### 3. READ_COMMITED
Only data that commited could read from this isolation level. This is default for many database (MySQL, PostgreSQL)
**Concurrency Issues**:
- Non-repeatable reads
- Phantom reads
### 4. REPEATABLE_READ
Guarantees that multiple read in transaction remains consistent for subsequent reads, preventing dirty reads and non-repeatable reads. However, phantom reads are still possible
**Concurrency Issues:**
- Phantom reads
### 5. SERIALIZABLE
Strictest mode of isolation level. Prevent all issue above but trade-off is system performance may lead to deadlocks or reduced concurrency
## Propagation
### 1. REQUIRED (Default)
If a transaction existed then join it, otherwise create a new transaction.
**Pros**: simplify transaction management, ensure atomicity between multiple operation
**Cons**: Long-running transacitons may hold resource for a long time.
```java
@Transactional(propagation = Propagation.REQUIRED)
public void placeOrder(Order order) {
    saveOrder(order);
    updateInventory(order.getProductId(), order.getQuantity());
}
```
### 2. REQUIRES_NEW
Always create new transaction, suspending the existing transaction
```java
@Transactional(propagation = Propagation.REQUIRES_NEW)
public void logAuditEvent(String event) {
    // Always commits, even if outer transaction rolls back
    entityManager.persist(new AuditLog(event));
}
```
Usecase: do some process that must alway isolate with caller transaction
### 3. NESTED
Create a nested transaction within existed transaction or create a new one if none existed.
Use when you want sub-transaction that can rollback independently without effect to outer transaction. (partial rollback in complex workflow)
**Behavior:**
- If an outer transaction exists, the method runs in a nested transaction (using savepoint)
- If the nested transaction fail, only its change are rolled back; the outer transaction can continue
- if no transaction exist create a new one like `REQUIRED`
### 4. MANDATORY
Require an existed transaction; throw an exception if none exists. Throw `TransactionRequiredException` if no transaction exists.
### 5. NEVER
Execute non-transactionally and throw exception if transaction exist.
### 6. NOT_SUPPORTED
Allow to execute non-transactionally flow in transaction flow, suspending current transaction if existed. Like call 3rd system for example. 
### 7. SUPPORTS
Allow join current transaction or if none exist will do as non-transactionally

