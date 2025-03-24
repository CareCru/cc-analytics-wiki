These pages capture the definitions used to drive the metrics presented throughout CareCru.

To avoid excessive navigation, multiple related metrics are grouped into a related ancestor-metric within one page.

Within Intelligence reports, the metrics are calculated overnight and therefore do not reflect changes made in the current day.

### Fundamental Metrics
There are some initial definitions worth understanding which many of the subsequent metrics are built from. As suitable order for initial understanding is:
* [Production](./Production.md)
* [Visits](./Visits.md)
* [Patients](./Patients.md)

From there, the other metrics are more interchangeable with less back-references.

### Funnel Metrics
**Funnel Metrics** are a specific method of grouping related metrics. The best way to visualize these metrics is in regards 
to a funnel chart. These metrics have several key concepts:
* Backward Qualification: every record will have achieved a specific stage; however, to get to that stage it MUST have previously been valid at the previous stage.
  * ie. To get to "Stage 3", it will only be included if it met the qualifications at some point previously to be considered in "Stage 2"
* An "FM ‹metric›" (funnel metric) captures if that record passed through that stage of the journey and is therefore cumulative of the subsequent stages as well.
  * ie. "Stage 2" will include an aggregation of all records currently in "Stage 2" as well as the aggregation of all records that occur after that stage
* An "FS ‹metric›" (funnel stage) is defined as the most advanced stage achieved in the journey of that funnel for a record.
  * ie. if a record passed through "Stage 2" but not "Stage 3", then the "FS ‹metric›" will only count towards "Stage 2" and not "Stage 1" nor any subsequent stage
