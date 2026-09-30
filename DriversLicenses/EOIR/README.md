# Dataset: EOIR State-Month DPC Panel

## Overview

`EOIR_DPC_panel.csv` is a state-month panel of U.S. immigration court proceedings derived from Executive Office for Immigration Review (EOIR) administrative records and merged with state driving privilege card (DPC) policy timing plus annual state-level economic, demographic, and immigration-enforcement covariates.

The dataset was constructed to study whether state access to driver's licenses or driving privilege cards is associated with changes in immigration-court **in absentia** outcomes.

Each row represents one state-month. Counts refer to proceedings, not necessarily unique people or unique immigration cases.

## Coverage

The EOIR panel spans **1990-2024**.

Covariate coverage varies by source:

- **State unemployment:** annual BLS Local Area Unemployment Statistics series retrieved through FRED.
- **Population and noncitizen measures:** ACS-based and populated beginning in 2005 in the current research covariate file.
- **Secure Communities, 287(g), and detainer measures:** populated only where the underlying enforcement data support state-year construction.

Covariates are  left missing outside their source coverage.


## Primary Research Outcomes

The project uses two principal removal-proceeding outcomes.

### All non-detained removal proceedings

```text
rmv_absentia_rate =
    rmv_absentia / rmv_total
```

### Non-detained, unrepresented removal proceedings

```text
rmv_unrepresented_absentia_rate =
    rmv_unrepresented_absentia / rmv_unrepresented_total
```

The unrepresented outcome is the narrower population originally motivating the project. The broader `rmv_absentia_rate` has substantially stronger state-time cell support and is used in the main national analysis.

## EOIR Variables

### Identifiers

- `state`: Two-letter state or District of Columbia code.
- `month`: Month in which the proceeding was completed.
- `year`: Calendar year extracted from `month`.

### Proceeding counts

- `total_proceedings`: Total completed proceedings in the state-month.
- `absentia_count`: Proceedings completed in absentia.
- `non_detained_total`: Non-detained proceedings.
- `non_detained_absentia`: Non-detained proceedings completed in absentia.
- `unrepresented_total`: Non-detained proceedings without identified representation.
- `unrepresented_absentia`: Unrepresented proceedings completed in absentia.
- `transferred_total`: Proceedings with at least one recorded transfer.
- `transferred_absentia`: Transferred proceedings completed in absentia.
- `recent_address_change_total`: Proceedings with a recorded address change within 180 days before completion.
- `recent_address_change_absentia`: Recent-address-change proceedings completed in absentia.
- `mx_nca_total`: Proceedings involving nationals of Mexico, Guatemala, El Salvador, or Honduras.
- `mx_nca_absentia`: Those proceedings completed in absentia.
- `rmv_total`: Non-detained proceedings with EOIR case type `RMV`.
- `rmv_absentia`: Non-detained `RMV` proceedings completed in absentia.
- `rmv_unrepresented_total`: Non-detained, unrepresented `RMV` proceedings.
- `rmv_unrepresented_absentia`: Non-detained, unrepresented `RMV` proceedings completed in absentia.
- `represented_total`: Non-detained proceedings classified as represented.
- `transfer_count`: Count of proceedings with at least one recorded transfer.
- `recent_address_change_count`: Count of proceedings with a recent address change.

### EOIR rates

All rate variables are decimal proportions between 0 and 1. Rates are `NA` when the corresponding denominator is zero.

```text
absentia_rate = absentia_count / total_proceedings
non_detained_absentia_rate = non_detained_absentia / non_detained_total
unrepresented_absentia_rate = unrepresented_absentia / unrepresented_total
transferred_absentia_rate = transferred_absentia / transferred_total
recent_address_change_absentia_rate = recent_address_change_absentia / recent_address_change_total
mx_nca_absentia_rate = mx_nca_absentia / mx_nca_total
rmv_absentia_rate = rmv_absentia / rmv_total
rmv_unrepresented_absentia_rate = rmv_unrepresented_absentia / rmv_unrepresented_total
representation_rate = represented_total / non_detained_total
transfer_rate = transfer_count / total_proceedings
recent_address_change_rate = recent_address_change_count / total_proceedings
```

