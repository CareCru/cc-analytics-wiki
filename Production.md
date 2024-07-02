## Production (cc-production)

Total value of all dental services provided by a practice which includes all billable services rendered to patients, such as examinations, cleanings, fillings, and other dental procedures.

### Historical Production
All completed delivered procedures that have occurred before the current day regardless of the type of appointment.

`Note that missed appointments are captured in this metric`

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * entryDate must be prior to the current day
  * deletedAt must be null
  * isCompleted must be true
    * `EXCEPTION: Tracker prior to 3.21.0 does not reliably set isCompleted and therefore must be ignored from consideration`
  * in rare cases when duplicate entries exist for one pmsId, then the maximum totalAmount present across those records is used in the evaluation
  * `note that isDeletedFromPms is not evaluated at this point`
</details>

<details>
  <summary>Usages:</summary>

#### Dashboard
* 30 Days Average
#### Reporting
* Production (Huron only so far)
  * Total Production
  * Practice Production by Type
    * Total Production
* Practice Performance 
  * Billed Production
* Provider Performance
  * Billed Production

</details>

Formerly known as:
* Production MTD

### Scheduled Production
Uses the monetary amount attached to upcoming delivered procedures and appointments for the **current day and days in the future**.

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * uses totalAmount
  * entryDate must be current day or in the future
  * isCompleted is ignored
  * deletedAt must be null
  * in rare cases when duplicate entries exist for one pmsId, then the maximum totalAmount present across those records is used in the evaluation
  * `note that isDeletedFromPms is not evaluated at this point`
* Appointments
  * uses estimatedRevenue
  * startDate must be current day or in the future
  * deletedAt must be null
  * isMissed must be false
  * isCancelled is false
  * isDeleted must be false
  * isPending must be false
</details>

<details>
  <summary>Usages:</summary>

#### Dashboard
* Today's Scheduled (to be verified)
#### Reporting
* Practice Performance
  * Scheduled Production
* Provider Performance
  * Scheduled Production
</details>

