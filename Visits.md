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
* Provider Performance - Aggregated
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

## New Patient Visits
> Comprised of **Historical Patient Visits** where the patient's previous visit was not within 18 months.

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * uses COUNT( DISTINCT patientId, entryDate )
  * see [Historical Production](#historical-production)
  * procedureCode NOT IN `MissedAppointmentCodes` (defined below)
  * totalAmount > 0
  * having NO DeliveredProcedure prevDP such that
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

## Engaged Patient Visits
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

## Re-Engaged Patient Visits
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