## DPC Treatment Variables

- `adoption_month`: First full month of the state's DPC policy implementation.
- `treated`: Indicator equal to 1 on or after `adoption_month` and 0 otherwise.
- `event_month`: Integer number of months relative to implementation. `0` is the implementation month, negative values are pre-treatment, and positive values are post-treatment.

Never-treated states have `treated = 0` and `event_month = NA` throughout the panel.

Quarterly analyses use the first full treated quarter when implementation occurs during a quarter.

## Economic and Demographic Covariates

- `unemployment_rate`: Annual state unemployment rate from the Bureau of Labor Statistics Local Area Unemployment Statistics program, retrieved through FRED.
- `population`: State population from the American Community Survey.
- `noncitizen_population`: ACS estimate of the state noncitizen population.
- `noncitizen_share`: `noncitizen_population / population`.

Annual covariates are merged at the state-year level and repeated across months within each state-year.

## Immigration-Enforcement Covariates

- `secure_communities_pop_share`: Share of the state population living in counties coded as exposed to Secure Communities in that state-year.
- `g287_pop_share`: Share of the state population living in counties covered by an active 287(g) agreement in that state-year.
- `any_287g`: Indicator equal to 1 when any 287(g) exposure is active in the state-year.
- `detainer_count`: County-level ICE detainers summed to the state-year level.
- `detainer_rate_per_100k_noncitizens`: `detainer_count / noncitizen_population * 100000`.
- `log_detainer_rate`: `log(1 + detainer_rate_per_100k_noncitizens)`.

Additional construction-diagnostic columns may be retained when available, such as counts of exposed counties, counties with population data, or counties contributing detainer observations.

## Data Construction

The combined panel merges:

1. EOIR state-month outcomes and DPC timing
2. State-year economic and demographic covariates
3. State-year immigration-enforcement covariates

Missing annual covariates remain `NA`. Missing source coverage is not recoded as zero unless the underlying source definition justifies zero exposure.

## Sources

The panel draws on the following sources:

- **EOIR administrative records:** U.S. Department of Justice, Executive Office for Immigration Review, FOIA Library and EOIR Case Data.  
  [EOIR FOIA Library](https://www.justice.gov/eoir/foia-library-0)

- **State DPC implementation dates:** National Conference of State Legislatures, *States Offering Driver’s Licenses to Immigrants*. The implementation dates in this panel were compiled from the effective dates reported by NCSL.  
  [NCSL: States Offering Driver’s Licenses to Immigrants](https://www.ncsl.org/immigration/states-offering-drivers-licenses-to-immigrants)

- **State unemployment:** U.S. Bureau of Labor Statistics Local Area Unemployment Statistics, retrieved through FRED, Federal Reserve Bank of St. Louis. The annual state unemployment series follow the pattern `LAUSTss0000000000003A`, where `ss` is the two-digit state FIPS code.  
  [Example FRED series: Alabama](https://fred.stlouisfed.org/series/LAUST010000000000003A)

- **Population and noncitizen measures:** U.S. Census Bureau American Community Survey state-level population and citizenship-status estimates.

- **Secure Communities and 287(g) policy data:** replication materials for *Reconsideration of Secure Communities Rollout Reveals Preemptive Local-Federal Cooperation in Immigration Enforcement* by Vargas-Núñez et al.  
  [Vargas-Núñez et al. replication materials](https://github.com/asadlasad/vargasnunezetal_2026_pnas)

- **Detainer data:** Baumer, Eric P., and Min Xie. *Illegal Immigration, Immigration Enforcement Policies, and American Citizens' Victimization Risk, [United States], 2005-2015* (ICPSR 39329). County-level detainer data were aggregated to the state-year level for this project.  
  [ICPSR 39329](https://www.icpsr.umich.edu/web/NACJD/studies/39329)  
  DOI: `10.3886/ICPSR39329.v1`