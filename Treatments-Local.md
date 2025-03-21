# Treatment Metrics

Treatments have a nuanced differen

> See below for a list of refined variations of this metric

## Total Hours Scheduled
The summation of time (in hours) for all appointments within a given day, for a specific provider. Situations where multiple appointments exists
in parallel for a provider are all included in the calculation.

> Note: there are known issues with the way that this metric is currently calculated as detailed in Technical Details

<details>
<summary>Technical Details:</summary>

* Practitioners
    * isActive must be true
        * » note that old calculations are not removed; so inactive providers will have an out-dated value until they are active again
    * deletedAt must be null
        * » note that old calculations are not removed; so deleted providers will have an out-dated value until they are active again
* Appointments
    * uses estimatedRevenue
    * deletedAt must be null
    * isMissed must be false
    * isCancelled is false
    * isDeleted must be false
    * isPending must be false
    * duration - calculation using:
        * startDate
        * endDate
          Output to:
* PractitionerScheduledHours
    * totalTimeScheduled
</details>

<details>
  <summary>Usages:</summary>

### Dashboard
### Reporting
</details>

## Total Hours Spent
The summation of the duration (in hours) whereby a practitioner is booked in one or more appointments. In other words, for appointments
that have an intersection of time, then the output is the union of the hours and not the summation of the 2 appointments.

> Note: there are known issues with the way that this metric is currently calculated as detailed in Technical Details

<details>
<summary>Technical Details:</summary>

* Practitioners
    * isActive must be true
        * » note that old calculations are not removed; so inactive providers will have an out-dated value until they are active again
    * deletedAt must be null
        * » note that old calculations are not removed; so deleted providers will have an out-dated value until they are active again
* Appointments
    * uses estimatedRevenue
    * deletedAt must be null
    * isMissed must be false
    * isCancelled is false
    * isDeleted must be false
    * isPending must be false
    * duration - calculation using:
        * startDate
        * endDate
        * » known bug: if Appt-A is from 9:00 - 11:00 and Appt-B is completely inclusive at 10:00 - 10:30; Expected Output: 2 hours; Actual output: 1.5 hours
          Output to:
* PractitionerScheduledHours
    * totalTimeSpent
</details>

<details>
  <summary>Usages:</summary>

### Dashboard
### Reporting
* Practice Performance
    * Hourly Production (DDS)
    * Hourly Production (Hyg)
* Provider Performance
    * Hourly Production
* Provider Performance - Aggregated
    * Hourly Production
</details>

