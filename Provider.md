# Provider Metrics

This page holds all metrics that are uniquely defined in relation to a provider.  Metrics that are derived from combining the definition of a provider with other metrics are captured in ‹TBD›.

## Active Practice Providers
Within a defined time period, includes all providers which exist in that period and currently have isActive as true.

<details>
<summary>Technical Details:</summary>

* Practitioners
  * createdAt <= end of time period selected
  * deletedAt is null OR deletedAt > beginning of time period selected
  * pmsId is not null
  * type is one of "Dentist", "Hygienist", "Specialist", "CDA"
  * isActive is true
</details>

## Productive Practice Providers
A more generalized definition for finding relevant providers; Within a defined time period, includes all providers which have isActive as true OR have an Expected Production amount greater than 0.

> Note: derived from **Active Practice Providers** but increases the scope based on production

<details>
<summary>Technical Details:</summary>

* DeliveredProcedure
  * see definition in **Active Practice Providers** except:
    * ~~isActive is true~~
    * isActive is true OR has positive, non-zero **Expected Production** within the time range
    * ~~pmsId is not null~~
    * any pmsId is accepted
    * ~~type is one of "Dennis", "Hygienist", "Specialist", "CDA"~~
    * any type is accepted
    * ~~createdAt <= end of time period selected~~
    * `firstDeliveredProcedure` <= end of time period selected (see below for `firstDeliveredProcedure`)
    * `firstDeliveredProcedure` uses MIN(entryDate) to provide a substitute for invalid createdAt entries
    * `note: no other restrictions are placed on finding the min value as it is meant for a lazy substitution`
</details>
