---
uid: HistoryRecovery
---

# History recovery

The adapter you are using supports the following data collection modes which you configure in the **DataCollectionMode** parameter of your adapter's data source configuration:

- `CurrentOnly`: The adapter component operates normally. History recovery is disabled.
- `CurrentWithBackfill` (Default): The adapter component operates normally, but disconnections are recorded based on the device status. History recovery backfills data once device status is `good`.
- `HistoryOnly`: The adapter component does not get started. Historical data only are collected.

History recovery for adapters supports the following two operations related to the data collection mode:

- **On demand history recovery**: Recovers data from a specified start time or end time (or both). If neither start nor end time is specified, the default is `utcnow`. On demand history recovery is related to the `HistoryOnly` data collection mode.
- **Limited automatic history recovery**: Backfills data gaps that originated from connection disruptions, data source issues, or PI adapter shutdown or both. This is limited to a maximum time-range of four days. Limited automatic history recovery is related to the `CurrentWithBackfill` data collection mode.
