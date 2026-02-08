# Infrastructure Jobs Reproduction

This workflow is intended to reproduce the infrastructure jobs for the FiveThirtyEight project, done in R. This workflow is illustrative of combining previously normalized data from a relational database that was exported table-wise without the explicit foreign key relationships. The workflow is designed to be flexible and can be adapted to other similar datasets that have been exported in a similar manner.

## Pre-roundup

1. Download the data in the browser
   - http://download.bls.gov/pub/time.series/sm/sm.data.62.Construction.Current -> sm.data.62.Construction.Current.tsv
   - http://download.bls.gov/pub/time.series/sm/sm.series -> sm.series.tsv
   - http://download.bls.gov/pub/time.series/sm/sm.area -> sm.industry.tsv

## Roundup

1. Upload the four files:
   1. `sm.data.62.Construction.Current.tsv`
   2. `sm.series.tsv`
   3. `sm.industry.tsv`
   4. `payroll-state.csv` (from the original repo)

2. Rename the files: We can use more semantic names for files, instead of the original file names, which are not very descriptive. The new names should reflect the content of the files and make it easier to understand what they contain at a glance. Here are the suggested new names for the files:
   1. `sm.data.62.Construction.Current.tsv` -> `state pay raw`
   2. `sm.series.tsv` -> `series raw`
   3. `sm.industry.tsv` -> `industry raw`
   4. `payroll-state.csv` -> `state`

3. Create a pack operation with `series` and `state`, using the `state_code` columns in each table as keys, keep all matches (FULL OUTER JOIN)
4. Pack the previous pack operation with the `industry` table, using the `industry_code` columns in each table as keys, keep all matches (FULL OUTER JOIN)
5. Pack the previous pack operation with the `state pay raw` table, using the `series_id` columns in each table as keys, keep all matches (FULL OUTER JOIN)
6. Export file for downstream analysis
