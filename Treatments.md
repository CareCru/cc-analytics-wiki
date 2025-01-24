# Treatment Metrics

Treatments have different categorizations of grouping. A treatment can consist of multiple Appointments and fundamentally should be composed of a Diagnosing Provider and a Service Provider; however, currently only the Service Provider is synced. Treatments do NOT have to be completed, and all procedure codes are included as part of the treatment plan (even missed appointments).

> See below for a list of refined variations of this metric

## Diagnosed Treatments Funnel
Within this funnel a treatment will belong to exactly one status; however, as part of a funnel-metric (FM), a given value implies it came from the funnel step before it and will include any funnel state subsequent in the funnel.

The stages of a **Diagnosed Treatments Funnel** include:
* Treatments Diagnosed
* Treatments Not Rejected
* Treatments Booked
* Treatments Completed

### Treatments Diagnosed
All procedures grouped as a Treatment Plan that the practitioner has presented to the patient but not yet acted upon. This would include all treatments that have not been accepted, cancelled, deleted, or completed. When querying this metric, the date that the treatment was originally presented to the patient will be used as the date for the treatment plan.

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * treatmentPlanId cannot be null
  * originDate cannot be null and must fall within time range of query
  * deletedAt is null
  * scheduledStatus is one of 'Unscheduled', 'Cancelled', or null
</details>

<details>
  <summary>Usages:</summary>

### Dashboard
### Reporting

</details>

### FM Treatments Diagnosed
All procedures grouped as a Treatment Plan that the practitioner has presented to the patient. This would include all treatments regardless if they have been accepted, cancelled, deleted, or completed. When querying this metric, the date that the treatment was originally presented to the patient will be used as the date for the treatment plan.

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * treatmentPlanId cannot be null
  * originDate cannot be null and must fall within time range of query
  * deletedAt is null
  * scheduledStatus is one of 'Completed', 'Schedules', 'Unscheduled', 'Cancelled', or null
    * OR: scheduledStatus is 'Done (Unlinked)' AND isCompleted is FALSE
</details>

<details>
  <summary>Usages:</summary>

### Dashboard
### Reporting

</details>

SPS - Continue from here using RPT-346/RPT-351, RPT-347/RPT-353, RPT-348/RPT-352
### FM Treatments Not Rejected

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * treatmentPlanId cannot be null
  * originDate cannot be null and must fall within time range of query
  * deletedAt is null
  * scheduledStatus is one of 'Completed', 'Schedules', 'Unscheduled', 'Cancelled', or null
    * OR: scheduledStatus is 'Done (Unlinked)' AND isCompleted is FALSE
</details>

<details>
  <summary>Usages:</summary>

### Dashboard
### Reporting

</details>



