# Difference between teradata and Hive

## decimal(precise,scale)
*When facing overflow on deciaml type column, hive will see this data as NULL. Teradata will report error.*

```
table Test
    colume: col1 decimal(4,2)

Teradata 
insert into test values(100.20);

Error.

Hive 
insert into test values(100.20);

Succeed. But select col1 get null.
```
