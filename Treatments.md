# Treatment Metrics

Treatments have different categorizations of grouping. A treatment can consist of multiple Appointments and fundamentally should be composed of a Diagnosing Provider and a Service Provider; however, currently only the Service Provider is synced. Treatments do NOT have to be completed, and all procedure codes are included as part of the treatment plan (even missed appointments).

> See below for a list of refined variations of this metric

## Diagnosed Treatments Funnel
Within this funnel a treatment will belong to exactly one status; however, as part of a funnel-metric (FM), a given value implies it came from the funnel step before it and will include any funnel state subsequent in the funnel.

The stages of a **Diagnosed Treatments Funnel** include:
* Treatments Diagnosed
* Treatments Not Rejected
* Treatments Booked
* Treatments Scheduled
* Treatments Completed

### FM Treatments Diagnosed
All procedures which are associated to a treatment plan and have not been deleted. 

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * treatmentPlanId cannot be null
  * originDate cannot be null and must fall within time range of query
  * deletedAt is null
  * isDeleted is false
</details>

### FS Treatments Diagnosed
All procedures included as treatments diagnosed which have not been accepted.

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * treatmentPlanId cannot be null
  * originDate cannot be null and must fall within time range of query
  * deletedAt is null
  * isDeleted is false
  * isAccepted is false
</details>

### FM Treatments Not Rejected
All procedures included as treatments diagnosed which were accepted and/or booked.

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * treatmentPlanId cannot be null
  * originDate cannot be null and must fall within time range of query
  * deletedAt is null
  * isDeleted is false
  * isAccepted is true
* Appointments
  * any appointment (indicates booked)
</details>


### FS Treatments Not Rejected
All procedures included as treatments diagnosed which were accepted but were not subsequently booked.

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * treatmentPlanId cannot be null
  * originDate cannot be null and must fall within time range of query
  * deletedAt is null
  * isDeleted is false
  * isCancelled is false
  * isAccepted is true
  * isCompleted is false
  * appointmentId is null
</details>

### FM Treatments Booked
All procedures included as treatments diagnosed which were at one point associated to an appointment whether it is currently active or not.

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * treatmentPlanId cannot be null
  * originDate cannot be null and must fall within time range of query
  * deletedAt is null
  * isDeleted is false
  * isCancelled is false
  * appointmentId is not null
</details>

### FS Treatments Booked
All procedures included as treatments diagnosed which are associated to an appointment that appointment is no longer active.

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * treatmentPlanId cannot be null
  * originDate cannot be null and must fall within time range of query
  * deletedAt is null
  * isDeleted is false
  * isCancelled is false
  * appointmentId is not null
  * JOIN associated appointment using logic:
    * when isCompleted is true, find Appointment for patientId on same date as entryDate
    * when isCompleted is false, use appointmentId
* Appointments
  * ANY of the following is true:
    * isDeleted is true
    * isPending is true
    * isCancelled is true
    * isMissed is true
</details>

### FS Treatments Scheduled
All procedures which have a totalAmount > 0 and are associated to either an active upcoming appointment or to a completed appointment which wasn't missed.

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * treatmentPlanId cannot be null
  * originDate cannot be null and must fall within time range of query
  * deletedAt is null
  * isDeleted is false
  * isCancelled is false
  * appointmentId is not null
  * totalAmount > 0
  * JOIN associated appointment using logic:
    * when isCompleted is true, find Appointment for patientId on same date as entryDate
    * when isCompleted is false, use appointmentId
* Appointments
  * isDeleted is false
  * isPending is false
  * isCancelled is false
  * isMissed is false
</details>

