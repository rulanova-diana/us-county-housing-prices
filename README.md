# U.S. County Housing Prices

How do income, poverty, education and unemployment relate to home values across U.S. counties?

This project merges three public data sources into one county-level dataset (3,052 counties, 2022) and looks at the question with correlation analysis, K-means clustering, a simple regression for outliers and a random forest for feature importance.

Final project for DSCI 510 (Principles of Programming for Data Science), University of Southern California, Fall 2025.

## Key findings

- **Income and education matter most.** Median home value correlates with median household income (r = 0.66) and with the share of college-educated adults (r = 0.65). Poverty is a moderate negative factor (r = -0.38). Unemployment alone explains very little (r = -0.10).
- **Four county profiles.** K-means (k = 4) splits counties into clear socio-economic groups:

  | Cluster | Counties | Median home value | Median income | Poverty rate | College educated |
  |---|---|---|---|---|---|
  | Affluent | 212 | $545K | $100K | 7.0% | 45% |
  | Prosperous | 898 | $292K | $72K | 9.6% | 29% |
  | Average | 1,308 | $179K | $58K | 13.5% | 19% |
  | Struggling | 634 | $150K | $47K | 20.6% | 15% |

- **Resort markets break the pattern.** A linear model on the four socio-economic factors underpredicts places like Nantucket, MA (+$1.99M above predicted), Pitkin, CO (+$1.62M) and Teton, WY (+$1.29M).
- **Feature importance (random forest):** income 0.43, college education 0.34, unemployment 0.12, poverty 0.11.

Slides with charts: [doc/Diana_Rulanova_presentation.pdf](doc/Diana_Rulanova_presentation.pdf)

## Data

| Source | What | Rows |
|---|---|---|
| Zillow Home Value Index | County median home values, December 2022 | 3,073 |
| U.S. Census Bureau, ACS 5-year (API) | Income, population, poverty, educational attainment | 3,222 |
| Bureau of Labor Statistics, LAUS | County unemployment rate, 2022 | 3,225 |

The datasets are joined on 5-digit county FIPS codes. After dropping counties with missing values, 3,052 remain. Raw data is downloaded at run time and is not stored in this repository.

## Project structure

```
src/
  config.py          URLs and settings
  data_loading.py    Zillow, BLS and Census API loaders
  data_cleaning.py   Zillow cleaning, FIPS formatting
  data_merging.py    Join on FIPS
  analysis.py        Descriptive statistics, correlations, state-level summary
  main.py            Runs the pipeline end to end
  tests.py           Unit tests for cleaning and merging
notebooks/
  analysis.ipynb     Full analysis with outputs: clustering, maps, outliers, feature importance
doc/                 Presentation slides
```

## How to run

1. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

2. Get a free Census API key at https://api.census.gov/data/key_signup.html, copy `.env.example` to `.env` and put the key there:

   ```
   CENSUS_API_KEY=your_key
   ```

3. Run the tests and the pipeline:

   ```bash
   python src/tests.py
   python src/main.py
   ```

4. For clustering, maps and the outlier analysis, open `notebooks/analysis.ipynb`.

## Limitations

- One point in time (2022), so the results describe associations, not causes.
- Income, poverty and education are strongly correlated with each other (income vs. poverty r = -0.74), which makes individual effects hard to separate.
- County medians hide large differences inside big counties such as Los Angeles.

## Author

Diana Rulanova, M.S. Applied Data Science, University of Southern California
