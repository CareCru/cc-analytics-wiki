# Visit Metrics

Visit is an aggregation of multiple Appointments or DeliveredProcedures occurring on one day.

> See below for a list of refined variations of this metric

## Historical Patient Visits
Aggregation of multiple delivered procedures completed on one day which have a **Historical Production** amount greater than 0, while also not being comprised of procedure codes signifying a missed appointment. If one or more remaining procedures occur for a patient then that will be associated to one patient visit for that day. Of special note, this metric should include patients with both an Active and Inactive status, as well as patients marked as deleted.

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * uses COUNT( DISTINCT patientId, entryDate )
  * see [Historical Production](#historical-production)
  * procedureCode NOT IN `MissedAppointmentCodes` (defined below)
  * totalAmount > 0
* Patient
  * note: status can be Active or Inactive
  * note: deletedAt can be null
* AccountConfiguration
  * APPOINTMENT_MISSED_CUSTOM_KEY for the accountId defines the values identified as `MissedAppointmentCodes`
</details>

<details>
  <summary>Usages:</summary>

### Dashboard
### Reporting
* Practice Performance 
  * Total Patient Visits
    * Drill down
* Provider Performance
  * Patient Visits
    * Drill down
  * Re-Appointment % (denominator)
    * Drill down

</details>

## Today's Patient Visits
All patient visits that have occurred on the current day.

> Note: derived from **Historical Patient Visits** but specifically scoped to the current day

<details>
<summary>Technical Details:</summary>

* see [Historical Patient Visits](#historical-patient-visits)
* DeliveredProcedure
  * see [Today's Completed Production](#todays-completed-production)
</details>

<details>
  <summary>Usages:</summary>

#### Dashboard
#### Reporting

</details>

## Historical Patient Visits - Hygiene
> Comprised of **Historical Patient Visits** where the visit is categorized as a hygiene visit

<details>
<summary>Technical Details:</summary>

* see [Historical Production](#historical-production)
* DeliveredProcedure
  * at least one record matches the _procedureCode_:
    * 111*
    * 4342*
    * 00121
    * D11*
    * D434*
    * D4910
    * D0120
    * matches a value present in `HygieneTypes` (defined below)
    * matches a value present in `HygieneCodes` (defined below)
  * OR at least one record has a _reason_ which:
    * contains a value present in `HygieneTypes` within the reason (defined below)
    * contains a value present in `HygieneCodes` within the reason (defined below)
* Practitioners
  * type is 'Hygienist'
* AccountConfiguration
  * HYGIENE_TYPES for the accountId defines the values identified as `HygieneTypes`
  * HYGIENE_PROCEDURE_CODES for the accountId defines the values identified as `HygieneCodes`

</details>

<details>
  <summary>Usages:</summary>

#### Dashboard
#### Reporting
* Practice Performance
  * Total Hygiene Visits
    * Drill down
  * Hygiene Visits Re-appointed (denominator)
    * Drill down
  * Hygiene Re-appointment % (denominator)
    * Drill down
* Provider Performance
  * Re-Appointment % (Hyg) (denominator)
</details>

## Today's Patient Visits - Hygiene
**Today's Patient Visits** where the visit is categorized as a hygiene visit

> Note: derived from ** Historical Patient Visits - Hygiene** but specifically scoped to the current day

<details>
<summary>Technical Details:</summary>

* see [Historical Patient Visits - Hygiene](#historical-patient-visits-hygiene)
* DeliveredProcedure
  * see [Today's Completed Production](#todays-completed-production)
</details>

<details>
  <summary>Usages:</summary>

#### Dashboard
#### Reporting

</details>

## Historical Patient Visits Re-Appointed
> Comprised of **Historical Patient Visits** where the patient has scheduled another appointment on a future day.

<details>
<summary>Technical Details:</summary>

* see [Historical Patient Visits](#historical-patient-visits)
* Patients
  * nextApptId is not null
</details>
SPS Continue here
<details>
  <summary>Usages:</summary>

#### Dashboard
#### Reporting
* Practice Performance
  * New Patient Visits
</details>

## Historical Patient Visits - Hygiene
> Comprised of **Historical Patient Visits** where the visit is categorized as a hygiene visit

<details>
<summary>Technical Details:</summary>

* see [Historical Production](#historical-production)
* DeliveredProcedure
  * at least one record matches the _procedureCode_:
    * 111*
    * 4342*
    * 00121
    * D11*
    * D434*
    * D4910
    * D0120
    * matches a value present in `HygieneTypes` (defined below)
    * matches a value present in `HygieneCodes` (defined below)
  * OR at least one record has a _reason_ which:
    * contains a value present in `HygieneTypes` within the reason (defined below)
    * contains a value present in `HygieneCodes` within the reason (defined below)
* Practitioners
  * type is 'Hygienist'
* AccountConfiguration
  * HYGIENE_TYPES for the accountId defines the values identified as `HygieneTypes`
  * HYGIENE_PROCEDURE_CODES for the accountId defines the values identified as `HygieneCodes`

</details>

<details>
  <summary>Usages:</summary>

#### Dashboard
#### Reporting
* Practice Performance
  * Total Hygiene Visits
    * Drill down
  * Hygiene Visits Re-appointed (denominator)
    * Drill down
  * Hygiene Re-appointment % (denominator)
    * Drill down
* Provider Performance
  * Re-Appointment % (Hyg) (denominator)
</details>

SPS Continue here
***

## Scheduled Production
Uses the monetary amount attached to upcoming delivered procedures and appointments for the **current day and days in the future**.

> Note: Only dates including the current day and any in the future are included in the Scheduled Production metric.

> Note: In Tracker these models are independent and as such both are counted; therefore by convention both cannot be entered for the same upcoming visit or it will result in double-counting.

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * uses totalAmount
  * entryDate must be current day or in the future
  * isCompleted is ignored
  * deletedAt must be null
  * in rare cases when duplicate entries exist for one pmsId, then the maximum totalAmount present across those records is used in the evaluation
  * `Note: isDeletedFromPms is not evaluated at this point`
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

## Live Scheduled Production
_name TBD_
Compliment to **Today's Completed Production** which captures **Scheduled Production** that has yet to be processed on the current day and in the future.
> **Scheduled Production** _minus_ 'estimate procedures which were completed today' 

<details>
<summary>Technical Details:</summary>

* see [Scheduled Production](#scheduled-production)
* excludes procedures that would now be captured in **Today's Completed Production**
</details>

<details>
  <summary>Usages:</summary>

#### Dashboard
#### Reporting
</details>

***

## Expected Production
The sum of all production which was delivered as well as estimates of all of the upcoming production to yet be performed. Production occurring on the current day is captured only as an estimate of what is to be performed.

> **Historical Production** + **Scheduled Production**

<details>
<summary>Technical Details:</summary>

* see [Historical Production](#historical-production)
* see [Scheduled Production](#scheduled-production)
</details>

<details>
  <summary>Usages:</summary>

#### Dashboard
* Next 3 Days Schedule (to be verified)
#### Reporting
* Practice Performance
  * Month Forecast
  * Next Month Forecast
* Provider Performance
  * Month Forecast
  * Next Month Forecast

</details>

Formerly known as:
* Month Forecast

## Live Expected Production
_name TBD_
The sum of all production which was delivered as well as estimates of all of the upcoming production to yet be performed. Unlike **Expected Production**, procedures delivered on the present day are captured in this metric.

> **Live Completed Production** + **Live Scheduled Production**

<details>
<summary>Technical Details:</summary>

**Historical Production** + **Scheduled Production**

* see [Live Completed Production](#live-completed-production)
* see [Live Scheduled Production](#live-scheduled-production)
</details>

<details>
  <summary>Usages:</summary>

#### Dashboard
#### Reporting
</details>

