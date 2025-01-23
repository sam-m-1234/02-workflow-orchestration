## Data Engineering Zoomcamp - Module 2 - Workflow Orchestration

### Question 1:

In `02_postgres_taxi.yaml`, update the below block

```yaml
- id: purge_files
  type: io.kestra.plugin.core.storage.PurgeCurrentExecutionFiles
  description: This will remove output files. If you'd like to explore Kestra outputs, disable it.
  disabled: true # Add this!!!
```

Run the `02_postgres_taxi` from kestra, with inputs: Taxi type=yellow, year=2020, month=12.
After execution, go to Flows->02_postgres_taxi->Executions->Outputs.
Select the 'extract' task and click on outputFiles->yellow_tripdata_2020-12.csv. The filesize (128.3MB - Option 1) will be displayed on the right side of the screen.

### Question 2:

When `taxi` is set to `green`, `year` is set to `2020`, and `month` is set to `04`, the variable `file`, defined as `{{inputs.taxi}}_tripdata_{{trigger.date | date('yyyy-MM')}}.csv` will resolve to `green_tripdata_2020-04.csv` (Option 2).

### Question 3:

Go to Flows->02_postgres_taxi_scheduled->Triggers
Click 'Backfill executions' for the `yellow_schedule`.
Set 'Taxi type' to 'yellow' 'Start' to '2020-01-01 00:00:00' and 'End' to '2020-12-31 00:00:00' and click 'Execute backfill'.
After the executions are finished, go to pgAdmin and run the following query against the `yellow_tripdata` table:

```sql
SELECT COUNT(*) FROM public.yellow_tripdata
WHERE filename LIKE 'yellow_tripdata_2020%';
```

The result should be `24648499` (Option 2)

### Question 4:

Go to Flows->02_postgres_taxi_scheduled->Triggers
Click 'Backfill executions' for the `green_schedule`.
Set 'Taxi type' to 'green' 'Start' to '2020-01-01 00:00:00' and 'End' to '2020-12-31 00:00:00' and click 'Execute backfill'.
After the executions are finished, go to pgAdmin and run the following query against the `green_tripdata` table:

```sql
SELECT COUNT(*) FROM public.green_tripdata
WHERE filename LIKE 'green_tripdata_2020%';
```

The result should be `1734051` (Option 3)

### Question 5:

Go to Flows->02_postgres_taxi_scheduled->Triggers
Click 'Backfill executions' for the `yellow_schedule`.
Set 'Taxi type' to 'yellow' 'Start' to '2021-01-01 00:00:00' and 'End' to '2021-07-31 00:00:00' and click 'Execute backfill'.
After the executions are finished, go to pgAdmin and run the following query against the `yellow_tripdata` table:

```sql
SELECT COUNT(*) FROM public.yellow_tripdata
WHERE filename = 'yellow_tripdata_2021-03.csv';
```

The result should be `1925152` (Option 3)

### Question 6:

As per https://kestra.io/docs/workflow-components/triggers/schedule-trigger, you would set the `timezone` property to `America/New_York` (Option 2) in the Schedule trigger configuration as per below:

````yaml
triggers:
  - id: daily
    type: io.kestra.plugin.core.trigger.Schedule
    cron: "@daily"
    timezone: America/New_York # !!!
    ```
````
