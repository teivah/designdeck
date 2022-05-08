# DB

## ACID property

Atomic: all transaction succeeds or none does (all or nothing)

Consistency: from one valid state to another (invariants must always be true)
Not necessarily a property of the DB (e.g. foreign key constraint), can be a property of the application (e.g. credits and debits must be balanced)
!= consistency in eventual consistency (which is more about convergence as the matter is replicating data)

Isolation: a transaction is not affected by another ongoing transaction (a transaction cannot read from another transaction that has not yet completed)

Durability: once a transaction is committed, it will remain in the system

[#db](db.md)