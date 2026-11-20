# U.S. Leading Causes of Death Analysis (1999–2017)

**Tools:** SQL (SQLite), Python, Pandas, Matplotlib, Plotly  
**Data Source:** CDC National Center for Health Statistics (NCHS)  
**Scope:** 10,868 records | 19 years | National and state-level analysis

## Project Overview

This project examines 19 years of U.S. mortality data to identify long-term trends, measure changes across leading causes of death, and investigate geographic differences.

Using SQL and Python, I validated and prepared the data, calculated year-over-year and cumulative changes in age-adjusted mortality rates, and developed visualizations to communicate the findings.

The analysis addresses four questions:

1. How have age-adjusted mortality rates changed between 1999 and 2017?
2. Which causes of death experienced the largest increases and decreases?
3. How have the relative rankings of leading causes changed?
4. Were national declines in heart disease mortality consistent across states?

**Explore the analysis:** [SQL Queries](sql/03_analysis.sql) | [Python Notebook](notebooks/01_eda.ipynb)

---

## Key Findings

- **Heart disease mortality declined by 38.1%** between 1999 and 2017 but remained the leading cause of death throughout the period.
- **Alzheimer's disease experienced the largest percentage increase**, with its age-adjusted mortality rate rising 87.9%.
- **Unintentional injuries increased by 39.9%**, moving from fifth to third in the mortality-rate ranking.
- **Heart disease mortality declined in all 50 states and Washington, D.C.**, although the magnitude of improvement varied geographically.

---

## Data and Methodology

### Dataset

The dataset contains mortality statistics for the 10 leading causes of death, along with all-cause mortality, across the 50 states, Washington, D.C., and the United States overall.

The primary measure is the **age-adjusted death rate per 100,000 people**, standardized to the 2000 U.S. population.

Age-adjusted rates were selected instead of raw death counts because they allow more meaningful comparisons across years and geographic areas with different population age structures.

