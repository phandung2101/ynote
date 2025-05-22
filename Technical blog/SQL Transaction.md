# Isolation level
## 1. Read uncommitted
Allow transactions to read uncommited change (dirty works) made by other transactions. This is least strict isolation level
**Concurrency Issues:**
- Dirty reads: read from uncommit data or may be rolled back
- Non-repeatable reads: data read from single transaction may be differ if another transaction commit save
- Phantom reads: new row should appear in data set that inserted from another transaction
## 2. Read committed
Only data that commited could read from this isolation level. This is default for many database (MySQL, PostgreSQL)
**Concurrency Issues**:
- Non-repeatable reads
- Phantom reads
## 3. Repeatable read
Guarantees that multiple read in transaction remains consistent for subsequent reads, preventing dirty reads and non-repeatable reads. However, phantom reads are still possible
**Concurrency Issues:**
- Phantom reads
## 4. Serializable
Strictest mode of isolation level. Prevent all issue above but trade-off is system performance may lead to deadlocks or reduced concurrency
# Locks
## 1. Shared Lock (S)
- type of read lock
- multiple transaction could use same shared lock on same resource
- not allow write on used resource
## 2. Execlusive Lock (X)
- use when insert, update, delete 
- only 1 transaction could hold lock on resource
- not allow read/write from another transaction
## 3. Update Lock (U)
- middleware lock, usually be used when transaction have intent to update data
- stop another transaction put execlusive lock before complete
## 4. Intent Lock (IS, IX, SIX)
- use to notify intent lock on child resource (on table when lock row)
- consist of: Intent Share (IS), Intent Execlusive (IX), Shared with Intent Execlusive (SIX)
## 5. Range Lock
- use at **serializable** isolation level to lock range of data, lock insert new data on its.
# How lock work on Isolation level
## Read uncommited
- not use **shared lock** on read
- use **execlusive lock** on write
- because of not use **share lock** -> transaction could read current change data from another transaction => lead to **dirty read**
> Example: T1 update a row (put execlusive lock) but didn't commit. T2 could read that row without waiting -> leading to **dirty data**
## Read committed
- use **shared lock** on read data but **release after read (not till end of transaction)**
- use **execlusive lock** on write until commit/rollback of transaction
- lock apply on **row-level blocking**, not lock all table
> Example: T1 read a row, put shared lock then release. T2 commit change to that row. T1 read same row againt and that row has been change. -> leading to **non-reapeatable read**
## Repeatable read
- use **shared lock** on read data, 