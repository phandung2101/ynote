 The ACID principle ensure reliable database transactions with 4 properties:
# Atomicity - All or nothing
A transaction is treated as single unit of work. Either all changes within the transaction successful or none of them are applied. 
# Consistency - Keeping rules intact
Database must ensure valid before and after transaction. It must follow all rules, like ensuring account balance never be negative
# Isolation - No interference
Isolation ensure that if multiple transactions process at the same time. They can not know other state or incomplete changes from others. For examples, if two transaction update the same account, one waits until the other to finished.
# Durability - Change sticks
It mean when one transaction is done and complete, all change must be permanently. Even if system crash, power outage, the changes are saved and won't be lost.