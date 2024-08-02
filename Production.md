# Production Metrics

Total value of all dental services provided by a practice which includes all billable services rendered to patients, such as examinations, cleanings, fillings, and other dental procedures.

> See below for a list of refined variations of this metric

## Historical Production
All dental procedures that have been supplied and billed to the client and have occurred before the current day.

> Note: missed appointments are included

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * entryDate must be prior to the current day
  * deletedAt must be null
  * isCompleted must be true
    * `EXCEPTION: the following release ignore isCompleted: Tracker before 3.21.0, `
  * in rare cases when duplicate entries exist for one pmsId, then the maximum totalAmount present across those records is used in the evaluation
  * `Note: isDeletedFromPms is not evaluated at this point`
</details>

<details>
  <summary>Usages:</summary>

### Dashboard
* 30 Days Average
### Reporting
* Production (Huron only so far)
  * Total Production
  * Practice Production by Type
    * Total Production (segregated by type)
  * Practitioner Production 
    * Total Production (segregated by practitioner)
* Practice Performance 
  * Billed Production
* Provider Performance
  * Billed Production

</details>

Formerly known as:
* Production MTD

## Today's Completed Production
All dental procedures that have been supplied and billed to the client on the current day and updated in real-time.

> Note: derived from **Historical Production** but specifically scoped to the current day

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * see definition in **Historical Production** except:
    * entryDate must be the current day
</details>

<details>
  <summary>Usages:</summary>

#### Dashboard
* Today's Billed Production
#### Reporting

</details>

## Live Completed Production
> **Historical Production** + **Today's Completed Production**

<details>
<summary>Technical Details:</summary>

* see [Historical Production](#historical-production)
* see [Today's Completed Production](#todays-completed-production)
</details>

<details>
  <summary>Usages:</summary>

#### Dashboard
* Month Gross
* Production Bar
* YTD Production
#### Reporting
</details>

Formerly known as:
* Production

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
#### Reporting
* Practice Performance
  * Scheduled Production
* Provider Performance
  * Scheduled Production
</details>

## Today's Scheduled Production
Includes all **Scheduled Production** that is expected to occur on the current day; however, it does not update within the day when a procedure has been completed.
> Real-time gathering of **Scheduled Production** (including procedures also in **Today's Completed Production**)

<details>
<summary>Technical Details:</summary>

* see [Scheduled Production](#scheduled-production)
* calculated in real-time 
* still includes procedures that are also captured in **Today's Completed Production**
</details>

<details>
  <summary>Usages:</summary>

#### Dashboard
* Today's Scheduled (to be verified)
#### Reporting
</details>


## Live Scheduled Production
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
* Next X Days Schedule (to be verified)
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


