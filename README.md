# Northern Territory Population Analysis, 1986 to 2025

Regional analysis of 40 years of Northern Territory population estimates, built in Power BI from NT Government open data.

**Finding: Greater Darwin absorbed 80 percent of the Territory's working age growth since 2011, and is ageing faster than any other region while doing it.**

![Dashboard](images/dashboard.png)

---

## Why this question

The NT Government publishes population growth as a standing priority and maintains a Population Growth Strategy focused on attracting residents and retaining those already here. That makes *where* growth is landing, and what it is doing to regional age structures, a question with direct service planning consequences.

The analysis asks one thing: **which NT service regions are gaining or losing working age population, and which are ageing fastest?**

## What the data shows

**Growth is concentrated.** The Territory added 19,631 working age residents between 2011 and 2025. Greater Darwin accounted for 15,665 of them, 79.8 percent of the total. Big Rivers took 9.6 percent, Top End 5.4, Central Australia 4.6, East Arnhem 0.7. Barkly went backwards.

**Ageing is concentrated in the same place.** Greater Darwin's population aged 65 and over more than doubled from 8,142 to 17,248 over the same period, lifting its senior share from 6.3 to 10.9 percent. Its working age share fell 3.6 percentage points, the steepest decline in the Territory.

**Two regions have not recovered.** Barkly is the only region with fewer working age residents in 2025 than in 2011, and it peaked as far back as 1997. Central Australia peaked in 2010 and remains below that level.

**A visible economic shock.** Greater Darwin's working age population declined every year from 2017 to 2021, falling from 106,953 to 103,576, before rebounding to a new high of 110,509. The timing tracks the wind down of major construction activity in the region.

**Territory wide context.** The population aged 65 and over grew 6.6 times since 1986 while total population grew 1.7 times. Median age rose from 25.8 to 33.9.

The question this raises is how service capacity, particularly health and aged care, is distributed against a population whose growth and whose ageing are concentrating in the same region.

## Data

| | |
|---|---|
| Source | [NT Government Open Data Portal](https://data.nt.gov.au) |
| Dataset | Population estimates for NTG Service Regions, 1986 to 2025 |
| Licence | CC BY 4.0 |
| Grain | Year × Sex × Age Group × Aboriginal status × Region |
| Rows | 17,280, complete with no nulls |
| Vintage | 1986 to 2021 final, 2022 to 2024 revised, 2025 preliminary |

## Method and design decisions

**Population is a stock, not a flow.** Population estimates are point in time headcounts, so they are not additive across years. Every visual either filters to a single year or places Year on an axis. No visual aggregates population over time.

**Flat table, no star schema.** A single 17,280 row fact table with two derived columns. At this grain a dimensional model would add maintenance overhead with no query performance or usability benefit.

**No date table.** The data is annual and Year is an integer. DAX time intelligence operates on date grain and offers nothing at annual resolution, so Year is used directly on axes.

**Custom age sort.** Source age labels ("0-", "5-", "10-") sort alphabetically, placing "5-" after "45-". A numeric sort column was derived and applied via Sort By Column, and labels were rewritten to readable form.

**No disaggregation by Aboriginal status.** The dataset carries this dimension. It is excluded from the report because the question does not require it. The trends reported were verified to hold across both cohorts: Greater Darwin's senior share rose from 2.9 to 5.2 percent among Aboriginal residents and from 6.7 to 11.8 percent among non Aboriginal residents over the same period.

**Truncated y axis on the Greater Darwin series.** The axis minimum is set below the series minimum to make a 3 percent movement in a large stock legible. No data points are clipped.

## Model

Twelve DAX measures in a dedicated `_Measures` table, covering population by life stage, share measures, dependency ratio, and 2011 baseline comparisons using `ALL()` to clear filter context.

Two derived columns built in Power Query: `Age Sort` (numeric, for axis ordering) and `Life Stage` (0-14, 15-64, 65+).

## Repository contents

```
├── README.md
├── nt-population-analysis.pbix     Power BI file, model and report
├── report.pdf                       Exported single page report
├── data/                            Source data as downloaded
└── images/                          Report screenshot
```

## Attribution

Contains data sourced from the Northern Territory Government Open Data Portal, licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Analysis and interpretation are the author's own.

---

**Sadik** · Bachelor of Computer Science, Charles Darwin University · Darwin, NT
