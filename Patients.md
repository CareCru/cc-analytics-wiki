# Patient Metrics

Patients can be considered on several different dimensions such as their Engagement Type or Active Type. During reporting, patient values will count distinct patients during that range; so a patient should only count 1 time for that metric during the time period. 

**It is important to note that ALL mentions of a visit on this page refer to a visit that was completed, was not a missed appointment, and had a production amount > 0.**

> See below for a list of refined variations of this metric

## New Patient
Patient that had a [New Patient Visit](/CareCru/analytics-service/wiki/Visits#new-patient-visits) during the time period, regardless of when the profile was created.

TBD: should we account for deletedAt?

> **Engagement Metrics***
> 
> Engagement metrics are defined purely based on the characteristics of the patient's visit history. 

_Note that if a patient could have the qualifications for multiple engagement types then only one is attributed based on the following hierarchy_:
* New Patient
* Disengaging Patient 
* Re-Engaged Patient
* Engaged Patient
* Disengaged Patient

## Re-Engaged Patient
Patient that had a [Re-Engaged Patient Visit](/CareCru/analytics-service/wiki/Visits#re-engaged-patient-visits) during the time period.

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
Patients whose last visit is newer than 18 months prior to the start date of the query AND the last visit is 18 months prior to the end date of the query.

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

