# Patient Metrics

Patients can be considered on several different dimensions such as their Engagement Type or Active Type. During reporting, patient values will count distinct patients during that range; so a patient should only count 1 time for that metric during the time period. 

**It is important to note that ALL mentions of a visit on this page refer to a visit that was completed, was not a missed appointment, and had a production amount > 0.**

> See below for a list of refined variations of this metric

## New Patient
Patient that had their first visit during the time period, regardless of when the profile was created.
TBD: should we account for deletedAt?

> Engagement Metrics
> Engagement metrics are defined purely based on the characteristics of the patient's visit history. 

_Note that if a patient could have the qualifications for multiple engagement types then only one is attributed based on the following hierarchy:
* New Patient
* Disengaging Patient 
* Re-engaged Patient
* Engaged Patient
* Disengaged Patient

## Re-engaged Patient
Patient that had a Re-Engaged Visit during the time period.

## Engaged Patient
Aggregate of all unique patients that have had at least one visit within the last 18 months. (SPS-TODO: add link to visit definition)

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * SPS TODO
  * uses COUNT( DISTINCT patientId )
  * see [Historical Production](#historical-production)
  * procedureCode NOT IN `MissedAppointmentCodes` (defined below)
  * totalAmount > 0
* Patient
  * note: status can be Active or Inactive
  * note: deletedAt can be set
* AccountConfiguration
  * APPOINTMENT_MISSED_CUSTOM_KEY for the accountId defines the values identified as `MissedAppointmentCodes`
</details>

<details>
  <summary>Usages:</summary>

### Dashboard
### Reporting

</details>

## Disengaging Patient
Patients whose last visit is between 17 and 18 months of the end date of the query.

## Disengaged Patient
Patient has not had a visit in longer than 18 months.

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
* Provider Performance - Aggregated
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
* Provider Performance - Aggregated
  * Re-Appointment % (Hyg) (denominator)

</details>

## Engaged Patient Visits - Hygiene
> Comprised of **Historical Patient Visits** where the patient's previous visit was within 18 months.

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * uses COUNT( DISTINCT patientId, entryDate )
  * see [Historical Production](#historical-production)
  * procedureCode NOT IN `MissedAppointmentCodes` (defined below)
  * totalAmount > 0
  * having DeliveredProcedure prevDP 
    * prevDP.patientId, prevDp.entryDate >= entryDate - 18 months
    * prevDP.procedureCode NOT IN `MissedAppointmentCodes` (defined below)
    * prevDP.totalAmount > 0
* Patient
  * note: status can be Active or Inactive
  * note: deletedAt can be set
* AccountConfiguration
  * APPOINTMENT_MISSED_CUSTOM_KEY for the accountId defines the values identified as `MissedAppointmentCodes`
</details>

<details>
  <summary>Usages:</summary>
#### Dashboard
#### Reporting
* Patient Visits
  * SPS - TBD
</details>

## Re-Engaged Patient Visits - Hygiene
> Comprised of **Historical Patient Visits** where the patient's previous visit was not within 18 months.

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * uses COUNT( DISTINCT patientId, entryDate )
  * see [Historical Production](#historical-production)
  * procedureCode NOT IN `MissedAppointmentCodes` (defined below)
  * totalAmount > 0
  * having DeliveredProcedure prevDP 
    * prevDP.patientId, prevDp.entryDate < entryDate - 18 months
    * prevDP.procedureCode NOT IN `MissedAppointmentCodes` (defined below)
    * prevDP.totalAmount > 0
* Patient
  * note: status can be Active or Inactive
  * note: deletedAt can be set
* AccountConfiguration
  * APPOINTMENT_MISSED_CUSTOM_KEY for the accountId defines the values identified as `MissedAppointmentCodes`
</details>

<details>
  <summary>Usages:</summary>
#### Dashboard
#### Reporting
* Patient Visits
  * SPS - TBD
</details>
