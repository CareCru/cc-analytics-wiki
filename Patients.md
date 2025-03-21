# Patient Metrics

Patients can be considered on several different dimensions such as their Engagement Type or Active Type. During reporting, patient values will count distinct patients during that range; so a patient should only count 1 time for that metric during the time period. 

**It is important to note that ALL mentions of a visit on this page refer to a visit that was completed, was not a missed appointment, and had a production amount > 0.**

## CC Active Patient 
Patient who is an **Engaged Patient** and has a PMS patient status of 'Active' at the end of the reporting period indicated.

---

## Patient Engagment Type
> Consists as one of **New Patient**, **Engaged Patient**, or **Re-Engaged Patient**

_Note that if a patient could have the qualifications for multiple engagement types then only one is attributed based on the following hierarchy_:
* New Patient
* Re-Engaged Patient
* Engaged Patient

### New Patient
Patient that was created within the time period of the report.

<details>
<summary>Technical Details:</summary>

* see [New Patient Visit](/CareCru/analytics-service/wiki/Visits#new-patient-visits
  * is **New Patient** if a **New Patient Visit** is within the time period
</details>

### Re-Engaged Patient
Patient that was not created within the time period, had at least one [Patient Visit](/CareCru/analytics-service/wiki/Visits#patient-visits) which occurred at least 18 months from the previous one, and is active at the end of the time period.

<details>
<summary>Technical Details:</summary>

* see [Re-Engaged Patient Visit](/CareCru/analytics-service/wiki/Visits#re-engaged-patient-visits
  * has **Re-Engaged Patient Visit** within the time period of the report
</details>

### Engaged Patient
Patient that was not created within the time period and had one or more [Engaged Patient Visits](/CareCru/analytics-service/wiki/Visits#engaged-patient-visits) during the reporting time period.

<details>
<summary>Technical Details:</summary>

* see [Engaged Patient Visit](/CareCru/analytics-service/wiki/Visits#engaged-patient-visits
  * has **Engaged Patient Visit** within the time period of the report
</details>
