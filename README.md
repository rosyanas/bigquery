# bigquery
Here is my historical Query
## Table of contents
- [UNNEST](#UNNEST)
- [TABLE SUFFIX](#TABLE-SUFFIX)
- [References](#References)

## UNNEST
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

## TABLE SUFFIX
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
If the clause where we change into `... WHERE _TABLE_SUFFIX = 'app_remove' and event.date like '201707%' ...` cannot be processed because the partition dataset per day is not per event_name.

Another example you can explore:
```
SELECT concat(year,mo,da) as date, avg(temp) as avgtemp, sum(count_temp) as observations
FROM `bigquery-public-data.noaa_gsod.gsod20*`
WHERE _TABLE_SUFFIX = '01' OR _TABLE_SUFFIX = '10'
GROUP BY date ORDER BY date;
```

## TABLE DATE RANGE

## References
- [`unnest`](https://cloud.google.com/bigquery/docs/reference/standard-sql/aggregate_functions)
- [`_table_suffix`](https://cloud.google.com/bigquery/docs/querying-wildcard-tables)
