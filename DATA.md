# Data in this repository

Everything under `data/raw/` is real, public data, committed on purpose and **never edited**. Where it came from, its license, and exactly what was changed for this course:

## Online Retail II (UCI), sheet “Year 2009-2010”

**Source:** Daqing Chen, *Online Retail II*, UCI Machine Learning Repository, https://doi.org/10.24432/C5CG6D —
`https://archive.ics.uci.edu/dataset/502/online+retail+ii (sheet 'Year 2009-2010')` (fetched 2026-09-19, SHA-256 `17181af53059386e…`).

**License:** **CC BY 4.0** (as stated on the UCI dataset page).

**What it is:** every invoice line of a UK-based online retailer of gifts, 1 December 2009 to 9 December 2010:
525,461 rows, one row per product on an invoice. The UCI page documents that an invoice number starting with `C`
is a cancellation. It does not say why some lines have no `Customer ID`.

**Changes made for this course:** the 2009–2010 sheet only (the second sheet, 2010–2011, is not included);
converted from CSV to Parquet with the column types DuckDB inferred from the CSV (`Customer ID` is a `DOUBLE`
because that is what inference produced — part of Block 1). No rows or values were changed.

## Files

| File | Bytes | SHA-256 |
|---|---:|---|
| `online_retail.parquet` | 3,218,445 | `4f7f7bc8788b32e7…` |

## Our World in Data — CO₂ and greenhouse-gas emissions

**Source:** Our World in Data, *CO₂ and Greenhouse Gas Emissions* dataset, https://github.com/owid/co2-data —
data file `https://raw.githubusercontent.com/owid/co2-data/master/owid-co2-data.csv` (fetched 2026-09-19, SHA-256 `7f78e2b218ce4bb8…`); codebook `https://raw.githubusercontent.com/owid/co2-data/master/owid-co2-codebook.csv` (fetched 2026-09-19, SHA-256 `33b4f5e00efd58c7…`).

**Authors:** Pablo Rosado, Hannah Ritchie, Max Roser, Edouard Mathieu, Bobbie Macdonald (Our World in Data).

**License:** Our World in Data's own work is licensed **CC BY 4.0**. Each underlying series keeps the terms of
its original source, named per column in `owid_codebook.csv` (the `source` column): the CO₂ series come from the
Global Carbon Budget (CC BY 4.0); the greenhouse-gas totals from Jones et al., *National contributions to climate
change* (CC BY 4.0); population and GDP from the sources OWID lists. Cite OWID and the named source when reusing.

**Changes made for this course:** a subset of 15 of the file's columns; years 1990 onward; rows sorted by
country and year. `countries.csv` is a further subset: only the rows that are countries, kept by the filter Lab 1 builds (the
file's aggregate rows — `World`, the continents, the income groups and the like — are gone). No values were edited.

## Files

| File | Bytes | SHA-256 |
|---|---:|---|
| `owid_co2.csv` | 790,518 | `8af6dcc277a5b174…` |
| `owid_codebook.csv` | 3,478 | `f74086edb7064dcd…` |
