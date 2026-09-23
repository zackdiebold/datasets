# Dataset: EOIR State-Month Panel

## Overview

This dataset contains monthly state-level aggregates of U.S. immigration court proceedings derived from EOIR proceeding-level records.

It was constructed to study in absentia outcomes, detention status, legal representation, transfers, recent address changes, selected nationality groups, removal proceedings, and state adoption of driver's license or driving privilege card policies.

The primary research outcome is the in absentia rate among non-detained, unrepresented removal proceedings.

## Contents

State-month panel data containing:

- Total completed proceedings
- In absentia proceedings
- Non-detained proceedings
- Unrepresented proceedings
- Transferred proceedings
- Proceedings with a recent address change
- Proceedings involving nationals of Mexico, Guatemala, El Salvador, and Honduras
- Removal proceedings (`RMV`)
- Unrepresented removal proceedings
- Representation, transfer, and recent-address-change rates
- In absentia rates for each subgroup
- State DPC policy adoption month
- Treatment status
- Months relative to treatment

Each row represents one state-month.

## Format

`eoir_state_month_panel.csv`

```csv
state,month,total_proceedings,absentia_count,non_detained_total,non_detained_absentia,unrepresented_total,unrepresented_absentia,transferred_total,transferred_absentia,recent_address_change_total,recent_address_change_absentia,mx_nca_total,mx_nca_absentia,rmv_total,rmv_absentia,rmv_unrepresented_total,rmv_unrepresented_absentia,represented_total,transfer_count,recent_address_change_count,absentia_rate,non_detained_absentia_rate,unrepresented_absentia_rate,transferred_absentia_rate,recent_address_change_absentia_rate,mx_nca_absentia_rate,rmv_absentia_rate,rmv_unrepresented_absentia_rate,representation_rate,transfer_rate,recent_address_change_rate,adoption_month,treated,event_month
```

Example:

```csv
state,month,total_proceedings,absentia_count,non_detained_total,non_detained_absentia,unrepresented_total,unrepresented_absentia,transferred_total,transferred_absentia,recent_address_change_total,recent_address_change_absentia,mx_nca_total,mx_nca_absentia,rmv_total,rmv_absentia,rmv_unrepresented_total,rmv_unrepresented_absentia,represented_total,transfer_count,recent_address_change_count,absentia_rate,non_detained_absentia_rate,unrepresented_absentia_rate,transferred_absentia_rate,recent_address_change_absentia_rate,mx_nca_absentia_rate,rmv_absentia_rate,rmv_unrepresented_absentia_rate,representation_rate,transfer_rate,recent_address_change_rate,adoption_month,treated,event_month
AK,1990-01-01,6,0,5,0,0,0,0,0,0,0,3,0,0,0,0,0,5,0,0,0,0,NA,NA,NA,0,NA,NA,1,0,0,NA,0,NA
```

## Key Variables

- `state`: Two-letter state or District of Columbia code.
- `month`: Month in which the proceeding was completed.
- `total_proceedings`: Total number of completed proceedings in the state-month.
- `absentia_count`: Number of proceedings completed in absentia.
- `non_detained_total`: Number of non-detained proceedings.
- `unrepresented_total`: Number of non-detained proceedings without identified representation.
- `rmv_total`: Number of non-detained proceedings with case type `RMV`.
- `rmv_unrepresented_total`: Number of non-detained, unrepresented removal proceedings.
- `rmv_unrepresented_absentia`: Number of non-detained, unrepresented removal proceedings completed in absentia.
- `rmv_unrepresented_absentia_rate`: In absentia rate among non-detained, unrepresented removal proceedings.
- `adoption_month`: First full month of the state's DPC policy implementation.
- `treated`: Indicator equal to 1 for state-months on or after policy implementation and 0 otherwise.
- `event_month`: Number of months relative to implementation. `0` is the implementation month, negative values are pre-treatment, and positive values are post-treatment.

## Rate Definitions

Rate variables are stored as decimal proportions between 0 and 1.

```text
absentia_rate =
    absentia_count / total_proceedings

non_detained_absentia_rate =
    non_detained_absentia / non_detained_total

unrepresented_absentia_rate =
    unrepresented_absentia / unrepresented_total

transferred_absentia_rate =
    transferred_absentia / transferred_total

recent_address_change_absentia_rate =
    recent_address_change_absentia / recent_address_change_total

mx_nca_absentia_rate =
    mx_nca_absentia / mx_nca_total

rmv_absentia_rate =
    rmv_absentia / rmv_total

rmv_unrepresented_absentia_rate =
    rmv_unrepresented_absentia / rmv_unrepresented_total

representation_rate =
    represented_total / non_detained_total

transfer_rate =
    transfer_count / non_detained_total

recent_address_change_rate =
    recent_address_change_count / non_detained_total
```

Rates are recorded as `NA` when the corresponding denominator is zero.

## Source

The panel was constructed from EOIR administrative case and proceeding records.

Proceeding-level data were cleaned and collapsed to the state-month level using the proceeding completion date.

Representation information was incorporated from EOIR representation records.

DPC policy implementation dates were merged from a separate state policy implementation dataset.

## Notes

- Counts represent proceedings, not necessarily unique individuals or unique immigration cases.
- A single individual may appear in more than one proceeding.
- Non-detained status is based on the EOIR proceeding custody classification used during data construction.
- Unrepresented status indicates that no representation record was identified by the processing procedure.
- A transferred proceeding is defined as having at least one recorded transfer.
- `transfer_count` in this dataset is a count of proceedings with a transfer, not the total number of transfer events.
- A recent address change is defined as a recorded address change occurring within 180 days before proceeding completion.
- `mx_nca` includes nationality codes for Mexico, Guatemala, El Salvador, and Honduras.
- `rmv` refers to proceedings with EOIR case type `RMV`.
- Historical coverage of the `RMV` classification should be interpreted cautiously in early years.
- `adoption_month` is missing for states without a recorded DPC implementation date.
- Never-treated states have `treated = 0` and `event_month = NA` throughout the panel.
- The primary research outcome is `rmv_unrepresented_absentia_rate`.
