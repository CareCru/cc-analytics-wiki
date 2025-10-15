# Treatment Metrics

Treatments are procedures that are linked to a treatment plan. 

> Note: further refinements should be made to Treatments to match the discrepancies called out in `Production` due to the difference between past and future procedures.

## Treatment

Total value or count of all dental services associated to a treatment plan.

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * deletedAt is null
  * treatmentPlanId cannot be null
  * originDate cannot be null and must fall within time range of query
  * Note: when `scheduledStatus` is 'Done (Unlinked)' then include values with `isCompleted` = false for calculating completed procedures
</details>


---

## Diagnosed Treatments Funnel
Within this funnel a treatment will belong to exactly one status; however, as part of a funnel-metric (FM), a given value implies it came from the funnel step before it and will include any funnel state subsequent in the funnel.

The stages of a **Diagnosed Treatments Funnel** include:
* **FM Diagnosed** - All procedures which are associated to a treatment plan and have not been deleted
* **FM Opportunity** - All treatment which have not been marked as rejected
* **FM Booked** - All treatments which have at some point had an appointment (regardless if it was just pending, or has been subsequently cancelled, etc)
* **FM Scheduled** - All treatments which are linked to an upcoming appointment that is still in good standing
* **FM Completed** - All treatments which have been completed

---

### FM Diagnosed
All procedures which are associated to a treatment plan and have not been deleted.

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * deletedAt is null
  * treatmentPlanId cannot be null
  * originDate cannot be null and must fall within time range of query
</details>

### FM Opportunity
All procedures included as [FM Diagnosed](#fm-diagnosed) which were not marked as rejected.

<details>
<summary>Technical Details:</summary>

* see [Diagnosed Treatments : FM Diagnosed](#fm-diagnosed)
* DeliveredProcedure
  * isAccepted is not false
</details>

### FM Booked
All procedures included as [FM Not Rejected](#fm-not-rejected) which were at one point associated to an appointment whether it is currently active or not, or which are captured as [FM Completed](#fm-completed).

<details>
<summary>Technical Details:</summary>

* see [Diagnosed Treatments : FM Not Rejected](#fm-not-rejected)
* see [Diagnosed Treatments : FM Completed](#fm-completed)
* DeliveredProcedure
  * appointmentId is not null
</details>

### FM Scheduled
All procedures included as [FM Opportunity](#fm-opportunity) which have an [FM Completed](#fm-completed) procedure or a currently active appointment.

<details>
<summary>Technical Details:</summary>

* see [Diagnosed Treatments : FM Opportunity](#fm-opportunity)
* see [Diagnosed Treatments : FM Completed](#fm-completed)
* DeliveredProcedure
  * appointmentId is not null
* Appointment
  * deletedAt is null
  * isDeleted is false
  * isCancelled is false
  * isMissed is false
  * isPending is false
</details>

### FM Completed
All procedures included as [FM Diagnosed](#fm-diagnosed) which have been completed.

<details>
<summary>Technical Details:</summary>

* see [Diagnosed Treatments : FM Diagnosed](#fm-diagnosed)
* DeliveredProcedure
  * isCompleted is true OR scheduledStatus = 'Done (Unlinked)'
</details>

---


### FS Diagnosed
All procedures included as treatments diagnosed which have not been accepted.

<details>
<summary>Technical Details:</summary>

* [Diagnosed Treatments : FM Diagnosed](#fm-diagnosed) but0 not [Diagnosed Treatments : FM Opportunity](#fm-opportunity)
</details>

### FS Opportunity
All procedures included as treatments diagnosed which were accepted but were not subsequently booked.

<details>
<summary>Technical Details:</summary>

* [Diagnosed Treatments : FM Not Rejected](#fm-diagnosed) but not [Diagnosed Treatments : FM Booked](#fm-booked)
</details>

### FS Booked
All procedures included as treatments diagnosed which are associated to an appointment that appointment is no longer active.

<details>
<summary>Technical Details:</summary>

* [Diagnosed Treatments : FM Booked](#fm-booked) but not [Diagnosed Treatments : FM Scheduled](#fm-scheduled)
</details>


### FS Scheduled
All procedures which have a valid appointment but have not been completed.

<details>
<summary>Technical Details:</summary>

* [Diagnosed Treatments : FM Scheduled](#fm-booked) but not [Diagnosed Treatments : FM Completed](#fm-completed)
</details>

### FS Completed
Synonymous with [FM Completed](#fm-completed)

<details>
<summary>Technical Details:</summary>

* [Diagnosed Treatments : FM Completed](#fm-completed)
</details>
