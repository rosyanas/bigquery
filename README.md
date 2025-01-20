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
another example
```select d.*,
    case when product_sku like '%-vial' then REPLACE(product_sku,'-vial','')
    when product_sku like '%-OLMORVA' then REPLACE(product_sku,'-OLMORVA','')
    else product_sku end as product_sku
    from
      (select c.*,
      split(ecommerce_sku, ', ') as sku
      from
        (select a.*, case when a.ecommerce_product_sku like '%, 2' then REPLACE(a.ecommerce_product_sku,', 2','')
        when a.ecommerce_product_sku=REPLACE(b.sku_,'_',', ') then sku_induk
        else a.ecommerce_product_sku end as ecommerce_sku,
        dense_rank() over(partition by username order by date_paid, order_ecommerce_id) as order_ke
        from `oceanic-hash-232011.base.raw_ecommerce` a
        left join `oceanic-hash-232011.data_sources_external.virtual_bundle` b
          on a.ecommerce_product_sku=REPLACE(b.SKU_,'_',', ')
        --for random checking buka filter dibawah ini
        --where order_ecommerce_id='SP-230909V7Q868B2'
        where a.date_paid is not null
        ) c
      where order_ecommerce_id is not null
      ) d
    cross join UNNEST (sku) as product_sku
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
