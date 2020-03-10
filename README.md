# bigquery
Here is my historical Query

### `UNNEST`
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

### `_TABLE_SUFFIX`

### `_TABLE_DATE_RANGE`
