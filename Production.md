# Production Metrics

## Billed Production
Total value of all completed dental services provided by a practice which includes all billable services rendered to patients, such as examinations, cleanings, fillings, and other dental procedures and were completed before the current day.

> Note: missed appointments are included

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * entryDate must be prior to the current day
  * deletedAt must be null
  * isCompleted must be true
  * practitionerId cannot be null
  * in rare cases when duplicate entries exist for one pmsId, then the maximum totalAmount present across those records is used in the evaluation
  * `Note: isDeletedFromPms is not evaluated at this point`
</details>

Formerly known as:
* Historical Production
* Production MTD

## Scheduled Production
Total value of expected dental services to be provided by a practice by using the monetary amount attached to upcoming appointments for the current day and days in the future.

> Note: Only dates including the current day and any in the future are included in the Scheduled Production metric.

> Note: In Tracker the value does not incorporate values in the delivered procedures; whereas in Dentrix those values are synced as delivered procedures are added.

<details>
<summary>Technical Details:</summary>

* Appointments
  * uses estimatedRevenue
  * startDate must be current day or in the future
  * deletedAt must be null
  * isDeleted must be false
  * isMissed must be false
  * isCancelled is false
  * isPending must be false
</details>

## Expected Production
The sum of all production which was delivered as well as estimates of all of the upcoming production to yet be performed. Production occurring on the current day is captured only as an estimate of what is to be performed.

> **Historical Production** + **Scheduled Production**

<details>
<summary>Technical Details:</summary>

* see [Historical Production](#billed-production)
* see [Scheduled Production](#scheduled-production)
</details>

Formerly known as:
* Month Forecast

