# 1NF
- using row order to convey info is not permitted
- mixing data type in same column is not permitted
- having a table without a primary key is not permitted
- repeating groups are not permitted
# 2NF
- each non-key attribute in the table must be dependent on entire primary key
# 3NF
- each non-key attribute in the table must depend on the key, whole key and nothing but the key
# Boyce-Codd Normal Form(BCNF)
- each attribute in the table must depend on the key, the whole key and nothing but the key (same as 3NF but different is apply to all attribute)
# 4NF
- the only kinds of multivalued dependency allowed in a table are multivalued dependencies on the key
# 5NF
- It must not be possible to describe the table as being logical result of joining some other tables together