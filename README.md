# bigquery
all about Query in BigQuery

```select TIMESTAMP_MICROS(event_timestamp) as Date_Action, user_id, event_name, event.value.string_value
from
`table*`,
UNNEST(event_params) as event
WHERE event_name in ('PriceEngine_Result_Button_Jual','PriceEngine_Result_Button_Loan') and event.key='vehicle_type'
union all
select TIMESTAMP_MICROS(event_timestamp) as Date_Action, user_id, event_name, 'motor'
from
`firebase-sumo365.analytics_151119988.events_*`
WHERE event_name in ('SmartInspection_Result_Button_Jual','bt_jubel_valuation_clicked','loan_clicked');```