**Source:** [CDC/NCHS — Leading Causes of Death](https://www.cdc.gov/nchs/data-visualization/mortality-leading-causes/index.htm)

### Data Validation and Preparation

I began by inspecting the dataset in SQLite to evaluate its structure, completeness, and suitability for analysis.

Validation included checking:

- Record counts, data types, and year coverage
- Missing values and duplicate records
- Cause-of-death categories and geographic coverage
- Numeric validity of mortality values

The dataset contained **10,868 records across six columns**, covering 1999–2017. Validation identified no missing values, duplicate records based on year, cause, and state, or unexpected gaps in the year range.

The `Deaths` field was originally stored as text and was converted to a numeric type. I also standardized column names and separated national records from state-level records to prevent mixing different levels of aggregation.

The resulting national dataset contained **209 records**, representing 19 years and 11 cause categories.

### Analytical Approach

I used SQL for data validation, cleaning, and initial analysis, applying aggregations, common table expressions (CTEs), and window functions to calculate mortality-rate changes and rankings.

Python and Pandas were used for additional exploration, data transformations, and visualization with Matplotlib and Plotly.

Several decisions guided the analysis:

- **Absolute vs. percentage change:** Both measures were calculated to distinguish the magnitude of a change from its size relative to the starting rate.
- **Year-over-year vs. long-term trends:** Annual changes were examined alongside 1999–2017 comparisons to identify fluctuations that endpoint values alone might obscure.
- **Mortality rates vs. rankings:** Changes in rates were analyzed separately from changes in relative position among causes.
- **National vs. state-level analysis:** After identifying a substantial national decline in heart disease mortality, I examined state-level data to determine whether the pattern was geographically consistent.

---

## Analysis and Results

### 1. National Mortality Trends

Heart disease and cancer remained the two leading causes throughout the period, despite substantial declines in their age-adjusted mortality rates.

Heart disease declined from **266.5 to 165.0 deaths per 100,000**, a reduction of 38.1%. Cancer declined from 200.8 to 152.5, a reduction of 24.1%.

Other causes followed different trajectories. Alzheimer's disease increased from 16.5 to 31.0 deaths per 100,000, while unintentional injuries increased from 35.3 to 49.4.

![National Mortality Trends](outputs/charts/national_mortality_trends.png)

### 2. Largest Increases and Decreases

Comparing mortality rates in 1999 and 2017 revealed substantial differences across causes.

| Cause of Death | 1999 Rate | 2017 Rate | % Change |
|---|---:|---:|---:|
| Alzheimer's disease | 16.5 | 31.0 | +87.9% |
| Unintentional injuries | 35.3 | 49.4 | +39.9% |
| Suicide | 10.5 | 14.0 | +33.3% |
| Cancer | 200.8 | 152.5 | -24.1% |
| Heart disease | 266.5 | 165.0 | -38.1% |
| Stroke | 61.6 | 37.6 | -39.0% |

*Rates represent age-adjusted deaths per 100,000 people.*

Alzheimer's disease had the largest percentage increase, while heart disease experienced the largest absolute decrease at **101.5 deaths per 100,000**.

Year-over-year analysis also identified notable changes:

- **Largest annual decline:** Heart disease decreased by 14.7 deaths per 100,000 between 2003 and 2004.
- **Largest annual increase:** Unintentional injuries increased by 4.2 deaths per 100,000 between 2015 and 2016.

These annual comparisons provide additional context for trends that are not necessarily consistent from year to year.

![Percentage Change in Mortality Rates](outputs/charts/mortality_rate_percent_change.png)

### 3. Changes in Leading-Cause Rankings

Although heart disease and cancer retained their positions throughout the period, several other causes shifted in the rankings.

| Cause of Death | 1999 Rank | 2017 Rank |
|---|---:|---:|
| Unintentional injuries | 5 | 3 |
| Alzheimer's disease | 8 | 6 |
| Stroke | 3 | 5 |
| Suicide | 10 | 9 |

*Rankings are based on age-adjusted mortality rates among the 10 causes examined, rather than the CDC's official leading-cause rankings based on death counts.*

Unintentional injuries and Alzheimer's disease each moved up two positions, while stroke fell from third to fifth.

Ranking changes provide useful context but should not be interpreted independently of mortality rates. A cause's position can change because of movements in other causes, even when its own rate remains relatively stable.

![Changes in Mortality Rankings](outputs/charts/mortality_rate_rank_change.png)

### 4. State-Level Heart Disease Mortality

To investigate whether the national decline in heart disease mortality was geographically consistent, I compared age-adjusted rates across the 50 states and Washington, D.C., between 1999 and 2017.

**Every state and Washington, D.C., experienced a decline**, although the magnitude varied.

The largest absolute decreases occurred in:

| State | Change in Rate |
|---|---:|
| New York | -134.9 |
| West Virginia | -133.5 |
| Illinois | -115.8 |
| Mississippi | -115.8 |
| Kentucky | -115.0 |

*Change in age-adjusted deaths per 100,000 people.*

The results indicate that declining heart disease mortality was a widespread trend rather than one driven by a small number of states. However, the differences in magnitude show that the national average does not fully represent the geographic variation.

![State-Level Heart Disease Decreases](outputs/charts/heart_disease_state_decreases.png)

![Heart Disease Mortality Map](outputs/charts/heart_disease_mortality_map.png)

[View Interactive Heart Disease Mortality Map](outputs/charts/heart_disease_mortality_map.html)

---

## Interpretation and Next Steps

The findings highlight two distinct patterns: substantial declines in historically dominant causes of death, particularly heart disease and cancer, alongside increases in Alzheimer's disease, unintentional injuries, and suicide.

The increase in unintentional injury mortality and the geographic variation in heart disease mortality are particularly relevant areas for further investigation.

Additional analysis incorporating demographic, socioeconomic, and healthcare-related data could help identify which populations experienced the greatest changes and whether certain geographic or population-level characteristics are associated with those differences.

### Limitations

- **Descriptive analysis:** The findings identify patterns and associations over time but do not establish causal relationships.
- **Age-adjusted rates:** These improve comparability across populations but are not equivalent to raw death counts.
- **Relative rankings:** Changes in ranking depend on the rates of other causes and do not necessarily reflect a corresponding change in a cause's own mortality rate.
- **Time coverage:** The dataset ends in 2017 and does not represent more recent mortality trends.
- **Geographic analysis:** State-level comparisons identify differences in mortality trends but do not explain the factors responsible for those differences.

---

## Conclusion

Between 1999 and 2017, U.S. mortality patterns changed considerably across leading causes of death.

Heart disease and cancer remained the two leading causes while experiencing substantial declines in age-adjusted mortality rates. In contrast, Alzheimer's disease, unintentional injuries, and suicide increased over the same period.

The state-level analysis confirmed that declining heart disease mortality was geographically widespread, although the magnitude of improvement varied.

By examining absolute and percentage changes, annual trends, relative rankings, and geographic differences, this analysis provides a more complete picture of mortality patterns than any single measure alone.

---

## Repository Structure

```text
NCHS-Leading-Causes-of-Death-Analysis/
│
├── data/
│   ├── raw/
│   └── processed/
│
├── sql/
│   ├── 01_data_validation.sql
│   ├── 02_data_cleaning.sql
│   └── 03_analysis.sql
│
├── notebooks/
│   └── 01_eda.ipynb
│
├── outputs/
│   └── charts/
│       ├── national_mortality_trends.png
│       ├── mortality_rate_percent_change.png
│       ├── mortality_rate_rank_change.png
│       ├── heart_disease_state_decreases.png
│       ├── heart_disease_mortality_map.png
│       └── heart_disease_mortality_map.html
│
├── README.md
└── pyproject.toml
```

The `sql/` directory contains the validation, preparation, and analysis queries. The `notebooks/` directory contains the Python analysis, while `outputs/charts/` contains the resulting visualizations.
