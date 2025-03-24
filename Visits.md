# Visit Metrics

## Patient Visits
Aggregation of multiple delivered procedures completed on one day which have a **Historical Production** amount greater than 0, while also not being comprised of procedure codes signifying a missed appointment. If one or more remaining procedures occur for a patient then that will be associated to one patient visit for that day. 
> Includes patients with both an Active and Inactive status, as well as patients marked as deleted.

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * uses COUNT( DISTINCT patientId, entryDate )
  * see [Billed Production](./Production.md#billed-production)
  * procedureCode NOT IN `MissedAppointmentCodes` (defined below)
* Patient
  * note: status can be Active or Inactive
  * note: deletedAt can be null
* AccountConfiguration
  * APPOINTMENT_MISSED_CUSTOM_KEY for the accountId defines the values identified as `MissedAppointmentCodes`
</details>

---

## Patient Visits - Hygiene
Comprised of **Patient Visits** where the visit is categorized as a hygiene visit.

<details>
<summary>Technical Details:</summary>

* see [Billed Production](./Production.md#billed-production)
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

## Patient Visits - Re-Appointed
Comprised of **Patient Visits** where the patient has scheduled another appointment on a future day.

<details>
<summary>Technical Details:</summary>

* see [Patient Visits](#patient-visits)
* Patients
  * nextApptId is not null
</details>

---

## Patient Visit Types
> Consists as one of **New Patient Visit**, **Engaged Patient Visit**, or **Re-Engaged Patient Visit**

### New Patient Visits
Comprised of **Patient Visits** where the patient has no prior **Patient Visit**.
<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * see [Patient Visit](#patient-visits)
  * ALSO: this record is first [Patient Visit](#patient-visits) for this patient
</details>

### Engaged Patient Visits
Comprised of **Patient Visits** where the patient has a prior **Patient Visit** within 18 months of the visit in question.

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * see [Patient Visit](#patient-visits)
  * ALSO: another record exists within 18 months prior to this visit which also qualifies as a [Patient Visit](#patient-visits)
</details>

### Re-Engaged Patient Visits
Comprised of **Patient Visits** where the patient has a prior **Patient Visit** but that most recent visit is not within 18 months of the visit in question.

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * see [Patient Visit](#patient-visits)
  * ALSO: no record exists within 18 months prior to this visit which also qualifies as a [Patient Visit](#patient-visits), but this is not the first valid visit
</details>
