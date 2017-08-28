# pkmaspsql96
__using \x to enable vertical display__
## What is new in PostgreSQL 9.6?

### Understanding new database administration functions

#### Killing idle sessions
```
SET idle_in_transaction_session_timeout TO 2500; 
```

#### Finding more detailed information in pg_stat_activity
```
\d pg_stat_activity 
SELECT * FROM pg_blocking_pids(4711);
```

#### Tracking vaccum progress
```
select * from pg_stat_progress_vacuum;
```


### Digging into new SQL and developer-related functions
```
SELECT phraseto_tsquery('Under pressure') to_tsvector('Something was under some sort of pressure');
```
```
SELECT phraseto_tsquery('Under pressure')@@to_tsvector('Under pressure by David Bowie hit number 1 again');
```
in 9.6 __(personally I failed this. No reason?)__
```
SELECT tsquery('united <2> nations')@@to_tsvector('are we really united, happy nations?');
```
### Using new backup and replication functionality
#### Streamlining wal_level and monitoring
New ```pg_stat_wal_receiver``` function in 9.6.  
It is basically the slave-side mirror of the ```pg_stat_replication``` function and helps to determine the state of replication.


## Understanding Transactions and Locking
### Working with PostgreSQL transactions
#### Making use of savepoints
```
test=# BEGIN; 
BEGIN 
test=# SELECT 1; 
 ?column?  
---------- 
        1 
(1 row) 

test=# SAVEPOINT a; 
SAVEPOINT 
test=# SELECT 2 / 0; 
ERROR:  division by zero 
test=# ROLLBACK TO SAVEPOINT a; 
ROLLBACK 
test=# SELECT 3; 
 ?column?  
---------- 
        3 
(1 row) 

test=# COMMIT; 
COMMIT 
```
What will happen if you try to reach a savepoint after a transaction has ended?  
The life of a savepoint ends as soon as the transaction ends.  
In other words, there is no way to return to a certain point in time after the transactions have been completed.  

#### Transactional DDLs
```
BEGIN;
CREATE TABLE t_test (id int); 
ALTER TABLE t_test ALTER COLUMN id TYPE int8; 
ROLLBACK;
d t_test  //error
```
### Understanding basic locking
