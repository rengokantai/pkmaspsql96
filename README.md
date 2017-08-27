# pkmaspsql96

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
in 9.6(personally I failed this. No reason?)
```
SELECT tsquery('united <2> nations')@@to_tsvector('are we really united, happy nations?');
```
