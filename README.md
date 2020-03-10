# bigquery
Here is my historical Query
## Table of contents
- [unnest](#UNNEST)
- [table suffix] (#TABLE-SUFFIX)
- [table date range] (#TABLE DATE RANGE)

# UNNEST
```
select TIMESTAMP_MICROS(event_timestamp) as Date_Action, user_id, event_name, event.value.string_value
from `table*`,
UNNEST(event_params) as event
WHERE event_name in ('event_name1','event_name2') and event.key='event_key1'
union all
select TIMESTAMP_MICROS(event_timestamp) as Date_Action, user_id, event_name, 'motor'
from `table*`
WHERE event_name in ('event_name3','event_name3','event_name4');
```
don't forget to use "," after call table before unnest

# TABLE SUFFIX
```
select user_dim.user_id as user,
       TIMESTAMP_MICROS(event.timestamp_micros) as time,
       event.date as event_date,
       event.name as event_name
from `table*`,
UNNEST(event_dim) as event
WHERE _TABLE_SUFFIX like '201707%' AND event.name = 'app_remove'
ORDER BY time ASC;
```

If the clause where we change into 
```
select user_dim.user_id as user,
       TIMESTAMP_MICROS(event.timestamp_micros) as time,
       event.date as event_date,
       event.name as event_name
from `table*`,
UNNEST(event_dim) as event
WHERE _TABLE_SUFFIX = 'app_remove' and event.date like '201707%'
ORDER BY time ASC;
```
Cannot be processed because the partition dataset per day is not per event_name

### `_TABLE_DATE_RANGE`
