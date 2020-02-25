# bigquery
all about Query in BigQuery

### UNNEST
```
SQL
select TIMESTAMP_MICROS(event_timestamp) as Date_Action, 
user_id, event_name, event.value.string_value
from
`table*`,
UNNEST(event_params) as event
WHERE event_name in ('event_name1','event_name2') 
and event.key='event_key1'
union all
select TIMESTAMP_MICROS(event_timestamp) as Date_Action, 
user_id, event_name, 'motor'
from
`table*`
WHERE event_name in ('event_name3','event_name3','event_name4');
```
* [Dropwizard](http://www.dropwizard.io/1.0.2/docs/) - The web framework used
* **Billie Thompson** - *Initial work* - [PurpleBooth](https://github.com/PurpleBooth)
