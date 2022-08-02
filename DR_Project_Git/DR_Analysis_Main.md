DR_Analysis_Main
================
Vikas Maturi
2021-07-07

-   <a href="#setup" id="toc-setup">Setup</a>
    -   <a href="#libraries-and-data-import"
        id="toc-libraries-and-data-import">Libraries and data import</a>
    -   <a href="#join-severity-and-timeseries-patientvisit-data"
        id="toc-join-severity-and-timeseries-patientvisit-data">Join severity
        and timeseries patient/visit data</a>
-   <a href="#cleaned-datasets-for-analysis"
    id="toc-cleaned-datasets-for-analysis">Cleaned datasets for analysis</a>
-   <a href="#table-1-patient-demographics"
    id="toc-table-1-patient-demographics">Table 1: Patient Demographics</a>
    -   <a href="#construct-dataset-to-generate-data-tables"
        id="toc-construct-dataset-to-generate-data-tables">Construct dataset to
        generate data tables</a>
    -   <a href="#table-age-by-race-and-insurance"
        id="toc-table-age-by-race-and-insurance">Table: Age by race and
        insurance</a>
    -   <a href="#table-gender-by-race-and-insurance"
        id="toc-table-gender-by-race-and-insurance">Table: Gender by race and
        insurance</a>
    -   <a href="#table-baseline-va-by-race-and-insurance"
        id="toc-table-baseline-va-by-race-and-insurance">Table: Baseline VA by
        race and insurance</a>
    -   <a href="#table-vegf-drug-by-race-and-insurance"
        id="toc-table-vegf-drug-by-race-and-insurance">Table: VEGF drug by race
        and insurance</a>
    -   <a href="#table-smoke-status-by-race-and-insurance"
        id="toc-table-smoke-status-by-race-and-insurance">Table: Smoke status by
        race and insurance</a>
    -   <a href="#table-insurance-by-race"
        id="toc-table-insurance-by-race">Table: Insurance by race</a>
-   <a href="#calculation-x-mean-va-and-sd-of-va-and-at-baseline"
    id="toc-calculation-x-mean-va-and-sd-of-va-and-at-baseline">Calculation
    X: Mean VA and SD of VA and at baseline</a>
-   <a href="#calculation-x-n-after-eliminating-icd-10-diagnostic-codes"
    id="toc-calculation-x-n-after-eliminating-icd-10-diagnostic-codes">Calculation
    X: N after eliminating ICD-10 diagnostic codes</a>
-   <a href="#figure-1a-baseline-differences-in-severity-score-by-race"
    id="toc-figure-1a-baseline-differences-in-severity-score-by-race">Figure
    1A: Baseline Differences in Severity Score by Race</a>
-   <a
    href="#figure-2a-and-2b-differences-in-rates-of-bevacizumab-injection-by-severity-category-race-and-insurance-status"
    id="toc-figure-2a-and-2b-differences-in-rates-of-bevacizumab-injection-by-severity-category-race-and-insurance-status">Figure
    2A and 2B: Differences in Rates of Bevacizumab Injection by Severity
    Category, Race and Insurance Status</a>
-   <a
    href="#regression-x-variables-that-influence-likelihood-of-receiving-bevacizumab-treatment"
    id="toc-regression-x-variables-that-influence-likelihood-of-receiving-bevacizumab-treatment">Regression
    X: Variables that influence likelihood of receiving Bevacizumab
    treatment</a>
-   <a
    href="#regression-y-using-no-intercept-model-to-determine-variables-that-influence-likelihood-of-receiving-bevacizumab-treatment"
    id="toc-regression-y-using-no-intercept-model-to-determine-variables-that-influence-likelihood-of-receiving-bevacizumab-treatment">Regression
    Y: Using No-Intercept Model to Determine Variables that influence
    likelihood of receiving Bevacizumab treatment</a>
    -   <a
        href="#forest-plot-representing-effect-of-race-on-bevacizumab-administration-by-insurance-status"
        id="toc-forest-plot-representing-effect-of-race-on-bevacizumab-administration-by-insurance-status">Forest
        Plot Representing Effect of Race on Bevacizumab Administration by
        Insurance Status</a>
-   <a
    href="#supplement-differences-in-rates-of-bevacizumab-aflibercept-and-ranibizumab-injection-by-severity-category-race-and-insurance-status"
    id="toc-supplement-differences-in-rates-of-bevacizumab-aflibercept-and-ranibizumab-injection-by-severity-category-race-and-insurance-status">Supplement:
    Differences in Rates of Bevacizumab, Aflibercept, and Ranibizumab
    Injection by Severity Category, Race and Insurance Status</a>
-   <a
    href="#figure-3a-and-3b-visual-acuity-change-from-baseline-at-one-and-two-years-by-race"
    id="toc-figure-3a-and-3b-visual-acuity-change-from-baseline-at-one-and-two-years-by-race">Figure
    3A and 3B: Visual Acuity Change from Baseline at One and Two Years, by
    Race</a>
-   <a
    href="#figure-4a-4b-4c-visual-acuity-change-from-baseline-at-one-and-two-years-by-insurance-and-race"
    id="toc-figure-4a-4b-4c-visual-acuity-change-from-baseline-at-one-and-two-years-by-insurance-and-race">Figure
    4A, 4B, 4C: Visual Acuity Change from Baseline at One and Two Years, by
    Insurance and Race</a>
-   <a
    href="#regression-x-variables-that-influence-change-in-vision-after-one-year"
    id="toc-regression-x-variables-that-influence-change-in-vision-after-one-year">Regression
    X: Variables that influence change in vision after one year</a>
-   <a
    href="#regression-y-variables-that-influence-change-in-vision-after-one-year-stratified-by-race-and-insurance-status"
    id="toc-regression-y-variables-that-influence-change-in-vision-after-one-year-stratified-by-race-and-insurance-status">Regression
    Y: Variables that influence change in vision after one year: stratified
    by race and insurance status</a>
-   <a href="#forest-plots-on-va-change"
    id="toc-forest-plots-on-va-change">Forest Plots on VA change</a>
    -   <a
        href="#forest-plot-to-determine-factors-associated-with-va-changes-within-each-insurance-group"
        id="toc-forest-plot-to-determine-factors-associated-with-va-changes-within-each-insurance-group">Forest
        Plot to Determine Factors Associated with VA Changes within Each
        Insurance Group</a>
    -   <a
        href="#forest-plot-to-determine-factors-associated-with-va-changes-within-each-race"
        id="toc-forest-plot-to-determine-factors-associated-with-va-changes-within-each-race">Forest
        Plot to Determine Factors Associated with VA Changes within Each
        Race</a>
-   <a
    href="#calculation-x-number-of-patients-including-the-multivariate-linear-regression"
    id="toc-calculation-x-number-of-patients-including-the-multivariate-linear-regression">Calculation
    X: Number of patients including the multivariate linear regression</a>
-   <a
    href="#figure-5-average-severity-score-at-each-baseline-va-rounded-to-nearest-5-by-race"
    id="toc-figure-5-average-severity-score-at-each-baseline-va-rounded-to-nearest-5-by-race">Figure
    5: Average severity score at each baseline VA (rounded to nearest 5) by
    race</a>
-   <a
    href="#figure-6-average-severity-score-at-each-baseline-va-rounded-to-nearest-5-by-insurance"
    id="toc-figure-6-average-severity-score-at-each-baseline-va-rounded-to-nearest-5-by-insurance">Figure
    6: Average severity score at each baseline VA (rounded to nearest 5) by
    insurance</a>
-   <a href="#timeseries-analysis-of-visual-acuity"
    id="toc-timeseries-analysis-of-visual-acuity">Timeseries analysis of
    visual acuity</a>
    -   <a href="#distribution-of-va-by-race"
        id="toc-distribution-of-va-by-race">Distribution of VA by race</a>
    -   <a href="#timeseries-va-by-race-and-severity"
        id="toc-timeseries-va-by-race-and-severity">Timeseries VA by race and
        severity</a>
    -   <a href="#timeseries-va-by-race"
        id="toc-timeseries-va-by-race">Timeseries VA by race</a>
    -   <a href="#timeseries-va-by-insurance"
        id="toc-timeseries-va-by-insurance">Timeseries VA by insurance</a>
    -   <a href="#percentage-with-15-letter-loss-by-race"
        id="toc-percentage-with-15-letter-loss-by-race">Percentage with 15
        letter loss by race</a>
    -   <a href="#percentage-with-15-letter-loss-by-insurance"
        id="toc-percentage-with-15-letter-loss-by-insurance">Percentage with 15
        letter loss by insurance</a>
    -   <a href="#timeseries-va-by-insurance-and-race"
        id="toc-timeseries-va-by-insurance-and-race">Timeseries VA by insurance
        and race</a>
    -   <a href="#timeseries-va-by-smoking-and-race"
        id="toc-timeseries-va-by-smoking-and-race">Timeseries VA by smoking and
        race</a>
    -   <a href="#timeseries-va-by-gender-and-race"
        id="toc-timeseries-va-by-gender-and-race">Timeseries VA by gender and
        race</a>
    -   <a href="#race-va-timeseries" id="toc-race-va-timeseries">Race VA
        timeseries</a>
    -   <a href="#race-va-timeseries-bucketed-by-10"
        id="toc-race-va-timeseries-bucketed-by-10">Race VA timeseries bucketed
        by 10</a>
    -   <a href="#severity-race-va-timeseries"
        id="toc-severity-race-va-timeseries">Severity race VA timeseries</a>
    -   <a href="#severity-insurance-race-va-timeseries"
        id="toc-severity-insurance-race-va-timeseries">Severity insurance race
        VA timeseries</a>
-   <a href="#injections-by-va-analysis"
    id="toc-injections-by-va-analysis">Injections by VA analysis</a>
-   <a href="#baseline-analysis" id="toc-baseline-analysis">Baseline
    analysis</a>
    -   <a href="#severity-by-race" id="toc-severity-by-race">Severity by
        race</a>
    -   <a href="#severity-by-race-and-region"
        id="toc-severity-by-race-and-region">Severity by race and region</a>
    -   <a href="#severity-by-race-and-age"
        id="toc-severity-by-race-and-age">Severity by race and age</a>
    -   <a href="#severity-by-region" id="toc-severity-by-region">Severity by
        region</a>
    -   <a href="#severity-by-race-and-insurance"
        id="toc-severity-by-race-and-insurance">Severity by race and
        insurance</a>
    -   <a href="#severity-by-race-and-sex"
        id="toc-severity-by-race-and-sex">Severity by race and sex</a>
    -   <a href="#severity-by-type-of-treatment"
        id="toc-severity-by-type-of-treatment">Severity by type of treatment</a>
    -   <a
        href="#likelihood-of-drug-received-by-severity-score-and-race-given-that-patient-recieved-anti-vegf-treatment"
        id="toc-likelihood-of-drug-received-by-severity-score-and-race-given-that-patient-recieved-anti-vegf-treatment">Likelihood
        of drug received by severity score and race (given that patient recieved
        anti-vegf treatment)</a>
    -   <a
        href="#likelihood-of-drug-received-by-severity-score-category-and-race-given-that-patient-recieved-anti-vegf-treatment"
        id="toc-likelihood-of-drug-received-by-severity-score-category-and-race-given-that-patient-recieved-anti-vegf-treatment">Likelihood
        of drug received by severity score CATEGORY and race (given that patient
        recieved anti-vegf treatment)</a>
    -   <a
        href="#likelihood-of-drug-received-by-baseline-va-and-race-given-that-patient-recieved-anti-vegf-treatment"
        id="toc-likelihood-of-drug-received-by-baseline-va-and-race-given-that-patient-recieved-anti-vegf-treatment">Likelihood
        of drug received by baseline VA and race (given that patient recieved
        anti-vegf treatment)</a>
    -   <a
        href="#likelihood-of-drug-received-by-severity-score-category-and-insurance-given-that-patient-recieved-anti-vegf-treatment"
        id="toc-likelihood-of-drug-received-by-severity-score-category-and-insurance-given-that-patient-recieved-anti-vegf-treatment">Likelihood
        of drug received by severity score CATEGORY and insurance (given that
        patient recieved anti-vegf treatment)</a>
    -   <a href="#drug-received-by-race-and-insurance"
        id="toc-drug-received-by-race-and-insurance">Drug received by race and
        insurance</a>
    -   <a href="#severity-vs-vision" id="toc-severity-vs-vision">Severity
        vs. vision</a>
-   <a href="#regression" id="toc-regression">Regression</a>
    -   <a href="#linear-regression" id="toc-linear-regression">Linear
        regression</a>
    -   <a href="#quantile-regressionn" id="toc-quantile-regressionn">Quantile
        regressionn</a>

## Setup

#### Libraries and data import

``` r
# load libraries
library(tidyverse)
library(readxl)
library(lubridate)
library(knitr)
library(googlesheets4)
library(car)
library(lsmeans)
library(boot)

# parameters

race_colors <- c("#54AE55", "#189CD9", "#A167A5")

insurance_colors <- c("#A00033", "#D1603D", "#F7D002")
```

``` r
# read in cleaned, combined data on visits and severity
visits_severity <- read_csv(file = "data_clean/maturi_all_cleaned.csv")

# read in cleaned data on timeservies visual acuity
va_progression <- read_csv(file = "data_clean/va_progression.csv")
```

#### Join severity and timeseries patient/visit data

``` r
joined_data <- 
  visits_severity %>% 
  # select the data
  dplyr::select(pt_code_name, eye, first_problem_code, severity_score, gender, race_ethnicity, first_dr_age, age_group, insurance, region, smoke_status, new_class, valid_class, include_given_code, vision_category, vision_threatening, pt_code_name, eye, index_date, baseline_va_letter, proc_group_28, proc_group_365, proc_group_any, vegf_group_28, vegf_group_365, vegf_group_any, retina_speciality, baseline_iop, pdr_group, cat_eyes) %>% 
  left_join(va_progression, by = c("pt_code_name" = "patient_guid", "eye" = "va_eye", "index_date"))
```

## Cleaned datasets for analysis

The ‘timeseries_analysis’ dataset will be the basis of all analyses

``` r
# set seed to ensure that we randomly sample the same set of folks in each round
set.seed(1)
# filter out the necesssary data to conduct time series analysis
timeseries_analysis <-
  joined_data %>% 
  filter(
    # only data classified under new ICD codes or codes starting of the format 36x.xxx
    include_given_code == 1,
    #race is White, Black, or Hispanic, to ensure adequate numbers
    race_ethnicity %in% c("Caucasian", "Black or African American", "Hispanic"),
    #race and gender data available for the patient
    !is.na(race_ethnicity), 
    !is.na(gender),
    !is.na(severity_score),
    # only patients with one year and two year data available
    !is.na(one_year), 
    #!is.na(six_month),
    !is.na(two_year),
    # only patients getting antivegf treatment
    proc_group_28 == "antivegf",
    # no cat eyes
    !cat_eyes == 1
  ) %>% 
  # select only one eye per patient
  group_by(pt_code_name) %>%
  sample_n(1) %>%
  ungroup() %>%
  # adjust insurance data to combine medicare FFS and medicare managed
  mutate(
    insurance = if_else(insurance %in% c("Medicare FFS", "Medicare Managed"), "Medicare", insurance)
  ) %>% 
  mutate(
    baseline_va_r10 = plyr::round_any(baseline_va_letter, 10),
    baseline_va_quart = ntile(baseline_va_letter, 4),
    one_year_va_delta = one_year - baseline_va_letter,
    two_year_va_delta = two_year - baseline_va_letter,
    #turn race into a factor and rename races
    race_ethnicity = dplyr::case_when(
      race_ethnicity == "Caucasian" ~ "White",
      race_ethnicity == "Black or African American" ~ "Black",
      TRUE ~ race_ethnicity
    ),
    race_ethnicity = factor(race_ethnicity, ordered = TRUE, levels = c("White", "Black", "Hispanic")),
    insurance = factor(insurance, ordered = TRUE, levels = c("Private", "Medicare","Medicaid","Govt","Military","Unknown/Missing"))
  ) 
```

## Table 1: Patient Demographics

#### Construct dataset to generate data tables

``` r
# Create dataset for basic paper tables
tbl_data <-
  timeseries_analysis %>% 
  mutate(
    # unorder insurance to permit grouping
    insurance = as.character(insurance),
    insurance_tbl = if_else(
      insurance %in% c("Private", "Medicare", "Medicaid"),
      insurance, 
      "Other"
    ),
    age_tbl = case_when(
      first_dr_age < 53 ~ "< 53",
      first_dr_age >= 53 & first_dr_age <= 58 ~ "53 - 58",
      first_dr_age >= 59 & first_dr_age <= 64 ~ "59 - 64",
      first_dr_age >= 65 & first_dr_age <= 70 ~ "65 - 70",
      first_dr_age > 70 ~ "> 70"
    ),
    va_tbl = case_when(
      baseline_va_letter > 69.95 ~ "20/40 or better",
      baseline_va_letter >= 57.80 & baseline_va_letter <= 69.95 ~ "20/41 - 20/70",
      baseline_va_letter >= 35 & baseline_va_letter <= 57.80 ~ "20/71 - 20/200",
      baseline_va_letter < 35 ~ "20/201 or worse"
    ), 
    vegf_tbl = case_when(
      vegf_group_365 == "Avastin" ~ "bevacizumab" ,
      vegf_group_365 == "Lucentis" ~ "ranibizumab",
      vegf_group_365 == "Eylea" ~ "aflibercept",
      vegf_group_365 == "combo" ~ "combo"
    )
  )

tbl_data
```

    ## # A tibble: 43,274 × 40
    ##    pt_code_name   eye first_problem_c… severity_score gender race_ethnicity
    ##    <chr>        <dbl> <chr>                     <dbl> <chr>  <ord>         
    ##  1 00022808cb4…     1 362.02                       73 Male   White         
    ##  2 0002d4cc5f7…     1 362.02                       73 Male   Hispanic      
    ##  3 000340244d4…     1 E11.3411                     58 Male   White         
    ##  4 0003859ca8d…     1 E11.351                      78 Female White         
    ##  5 000406a5939…     1 362.05                       44 Male   White         
    ##  6 0007e418646…     1 362.02                       73 Female Black         
    ##  7 000944c41d8…     1 362.05                       44 Male   White         
    ##  8 00096924cfb…     1 E11.3211                     40 Male   White         
    ##  9 000aadb4949…     2 362.02                       73 Female White         
    ## 10 000b59b304e…     2 362.04                       35 Male   White         
    ## # … with 43,264 more rows, and 34 more variables: first_dr_age <dbl>,
    ## #   age_group <dbl>, insurance <chr>, region <chr>, smoke_status <chr>,
    ## #   new_class <dbl>, valid_class <dbl>, include_given_code <dbl>,
    ## #   vision_category <chr>, vision_threatening <dbl>, index_date <date>,
    ## #   baseline_va_letter <dbl>, proc_group_28 <chr>, proc_group_365 <chr>,
    ## #   proc_group_any <chr>, vegf_group_28 <chr>, vegf_group_365 <chr>,
    ## #   vegf_group_any <chr>, retina_speciality <dbl>, baseline_iop <dbl>, …

#### Table: Age by race and insurance

``` r
tbl_age_race <-
  tbl_data %>% 
  count(age_tbl, race_ethnicity) %>% 
  group_by(race_ethnicity) %>% 
  mutate(pct_age_race = round(n / sum(n) * 100, 3)) %>% 
  unite(x, "n", "pct_age_race", sep = " (") %>% 
  spread(key = race_ethnicity, value = x )

kable(tbl_age_race)
```

| age_tbl | White        | Black        | Hispanic     |
|:--------|:-------------|:-------------|:-------------|
| \< 53   | 5856 (18.897 | 1258 (21.243 | 1705 (26.796 |
| \> 70   | 6508 (21.001 | 1148 (19.385 | 911 (14.317  |
| 53 - 58 | 4986 (16.09  | 996 (16.819  | 1191 (18.718 |
| 59 - 64 | 6591 (21.269 | 1260 (21.277 | 1381 (21.704 |
| 65 - 70 | 7048 (22.744 | 1260 (21.277 | 1175 (18.466 |

``` r
tbl_age_insurance <-
  tbl_data %>% 
  count(age_tbl, insurance_tbl) %>% 
  group_by(insurance_tbl) %>% 
  mutate(pct_age_insurance = round(n / sum(n) * 100, 3)) %>%
  unite(x, "n", "pct_age_insurance", sep = " (") %>% 
  spread(key = insurance_tbl, value = x )

kable(tbl_age_insurance)
```

| age_tbl | Medicaid     | Medicare     | Other        | Private      |
|:--------|:-------------|:-------------|:-------------|:-------------|
| \< 53   | 1195 (48.283 | 2534 (9.505  | 1041 (30.927 | 4049 (37.581 |
| \> 70   | 94 (3.798    | 7687 (28.835 | 294 (8.734   | 492 (4.567   |
| 53 - 58 | 654 (26.424  | 2433 (9.126  | 872 (25.906  | 3214 (29.831 |
| 59 - 64 | 444 (17.939  | 5835 (21.888 | 695 (20.648  | 2258 (20.958 |
| 65 - 70 | 88 (3.556    | 8170 (30.646 | 464 (13.785  | 761 (7.063   |

#### Table: Gender by race and insurance

``` r
tbl_gender_race <-
  tbl_data %>% 
  count(gender, race_ethnicity) %>% 
  group_by(race_ethnicity) %>% 
  mutate(pct_age_race = round(n / sum(n) * 100, 3)) %>% 
  unite(x, "n", "pct_age_race", sep = " (") %>% 
  spread(key = race_ethnicity, value = x )

kable(tbl_gender_race)
```

| gender  | White         | Black        | Hispanic     |
|:--------|:--------------|:-------------|:-------------|
| Female  | 13715 (44.258 | 3455 (58.342 | 2928 (46.016 |
| Male    | 17160 (55.374 | 2441 (41.219 | 3389 (53.261 |
| Unknown | 114 (0.368    | 26 (0.439    | 46 (0.723    |

``` r
tbl_gender_insurance <-
  tbl_data %>% 
  count(gender, insurance_tbl) %>% 
  group_by(insurance_tbl) %>% 
  mutate(pct_gender_insurance = round(n / sum(n) * 100, 3)) %>%
  unite(x, "n", "pct_gender_insurance", sep = " (") %>% 
  spread(key = insurance_tbl, value = x )

kable(tbl_gender_insurance)
```

| gender  | Medicaid     | Medicare      | Other        | Private      |
|:--------|:-------------|:--------------|:-------------|:-------------|
| Female  | 1286 (51.96  | 12936 (48.524 | 1360 (40.404 | 4516 (41.916 |
| Male    | 1178 (47.596 | 13598 (51.007 | 1993 (59.21  | 6221 (57.741 |
| Unknown | 11 (0.444    | 125 (0.469    | 13 (0.386    | 37 (0.343    |

#### Table: Baseline VA by race and insurance

``` r
tbl_va_race <-
  tbl_data %>% 
  count(va_tbl, race_ethnicity) %>% 
  group_by(race_ethnicity) %>% 
  mutate(pct_va_race = round(n / sum(n) * 100, 3)) %>% 
  unite(x, "n", "pct_va_race", sep = " (") %>% 
  spread(key = race_ethnicity, value = x )

kable(tbl_va_race)
```

| va_tbl          | White         | Black        | Hispanic     |
|:----------------|:--------------|:-------------|:-------------|
| 20/201 or worse | 221 (0.713    | 53 (0.895    | 64 (1.006    |
| 20/40 or better | 17545 (56.617 | 3164 (53.428 | 3128 (49.159 |
| 20/41 - 20/70   | 8471 (27.336  | 1650 (27.862 | 1899 (29.844 |
| 20/71 - 20/200  | 4752 (15.334  | 1055 (17.815 | 1272 (19.991 |

``` r
tbl_va_insurance <-
  tbl_data %>% 
  count(va_tbl, insurance_tbl) %>% 
  group_by(insurance_tbl) %>% 
  mutate(pct_va_insurance = round(n / sum(n) * 100, 3)) %>%
  unite(x, "n", "pct_va_insurance", sep = " (") %>% 
  spread(key = insurance_tbl, value = x )

kable(tbl_va_insurance)
```

| va_tbl          | Medicaid     | Medicare      | Other        | Private      |
|:----------------|:-------------|:--------------|:-------------|:-------------|
| 20/201 or worse | 23 (0.929    | 229 (0.859    | 33 (0.98     | 53 (0.492    |
| 20/40 or better | 1335 (53.939 | 13799 (51.761 | 1952 (57.992 | 6751 (62.66  |
| 20/41 - 20/70   | 672 (27.152  | 7934 (29.761  | 865 (25.698  | 2549 (23.659 |
| 20/71 - 20/200  | 445 (17.98   | 4697 (17.619  | 516 (15.33   | 1421 (13.189 |

#### Table: VEGF drug by race and insurance

``` r
tbl_vegf_race <-
  tbl_data %>% 
  count(vegf_tbl, race_ethnicity) %>% 
  group_by(race_ethnicity) %>% 
  mutate(pct_vegf_race = round(n / sum(n) * 100, 3)) %>% 
  unite(x, "n", "pct_vegf_race", sep = " (") %>% 
  spread(key = race_ethnicity, value = x )

kable(tbl_vegf_race)
```

| vegf_tbl    | White         | Black        | Hispanic     |
|:------------|:--------------|:-------------|:-------------|
| aflibercept | 6241 (20.139  | 984 (16.616  | 673 (10.577  |
| bevacizumab | 14036 (45.293 | 3078 (51.976 | 4175 (65.614 |
| combo       | 6408 (20.678  | 1157 (19.537 | 1051 (16.517 |
| ranibizumab | 4304 (13.889  | 703 (11.871  | 464 (7.292   |

``` r
tbl_vegf_insurance <-
  tbl_data %>% 
  count(vegf_tbl, insurance_tbl) %>% 
  group_by(insurance_tbl) %>% 
  mutate(pct_vegf_insurance = round(n / sum(n) * 100, 3)) %>%
  unite(x, "n", "pct_vegf_insurance", sep = " (") %>% 
  spread(key = insurance_tbl, value = x )

kable(tbl_vegf_insurance)
```

| vegf_tbl    | Medicaid     | Medicare      | Other        | Private      |
|:------------|:-------------|:--------------|:-------------|:-------------|
| aflibercept | 268 (10.828  | 5152 (19.326  | 500 (14.854  | 1978 (18.359 |
| bevacizumab | 1578 (63.758 | 12610 (47.301 | 1935 (57.487 | 5166 (47.949 |
| combo       | 462 (18.667  | 5281 (19.809  | 586 (17.409  | 2287 (21.227 |
| ranibizumab | 167 (6.747   | 3616 (13.564  | 345 (10.25   | 1343 (12.465 |

#### Table: Smoke status by race and insurance

``` r
tbl_smoke_race <-
  tbl_data %>% 
  count(smoke_status, race_ethnicity) %>% 
  group_by(race_ethnicity) %>% 
  mutate(pct_smoke_race = round(n / sum(n) * 100, 3)) %>% 
  unite(x, "n", "pct_smoke_race", sep = " (") %>% 
  spread(key = race_ethnicity, value = x )


kable(tbl_smoke_race)
```

| smoke_status                                    | White         | Black        | Hispanic     |
|:------------------------------------------------|:--------------|:-------------|:-------------|
| Former / No longer active / Past History / Quit | 8753 (28.246  | 1434 (24.215 | 1386 (21.782 |
| No / Never                                      | 18345 (59.198 | 3846 (64.944 | 4341 (68.223 |
| Unknown / Unclassified                          | 352 (1.136    | 60 (1.013    | 67 (1.053    |
| Yes / Active                                    | 3539 (11.42   | 582 (9.828   | 569 (8.942   |

``` r
tbl_smoke_insurance <-
  tbl_data %>% 
  count(smoke_status, insurance_tbl) %>% 
  group_by(insurance_tbl) %>% 
  mutate(pct_smoke_insurance = round(n / sum(n) * 100, 3)) %>%
  unite(x, "n", "pct_smoke_insurance", sep = " (") %>% 
  spread(key = insurance_tbl, value = x )

kable(tbl_smoke_insurance)
```

| smoke_status                                    | Medicaid     | Medicare      | Other        | Private      |
|:------------------------------------------------|:-------------|:--------------|:-------------|:-------------|
| Former / No longer active / Past History / Quit | 528 (21.333  | 7990 (29.971  | 791 (23.5    | 2264 (21.014 |
| No / Never                                      | 1482 (59.879 | 15656 (58.727 | 2172 (64.528 | 7222 (67.032 |
| Unknown / Unclassified                          | 20 (0.808    | 251 (0.942    | 63 (1.872    | 145 (1.346   |
| Yes / Active                                    | 445 (17.98   | 2762 (10.36   | 340 (10.101  | 1143 (10.609 |

#### Table: Insurance by race

``` r
tbl_insurance_race <-
  tbl_data %>% 
  count(insurance_tbl, race_ethnicity) %>% 
  group_by(race_ethnicity) %>% 
  mutate(pct_insurance_race = n / sum(n) * 100)

kable(tbl_insurance_race)
```

| insurance_tbl | race_ethnicity |     n | pct_insurance_race |
|:--------------|:---------------|------:|-------------------:|
| Medicaid      | White          |  1392 |           4.491917 |
| Medicaid      | Black          |   371 |           6.264775 |
| Medicaid      | Hispanic       |   712 |          11.189690 |
| Medicare      | White          | 19551 |          63.090129 |
| Medicare      | Black          |  3803 |          64.218170 |
| Medicare      | Hispanic       |  3305 |          51.940908 |
| Other         | White          |  2091 |           6.747556 |
| Other         | Black          |   482 |           8.139142 |
| Other         | Hispanic       |   793 |          12.462675 |
| Private       | White          |  7955 |          25.670399 |
| Private       | Black          |  1266 |          21.377913 |
| Private       | Hispanic       |  1553 |          24.406726 |

## Calculation X: Mean VA and SD of VA and at baseline

``` r
mean_va_race <-
  tbl_data %>% 
  group_by(race_ethnicity) %>% 
  summarize(
    mean_baseline_va_letter = mean(baseline_va_letter),
    sd = sd(baseline_va_letter)
  )

kable(mean_va_race)
```

| race_ethnicity | mean_baseline_va_letter |       sd |
|:---------------|------------------------:|---------:|
| White          |                67.33531 | 12.61341 |
| Black          |                66.34541 | 13.06954 |
| Hispanic       |                65.21539 | 13.27573 |

``` r
mean_va_insurance <-
  tbl_data %>% 
  group_by(insurance) %>% 
  summarize(
    mean_baseline_va_letter = mean(baseline_va_letter),
    sd = sd(baseline_va_letter),
    n = n()
  )

kable(mean_va_insurance)
```

| insurance       | mean_baseline_va_letter |       sd |     n |
|:----------------|------------------------:|---------:|------:|
| Govt            |                68.75708 | 12.69992 |  1165 |
| Medicaid        |                66.39798 | 13.39400 |  2475 |
| Medicare        |                65.99012 | 12.72689 | 26659 |
| Military        |                68.27509 | 12.66333 |   538 |
| Private         |                68.99995 | 12.49771 | 10774 |
| Unknown/Missing |                66.57366 | 13.32002 |  1663 |

## Calculation X: N after eliminating ICD-10 diagnostic codes

``` r
tbl_data %>% 
  filter(!is.na(vision_category)) %>% 
  count()
```

    ## # A tibble: 1 × 1
    ##       n
    ##   <int>
    ## 1 20787

## Figure 1A: Baseline Differences in Severity Score by Race

To consider here: how do we want to respond to the NAs in this graph?
One option I could see is creating four new categories: (mild NPDR
unknown edema, moderate NPDR unknown edema, severe NPDR unknown edema,
PDR unknown edema)

``` r
timeseries_analysis %>% 
  dplyr::filter(!is.na(vision_category)) %>% 
  dplyr::count(vision_category, race_ethnicity) %>%
  dplyr::ungroup() %>% 
  dplyr::group_by(race_ethnicity) %>% 
  dplyr::mutate(prop_category = n / sum(n)) %>% 
  ggplot() + 
  geom_col(aes(x = vision_category, y = prop_category, group = race_ethnicity, fill = race_ethnicity), position = "dodge", alpha = 1) +
  theme_light() +
  theme(
    axis.text.x = element_text(angle = 55, hjust = 1),
    panel.grid.minor = element_blank(),
    panel.grid.major.x = element_blank()
  ) +
  scale_y_continuous(labels = scales::label_percent(accuracy = 10)) +
  scale_fill_manual(values = race_colors) +
  labs(
    x = "Severity Category",
    y = "Percentage ",
    fill = "Race"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

``` r
ggsave("figure1A.pdf")
```

    ## Saving 7 x 5 in image

## Figure 2A and 2B: Differences in Rates of Bevacizumab Injection by Severity Category, Race and Insurance Status

``` r
# Calculate likelihood of drug by severity score and race (given that patient received antivegf treatment)
drug_sev_cat_race <-
  timeseries_analysis %>% 
  filter(proc_group_365 == "antivegf") %>% 
  count(vision_category, race_ethnicity, vegf_group_365) %>% 
  group_by(vision_category, race_ethnicity) %>% 
  mutate(count_severity_race = sum(n)) %>% 
  mutate(prop = n / count_severity_race) %>% 
  ungroup() %>% 
  select(vision_category, race_ethnicity, vegf_group_365, prop, count = n)

drug_sev_cat_race %>% 
  arrange(desc(vision_category), race_ethnicity) #JAY changed this!! 
```

    ## # A tibble: 108 × 5
    ##    vision_category    race_ethnicity vegf_group_365   prop count
    ##    <chr>              <ord>          <chr>           <dbl> <int>
    ##  1 8 - PDR with edema White          Avastin        0.506   1848
    ##  2 8 - PDR with edema White          combo          0.191    699
    ##  3 8 - PDR with edema White          Eylea          0.209    762
    ##  4 8 - PDR with edema White          Lucentis       0.0939   343
    ##  5 8 - PDR with edema Black          Avastin        0.588    446
    ##  6 8 - PDR with edema Black          combo          0.182    138
    ##  7 8 - PDR with edema Black          Eylea          0.146    111
    ##  8 8 - PDR with edema Black          Lucentis       0.0831    63
    ##  9 8 - PDR with edema Hispanic       Avastin        0.678    729
    ## 10 8 - PDR with edema Hispanic       combo          0.156    168
    ## # … with 98 more rows

``` r
# Graphing likelihood of Bevacizumab by severity and race
plot_drug_sev_cat_race <-
  drug_sev_cat_race %>% 
  # remove individuals with no vision category
  filter(!is.na(vision_category), !vision_category %in% c("NA")) %>% 
  # remove combo, Eylea, and Lucentis for this graph
  filter(vegf_group_365 == "Avastin") %>% 
  # round proportion to nearest 100th
  mutate(pct_round = round(prop, 2)) %>% 
  ggplot(aes(x = vision_category, y = pct_round, fill = race_ethnicity)) +
  geom_col(position = "dodge") +
  theme_light() +
  theme(
    axis.text.x = element_text(angle = 45, hjust = 1),
    panel.grid.minor = element_blank(),
    panel.grid.major.x = element_blank()
  ) +
  #facet_grid(vegf_group_365 ~ .) +
  #geom_text(aes(label = paste0(pct_round, "%")), vjust = -0.5, position = position_dodge(width = 1)) +
  #scale_x_continuous()
  scale_y_continuous(labels = scales::label_percent(accuracy = 1), limits = c(0, 1)) +
  scale_fill_manual(values = race_colors) +
  labs(
    x = "Severity category",
    y = "Percentage of patients receiving Bevacizumab",
    fill = "Race"
  )

plot_drug_sev_cat_race
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->

``` r
ggsave("graphs_6_28/2A_plot_drug_sev_cat_race.png", plot_drug_sev_cat_race, width = 10, height = 5)
```

``` r
# Calculate likelihood of drug by severity score and insurance (given that patient received antivegf treatment)
drug_sev_cat_insurance <-
  timeseries_analysis %>%
  filter(proc_group_365 == "antivegf", insurance %in% c("Private", "Medicare", "Medicaid")) %>% 
  count(vision_category, insurance, vegf_group_365) %>% 
  group_by(vision_category, insurance) %>% 
  mutate(count_severity_race = sum(n)) %>% 
  mutate(prop = n / count_severity_race) %>% 
  ungroup() %>% 
  select(vision_category, insurance, vegf_group_365, prop, count = n)

drug_sev_cat_insurance %>% 
  arrange(desc(vision_category), insurance)
```

    ## # A tibble: 107 × 5
    ##    vision_category    insurance vegf_group_365   prop count
    ##    <chr>              <ord>     <chr>           <dbl> <int>
    ##  1 8 - PDR with edema Private   Avastin        0.535    824
    ##  2 8 - PDR with edema Private   combo          0.192    296
    ##  3 8 - PDR with edema Private   Eylea          0.182    281
    ##  4 8 - PDR with edema Private   Lucentis       0.0909   140
    ##  5 8 - PDR with edema Medicare  Avastin        0.528   1561
    ##  6 8 - PDR with edema Medicare  combo          0.185    547
    ##  7 8 - PDR with edema Medicare  Eylea          0.197    583
    ##  8 8 - PDR with edema Medicare  Lucentis       0.0890   263
    ##  9 8 - PDR with edema Medicaid  Avastin        0.680    329
    ## 10 8 - PDR with edema Medicaid  combo          0.151     73
    ## # … with 97 more rows

``` r
plot_drug_sev_cat_insurance <-
# Graphing likelihood of first drug type by severity and insurance
drug_sev_cat_insurance %>% 
  # remove individuals with no vision category
  filter(!is.na(vision_category), !vision_category %in% c("NA")) %>% 
  # remove combo, Eylea, and Lucentis for this graph
  filter(vegf_group_365 == "Avastin") %>% 
  # round proportion to nearest 100th
  mutate(pct_round = round(prop, 2)) %>%
  ggplot(aes(x = vision_category, y = prop, fill = insurance)) +
  geom_col(position = "dodge") +
  theme_light() +
  theme(
    axis.text.x = element_text(angle = 45, hjust=1),
    # remove background lines
    panel.grid.minor = element_blank(),
    panel.grid.major.x = element_blank()
  ) +
  #facet_grid(vegf_group_365~.) +
  scale_y_continuous(labels = scales::label_percent(accuracy = 1), limits = c(0, 1)) +
  # different colors for insurance
  scale_fill_manual(values = insurance_colors) +
  labs(
    x = "Severity category",
    y = "Percentage of patients receiving Bevacizumab",
    fill = "Insurance"
  )

plot_drug_sev_cat_insurance
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->

``` r
ggsave("graphs_6_28/2B_plot_drug_sev_cat_insurance.png", plot_drug_sev_cat_insurance, width = 10, height = 5)
```

## Regression X: Variables that influence likelihood of receiving Bevacizumab treatment

``` r
# create dataset for regression
data_avastin_reg <- 
  timeseries_analysis %>% 
  filter(
    insurance %in% c("Medicare", "Medicaid", "Private")
  ) %>% 
  mutate(
    # unorder for appropriate regression interpretation
    race_ethnicity = factor(race_ethnicity, ordered = FALSE),
    race_ethnicity = relevel(race_ethnicity, ref = "White"),
    # unorder for appropriate regression interpretation
    insurance = factor(insurance, ordered = FALSE),
    insurance = relevel(insurance, ref = "Private"),
    smoke_status = factor(smoke_status),
    smoke_status = relevel(smoke_status, ref = "No / Never"),
    avastin = if_else(vegf_group_365 == "Avastin", 1, 0)
  ) 

# glm for logistic regression
avastin_1.logm <- 
  glm(
    avastin ~ race_ethnicity + insurance + race_ethnicity * insurance + gender + first_dr_age + smoke_status + baseline_va_letter +  severity_score, 
    data = data_avastin_reg, 
    family = binomial
  ) 

summary(avastin_1.logm)
```

    ## 
    ## Call:
    ## glm(formula = avastin ~ race_ethnicity + insurance + race_ethnicity * 
    ##     insurance + gender + first_dr_age + smoke_status + baseline_va_letter + 
    ##     severity_score, family = binomial, data = data_avastin_reg)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -1.9075  -1.0986  -0.9093   1.1740   1.5986  
    ## 
    ## Coefficients:
    ##                                                               Estimate
    ## (Intercept)                                                 -0.8304549
    ## race_ethnicityBlack                                          0.1180342
    ## race_ethnicityHispanic                                       0.6143160
    ## insuranceMedicare                                            0.0985570
    ## insuranceMedicaid                                            0.4330683
    ## genderMale                                                  -0.0942642
    ## genderUnknown                                               -0.0997580
    ## first_dr_age                                                -0.0096645
    ## smoke_statusFormer / No longer active / Past History / Quit  0.0202601
    ## smoke_statusUnknown / Unclassified                           0.1026341
    ## smoke_statusYes / Active                                     0.1370671
    ## baseline_va_letter                                           0.0051258
    ## severity_score                                               0.0142757
    ## race_ethnicityBlack:insuranceMedicare                        0.1777931
    ## race_ethnicityHispanic:insuranceMedicare                     0.1260080
    ## race_ethnicityBlack:insuranceMedicaid                        0.1872070
    ## race_ethnicityHispanic:insuranceMedicaid                     0.1661550
    ##                                                             Std. Error
    ## (Intercept)                                                  0.1055185
    ## race_ethnicityBlack                                          0.0611981
    ## race_ethnicityHispanic                                       0.0573083
    ## insuranceMedicare                                            0.0299273
    ## insuranceMedicaid                                            0.0596638
    ## genderMale                                                   0.0209544
    ## genderUnknown                                                0.1565012
    ## first_dr_age                                                 0.0010703
    ## smoke_statusFormer / No longer active / Past History / Quit  0.0240504
    ## smoke_statusUnknown / Unclassified                           0.1008541
    ## smoke_statusYes / Active                                     0.0339618
    ## baseline_va_letter                                           0.0008153
    ## severity_score                                               0.0006960
    ## race_ethnicityBlack:insuranceMedicare                        0.0709378
    ## race_ethnicityHispanic:insuranceMedicare                     0.0695434
    ## race_ethnicityBlack:insuranceMedicaid                        0.1368319
    ## race_ethnicityHispanic:insuranceMedicaid                     0.1173484
    ##                                                             z value
    ## (Intercept)                                                  -7.870
    ## race_ethnicityBlack                                           1.929
    ## race_ethnicityHispanic                                       10.720
    ## insuranceMedicare                                             3.293
    ## insuranceMedicaid                                             7.258
    ## genderMale                                                   -4.499
    ## genderUnknown                                                -0.637
    ## first_dr_age                                                 -9.030
    ## smoke_statusFormer / No longer active / Past History / Quit   0.842
    ## smoke_statusUnknown / Unclassified                            1.018
    ## smoke_statusYes / Active                                      4.036
    ## baseline_va_letter                                            6.287
    ## severity_score                                               20.511
    ## race_ethnicityBlack:insuranceMedicare                         2.506
    ## race_ethnicityHispanic:insuranceMedicare                      1.812
    ## race_ethnicityBlack:insuranceMedicaid                         1.368
    ## race_ethnicityHispanic:insuranceMedicaid                      1.416
    ##                                                             Pr(>|z|)    
    ## (Intercept)                                                 3.54e-15 ***
    ## race_ethnicityBlack                                          0.05377 .  
    ## race_ethnicityHispanic                                       < 2e-16 ***
    ## insuranceMedicare                                            0.00099 ***
    ## insuranceMedicaid                                           3.91e-13 ***
    ## genderMale                                                  6.84e-06 ***
    ## genderUnknown                                                0.52385    
    ## first_dr_age                                                 < 2e-16 ***
    ## smoke_statusFormer / No longer active / Past History / Quit  0.39956    
    ## smoke_statusUnknown / Unclassified                           0.30884    
    ## smoke_statusYes / Active                                    5.44e-05 ***
    ## baseline_va_letter                                          3.24e-10 ***
    ## severity_score                                               < 2e-16 ***
    ## race_ethnicityBlack:insuranceMedicare                        0.01220 *  
    ## race_ethnicityHispanic:insuranceMedicare                     0.07000 .  
    ## race_ethnicityBlack:insuranceMedicaid                        0.17126    
    ## race_ethnicityHispanic:insuranceMedicaid                     0.15680    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 55288  on 39907  degrees of freedom
    ## Residual deviance: 53629  on 39891  degrees of freedom
    ## AIC: 53663
    ## 
    ## Number of Fisher Scoring iterations: 4

## Regression Y: Using No-Intercept Model to Determine Variables that influence likelihood of receiving Bevacizumab treatment

``` r
no_intercept_regression_avastin_data <- 
  timeseries_analysis %>% 
  filter(
    insurance %in% c("Medicare", "Medicaid", "Private"),
    race_ethnicity %in% c("White", "Black", "Hispanic")
  ) %>% 
  mutate(
    # unorder for appropriate regression interpretation
    race_ethnicity = factor(race_ethnicity, ordered = FALSE),
    race_ethnicity = relevel(race_ethnicity, ref = "White"),
    # unorder for appropriate regression interpretation
    insurance = factor(insurance, ordered = FALSE),
    insurance = relevel(insurance, ref = "Private"),
    smoke_status = factor(smoke_status),
    smoke_status = relevel(smoke_status, ref = "No / Never"),
    avastin = if_else(vegf_group_365 == "Avastin", 1, 0)
  ) 

# glm for logistic regression
no_intercept_avastin <- 
  glm(avastin ~ 0 + gender + race_ethnicity + insurance + race_ethnicity * insurance + first_dr_age + smoke_status + baseline_va_letter +  severity_score, 
    data = no_intercept_regression_avastin_data, family = binomial(link = "probit"))


summary(no_intercept_avastin)
```

    ## 
    ## Call:
    ## glm(formula = avastin ~ 0 + gender + race_ethnicity + insurance + 
    ##     race_ethnicity * insurance + first_dr_age + smoke_status + 
    ##     baseline_va_letter + severity_score, family = binomial(link = "probit"), 
    ##     data = no_intercept_regression_avastin_data)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -1.9272  -1.0992  -0.9095   1.1741   1.6015  
    ## 
    ## Coefficients:
    ##                                                               Estimate
    ## genderFemale                                                -0.5182405
    ## genderMale                                                  -0.5769336
    ## genderUnknown                                               -0.5819260
    ## race_ethnicityBlack                                          0.0740645
    ## race_ethnicityHispanic                                       0.3823148
    ## insuranceMedicare                                            0.0608310
    ## insuranceMedicaid                                            0.2703450
    ## first_dr_age                                                -0.0059686
    ## smoke_statusFormer / No longer active / Past History / Quit  0.0125240
    ## smoke_statusUnknown / Unclassified                           0.0638353
    ## smoke_statusYes / Active                                     0.0851868
    ## baseline_va_letter                                           0.0031772
    ## severity_score                                               0.0088792
    ## race_ethnicityBlack:insuranceMedicare                        0.1106431
    ## race_ethnicityHispanic:insuranceMedicare                     0.0778363
    ## race_ethnicityBlack:insuranceMedicaid                        0.1146698
    ## race_ethnicityHispanic:insuranceMedicaid                     0.0924488
    ##                                                             Std. Error
    ## genderFemale                                                 0.0655382
    ## genderMale                                                   0.0656205
    ## genderUnknown                                                0.1167629
    ## race_ethnicityBlack                                          0.0381844
    ## race_ethnicityHispanic                                       0.0354531
    ## insuranceMedicare                                            0.0186220
    ## insuranceMedicaid                                            0.0370648
    ## first_dr_age                                                 0.0006639
    ## smoke_statusFormer / No longer active / Past History / Quit  0.0149404
    ## smoke_statusUnknown / Unclassified                           0.0627204
    ## smoke_statusYes / Active                                     0.0210994
    ## baseline_va_letter                                           0.0005061
    ## severity_score                                               0.0004327
    ## race_ethnicityBlack:insuranceMedicare                        0.0442570
    ## race_ethnicityHispanic:insuranceMedicare                     0.0429474
    ## race_ethnicityBlack:insuranceMedicaid                        0.0843732
    ## race_ethnicityHispanic:insuranceMedicaid                     0.0709398
    ##                                                             z value
    ## genderFemale                                                 -7.907
    ## genderMale                                                   -8.792
    ## genderUnknown                                                -4.984
    ## race_ethnicityBlack                                           1.940
    ## race_ethnicityHispanic                                       10.784
    ## insuranceMedicare                                             3.267
    ## insuranceMedicaid                                             7.294
    ## first_dr_age                                                 -8.991
    ## smoke_statusFormer / No longer active / Past History / Quit   0.838
    ## smoke_statusUnknown / Unclassified                            1.018
    ## smoke_statusYes / Active                                      4.037
    ## baseline_va_letter                                            6.278
    ## severity_score                                               20.521
    ## race_ethnicityBlack:insuranceMedicare                         2.500
    ## race_ethnicityHispanic:insuranceMedicare                      1.812
    ## race_ethnicityBlack:insuranceMedicaid                         1.359
    ## race_ethnicityHispanic:insuranceMedicaid                      1.303
    ##                                                             Pr(>|z|)    
    ## genderFemale                                                2.63e-15 ***
    ## genderMale                                                   < 2e-16 ***
    ## genderUnknown                                               6.23e-07 ***
    ## race_ethnicityBlack                                          0.05242 .  
    ## race_ethnicityHispanic                                       < 2e-16 ***
    ## insuranceMedicare                                            0.00109 ** 
    ## insuranceMedicaid                                           3.01e-13 ***
    ## first_dr_age                                                 < 2e-16 ***
    ## smoke_statusFormer / No longer active / Past History / Quit  0.40188    
    ## smoke_statusUnknown / Unclassified                           0.30878    
    ## smoke_statusYes / Active                                    5.40e-05 ***
    ## baseline_va_letter                                          3.42e-10 ***
    ## severity_score                                               < 2e-16 ***
    ## race_ethnicityBlack:insuranceMedicare                        0.01242 *  
    ## race_ethnicityHispanic:insuranceMedicare                     0.06993 .  
    ## race_ethnicityBlack:insuranceMedicaid                        0.17412    
    ## race_ethnicityHispanic:insuranceMedicaid                     0.19251    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 55324  on 39908  degrees of freedom
    ## Residual deviance: 53630  on 39891  degrees of freedom
    ## AIC: 53664
    ## 
    ## Number of Fisher Scoring iterations: 4

``` r
matrix_coef <- summary(no_intercept_avastin)$coefficients[,2]
matrix_coef
```

    ##                                                genderFemale 
    ##                                                0.0655382423 
    ##                                                  genderMale 
    ##                                                0.0656204919 
    ##                                               genderUnknown 
    ##                                                0.1167628953 
    ##                                         race_ethnicityBlack 
    ##                                                0.0381844271 
    ##                                      race_ethnicityHispanic 
    ##                                                0.0354530782 
    ##                                           insuranceMedicare 
    ##                                                0.0186219668 
    ##                                           insuranceMedicaid 
    ##                                                0.0370647984 
    ##                                                first_dr_age 
    ##                                                0.0006638559 
    ## smoke_statusFormer / No longer active / Past History / Quit 
    ##                                                0.0149403837 
    ##                          smoke_statusUnknown / Unclassified 
    ##                                                0.0627204391 
    ##                                    smoke_statusYes / Active 
    ##                                                0.0210994335 
    ##                                          baseline_va_letter 
    ##                                                0.0005060504 
    ##                                              severity_score 
    ##                                                0.0004326807 
    ##                       race_ethnicityBlack:insuranceMedicare 
    ##                                                0.0442569729 
    ##                    race_ethnicityHispanic:insuranceMedicare 
    ##                                                0.0429473582 
    ##                       race_ethnicityBlack:insuranceMedicaid 
    ##                                                0.0843731863 
    ##                    race_ethnicityHispanic:insuranceMedicaid 
    ##                                                0.0709397843

``` r
inv.logit(matrix_coef)
```

    ##                                                genderFemale 
    ##                                                   0.5163787 
    ##                                                  genderMale 
    ##                                                   0.5163992 
    ##                                               genderUnknown 
    ##                                                   0.5291576 
    ##                                         race_ethnicityBlack 
    ##                                                   0.5095449 
    ##                                      race_ethnicityHispanic 
    ##                                                   0.5088623 
    ##                                           insuranceMedicare 
    ##                                                   0.5046554 
    ##                                           insuranceMedicaid 
    ##                                                   0.5092651 
    ##                                                first_dr_age 
    ##                                                   0.5001660 
    ## smoke_statusFormer / No longer active / Past History / Quit 
    ##                                                   0.5037350 
    ##                          smoke_statusUnknown / Unclassified 
    ##                                                   0.5156750 
    ##                                    smoke_statusYes / Active 
    ##                                                   0.5052747 
    ##                                          baseline_va_letter 
    ##                                                   0.5001265 
    ##                                              severity_score 
    ##                                                   0.5001082 
    ##                       race_ethnicityBlack:insuranceMedicare 
    ##                                                   0.5110624 
    ##                    race_ethnicityHispanic:insuranceMedicare 
    ##                                                   0.5107352 
    ##                       race_ethnicityBlack:insuranceMedicaid 
    ##                                                   0.5210808 
    ##                    race_ethnicityHispanic:insuranceMedicaid 
    ##                                                   0.5177275

#### Forest Plot Representing Effect of Race on Bevacizumab Administration by Insurance Status

``` r
#FOREST PLOT DEMONSTRATING EFFECT OF RACE, SPLIT BY INSURANCE STATUS
Private_avastin_logreg <-
  glm(
    avastin ~ race_ethnicity + gender + first_dr_age + smoke_status + baseline_va_letter +  severity_score,
    data = timeseries_analysis %>%
      filter(
        insurance %in% c("Private"),
        race_ethnicity %in% c("White", "Black", "Hispanic"),
        gender %in% c("Male", "Female"),
        smoke_status %in% c("Yes / Active", "Former / No longer active / Past History / Quit", "No / Never")
       ) %>% 
      mutate(
        # unorder for appropriate regression interpretation
        race_ethnicity = factor(race_ethnicity, ordered = FALSE),
        race_ethnicity = relevel(race_ethnicity, ref = "White"),
        smoke_status = factor(smoke_status),
        smoke_status = relevel(smoke_status, ref = "No / Never"),
        avastin = if_else(vegf_group_365 == "Avastin", 1, 0)
      ),
    family = binomial
  )



Medicare_avastin_logreg <-
  glm(
    avastin ~ race_ethnicity + gender + first_dr_age + smoke_status + baseline_va_letter +  severity_score,
    data = timeseries_analysis %>%
      filter(
        insurance %in% c("Medicare"),
        race_ethnicity %in% c("White", "Black", "Hispanic"),
        gender %in% c("Male", "Female"),
        smoke_status %in% c("Yes / Active", "Former / No longer active / Past History / Quit", "No / Never")
       ) %>% 
      mutate(
        # unorder for appropriate regression interpretation
        race_ethnicity = factor(race_ethnicity, ordered = FALSE),
        race_ethnicity = relevel(race_ethnicity, ref = "White"),
        smoke_status = factor(smoke_status),
        smoke_status = relevel(smoke_status, ref = "No / Never"),
        avastin = if_else(vegf_group_365 == "Avastin", 1, 0)
      ),
    family = binomial
  )

Medicaid_avastin_logreg <-
  glm(
    avastin ~ race_ethnicity + gender + first_dr_age + smoke_status + baseline_va_letter +  severity_score,
    data = timeseries_analysis %>%
      filter(
        insurance %in% c("Medicaid"),
        race_ethnicity %in% c("White", "Black", "Hispanic"),
        gender %in% c("Male", "Female"),
        smoke_status %in% c("Yes / Active", "Former / No longer active / Past History / Quit", "No / Never")
       ) %>% 
      mutate(
        # unorder for appropriate regression interpretation
        race_ethnicity = factor(race_ethnicity, ordered = FALSE),
        race_ethnicity = relevel(race_ethnicity, ref = "White"),
        smoke_status = factor(smoke_status),
        smoke_status = relevel(smoke_status, ref = "No / Never"),
        avastin = if_else(vegf_group_365 == "Avastin", 1, 0)
      ),
    family = binomial
  )

#Extract values from regression output
bev_private <- summary(Private_avastin_logreg)$coefficients[1:9,]

#Convert values into data frame
bev_private = as.data.frame(bev_private)
bev_private <- tibble::rownames_to_column(bev_private, "FACTOR")

#Create limits for values
bev_private$up_error = bev_private$Estimate + 1.96*bev_private$`Std. Error`
bev_private$down_error = bev_private$Estimate - 1.96*bev_private$`Std. Error`

#Calculate odds ratios and confidence intervals
bev_private$odds <- exp(bev_private$Estimate)

bev_private$upper_limit = exp(bev_private$up_error)
bev_private$lower_limit = exp(bev_private$down_error)
bev_private
```

    ##                                                        FACTOR     Estimate
    ## 1                                                 (Intercept) -1.405305056
    ## 2                                         race_ethnicityBlack  0.090829928
    ## 3                                      race_ethnicityHispanic  0.594751369
    ## 4                                                  genderMale -0.141105791
    ## 5                                                first_dr_age -0.002534082
    ## 6 smoke_statusFormer / No longer active / Past History / Quit -0.004742439
    ## 7                                    smoke_statusYes / Active  0.079740497
    ## 8                                          baseline_va_letter  0.005050871
    ## 9                                              severity_score  0.018310333
    ##    Std. Error     z value     Pr(>|z|)     up_error   down_error      odds
    ## 1 0.203218271 -6.91524953 4.670405e-12 -1.006997244 -1.803612868 0.2452922
    ## 2 0.061986104  1.46532726 1.428317e-01  0.212322691 -0.030662836 1.0950827
    ## 3 0.058171662 10.22407393 1.546995e-24  0.708767826  0.480734912 1.8125802
    ## 4 0.040394522 -3.49319125 4.772846e-04 -0.061932528 -0.220279054 0.8683974
    ## 5 0.002032842 -1.24657093 2.125549e-01  0.001450289 -0.006518453 0.9974691
    ## 6 0.049595360 -0.09562263 9.238203e-01  0.092464467 -0.101949345 0.9952688
    ## 7 0.065035776  1.22610203 2.201603e-01  0.207210618 -0.047729624 1.0830060
    ## 8 0.001607349  3.14236192 1.675907e-03  0.008201275  0.001900468 1.0050636
    ## 9 0.001361540 13.44825505 3.152234e-41  0.020978951  0.015641715 1.0184790
    ##   upper_limit lower_limit
    ## 1   0.3653143   0.1647028
    ## 2   1.2365468   0.9698025
    ## 3   2.0314866   1.6172625
    ## 4   0.9399463   0.8022949
    ## 5   1.0014513   0.9935027
    ## 6   1.0968742   0.9030753
    ## 7   1.2302417   0.9533915
    ## 8   1.0082350   1.0019023
    ## 9   1.0212006   1.0157647

``` r
#Add a group name
bev_private$group <- "private"
bev_private$FACTOR_clean <- c("Intercept", "Black (rel to White)", "Hispanic (rel to White)", "Male (rel to Female)", "DR Diag. Age", "Former Smoker (rel to Non-Smoker)", "Current Smoker (rel to Non-Smoker)", "Baseline VA", "Severity Score")

View(bev_private)

#MEDICARE DATA ASSEMBLY

bev_medicare <- summary(Medicare_avastin_logreg)$coefficients[1:9,]
bev_medicare = as.data.frame(bev_medicare)
bev_medicare <- tibble::rownames_to_column(bev_medicare, "FACTOR")

#Create limits for values
bev_medicare$up_error = bev_medicare$Estimate + 1.96*bev_medicare$`Std. Error`
bev_medicare$down_error = bev_medicare$Estimate - 1.96*bev_medicare$`Std. Error`

#Calculate odds ratios and confidence intervals
bev_medicare$odds <- exp(bev_medicare$Estimate)

bev_medicare$upper_limit = exp(bev_medicare$up_error)
bev_medicare$lower_limit = exp(bev_medicare$down_error)
bev_private
```

    ##                                                        FACTOR     Estimate
    ## 1                                                 (Intercept) -1.405305056
    ## 2                                         race_ethnicityBlack  0.090829928
    ## 3                                      race_ethnicityHispanic  0.594751369
    ## 4                                                  genderMale -0.141105791
    ## 5                                                first_dr_age -0.002534082
    ## 6 smoke_statusFormer / No longer active / Past History / Quit -0.004742439
    ## 7                                    smoke_statusYes / Active  0.079740497
    ## 8                                          baseline_va_letter  0.005050871
    ## 9                                              severity_score  0.018310333
    ##    Std. Error     z value     Pr(>|z|)     up_error   down_error      odds
    ## 1 0.203218271 -6.91524953 4.670405e-12 -1.006997244 -1.803612868 0.2452922
    ## 2 0.061986104  1.46532726 1.428317e-01  0.212322691 -0.030662836 1.0950827
    ## 3 0.058171662 10.22407393 1.546995e-24  0.708767826  0.480734912 1.8125802
    ## 4 0.040394522 -3.49319125 4.772846e-04 -0.061932528 -0.220279054 0.8683974
    ## 5 0.002032842 -1.24657093 2.125549e-01  0.001450289 -0.006518453 0.9974691
    ## 6 0.049595360 -0.09562263 9.238203e-01  0.092464467 -0.101949345 0.9952688
    ## 7 0.065035776  1.22610203 2.201603e-01  0.207210618 -0.047729624 1.0830060
    ## 8 0.001607349  3.14236192 1.675907e-03  0.008201275  0.001900468 1.0050636
    ## 9 0.001361540 13.44825505 3.152234e-41  0.020978951  0.015641715 1.0184790
    ##   upper_limit lower_limit   group                       FACTOR_clean
    ## 1   0.3653143   0.1647028 private                          Intercept
    ## 2   1.2365468   0.9698025 private               Black (rel to White)
    ## 3   2.0314866   1.6172625 private            Hispanic (rel to White)
    ## 4   0.9399463   0.8022949 private               Male (rel to Female)
    ## 5   1.0014513   0.9935027 private                       DR Diag. Age
    ## 6   1.0968742   0.9030753 private  Former Smoker (rel to Non-Smoker)
    ## 7   1.2302417   0.9533915 private Current Smoker (rel to Non-Smoker)
    ## 8   1.0082350   1.0019023 private                        Baseline VA
    ## 9   1.0212006   1.0157647 private                     Severity Score

``` r
#Add a group name
bev_medicare$group <- "medicare"
bev_medicare$FACTOR_clean <- c("Intercept", "Black (rel to White)", "Hispanic (rel to White)", "Male (rel to Female)", "DR Diag. Age", "Former Smoker (rel to Non-Smoker)", "Current Smoker (rel to Non-Smoker)", "Baseline VA", "Severity Score")
View(bev_medicare)

#MEDICAID DATA ASSEMBLY
bev_medicaid <- summary(Medicaid_avastin_logreg)$coefficients[1:9,]
bev_medicaid = as.data.frame(bev_medicaid)
bev_medicaid <- tibble::rownames_to_column(bev_medicaid, "FACTOR")

#Create limits for values
bev_medicaid$up_error = bev_medicaid$Estimate + 1.96*bev_medicaid$`Std. Error`
bev_medicaid$down_error = bev_medicaid$Estimate - 1.96*bev_medicaid$`Std. Error`

#Calculate odds ratios and confidence intervals
bev_medicaid$odds <- exp(bev_medicaid$Estimate)

bev_medicaid$upper_limit = exp(bev_medicaid$up_error)
bev_medicaid$lower_limit = exp(bev_medicaid$down_error)
View(bev_private)
View(bev_medicare)
View(bev_medicaid)


#Add a group name
bev_medicaid$group <- "medicaid"
bev_medicaid$FACTOR_clean <- c("Intercept", "Black (rel to White)", "Hispanic (rel to White)", "Male (rel to Female)", "DR Diag. Age", "Former Smoker (rel to Non-Smoker)", "Current Smoker (rel to Non-Smoker)", "Baseline VA", "Severity Score")

#Combine all group output information
combined_bev_insurance_output = rbind(bev_medicaid,bev_medicare, bev_private)

#Remove original factor names that created unaesthetic graph
combined_bev_insurance_output = subset(combined_bev_insurance_output, select = -c(FACTOR) )

#Remove intercept values to allow for graph interpretation
combined_bev_insurance_output<-combined_bev_insurance_output[!(combined_bev_insurance_output$FACTOR_clean=="Intercept"),]

#Create factor for group name to order the facet grids
combined_bev_insurance_output$group = factor(combined_bev_insurance_output$group, levels=c("private","medicare","medicaid"), labels = c("Private", "Medicare", "Medicaid")) 
combined_bev_insurance_output$FACTOR_clean = factor(combined_bev_insurance_output$FACTOR_clean, levels = c("Severity Score",  "Baseline VA", "Current Smoker (rel to Non-Smoker)", "Former Smoker (rel to Non-Smoker)", "DR Diag. Age", "Male (rel to Female)","Hispanic (rel to White)", "Black (rel to White)", "Intercept"))
```

``` r
plot_forest_bev_insurance <- 
  ggplot(combined_bev_insurance_output,aes(y = FACTOR_clean, x = odds))+
  geom_point()+
  facet_wrap(~group)+
  geom_segment(aes(x = lower_limit, xend = upper_limit, yend = FACTOR_clean))+
  geom_vline(lty=2, aes(xintercept=1), colour = 'red') +
  labs(y= "Factor", x = "Odds of Receiving Bevacizumab", title = "Factors Associated with Bevacizumab Treatment by Insurance") + 
  theme_bw() +
  theme(
    plot.title = element_text(hjust = 0.5),
    # remove background lines
    panel.grid.minor = element_blank(),
    panel.grid.major = element_blank()
  )


plot_forest_bev_insurance
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-22-1.png)<!-- -->

``` r
#ggsave("graphs/plot_forest_bev_insurance.png", plot_forest_bev_insurance, width = 10, height = 5)
```

``` r
#FOREST PLOT DEMONSTRATING EFFECT OF RACE, SPLIT BY INSURANCE STATUS
White_avastin_logreg <-
  glm(
    avastin ~ insurance + gender + first_dr_age + smoke_status + baseline_va_letter +  severity_score,
    data = timeseries_analysis %>%
      filter(
        insurance %in% c("Medicare", "Medicaid", "Private"),
        race_ethnicity %in% c("White"),
        gender %in% c("Male", "Female"),
        smoke_status %in% c("Yes / Active", "Former / No longer active / Past History / Quit", "No / Never")
       ) %>% 
      mutate(
        # unorder for appropriate regression interpretation
        insurance = factor(insurance, ordered = FALSE),
        insurance = relevel(insurance, ref = "Private"),
        smoke_status = factor(smoke_status),
        smoke_status = relevel(smoke_status, ref = "No / Never"),
        avastin = if_else(vegf_group_365 == "Avastin", 1, 0)
      ),
    family = binomial
  )

summary(White_avastin_logreg)
```

    ## 
    ## Call:
    ## glm(formula = avastin ~ insurance + gender + first_dr_age + smoke_status + 
    ##     baseline_va_letter + severity_score, family = binomial, data = timeseries_analysis %>% 
    ##     filter(insurance %in% c("Medicare", "Medicaid", "Private"), 
    ##         race_ethnicity %in% c("White"), gender %in% c("Male", 
    ##             "Female"), smoke_status %in% c("Yes / Active", "Former / No longer active / Past History / Quit", 
    ##             "No / Never")) %>% mutate(insurance = factor(insurance, 
    ##     ordered = FALSE), insurance = relevel(insurance, ref = "Private"), 
    ##     smoke_status = factor(smoke_status), smoke_status = relevel(smoke_status, 
    ##         ref = "No / Never"), avastin = if_else(vegf_group_365 == 
    ##         "Avastin", 1, 0)))
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -1.6073  -1.0641  -0.9423   1.2191   1.6057  
    ## 
    ## Coefficients:
    ##                                                               Estimate
    ## (Intercept)                                                 -0.9262526
    ## insuranceMedicare                                            0.0880002
    ## insuranceMedicaid                                            0.4264501
    ## genderMale                                                  -0.0926253
    ## first_dr_age                                                -0.0089711
    ## smoke_statusFormer / No longer active / Past History / Quit  0.0494319
    ## smoke_statusYes / Active                                     0.1324540
    ## baseline_va_letter                                           0.0057675
    ## severity_score                                               0.0144313
    ##                                                             Std. Error
    ## (Intercept)                                                  0.1248362
    ## insuranceMedicare                                            0.0313183
    ## insuranceMedicaid                                            0.0601459
    ## genderMale                                                   0.0245241
    ## first_dr_age                                                 0.0012818
    ## smoke_statusFormer / No longer active / Past History / Quit  0.0278325
    ## smoke_statusYes / Active                                     0.0389534
    ## baseline_va_letter                                           0.0009743
    ## severity_score                                               0.0008229
    ##                                                             z value
    ## (Intercept)                                                  -7.420
    ## insuranceMedicare                                             2.810
    ## insuranceMedicaid                                             7.090
    ## genderMale                                                   -3.777
    ## first_dr_age                                                 -6.999
    ## smoke_statusFormer / No longer active / Past History / Quit   1.776
    ## smoke_statusYes / Active                                      3.400
    ## baseline_va_letter                                            5.920
    ## severity_score                                               17.537
    ##                                                             Pr(>|z|)    
    ## (Intercept)                                                 1.17e-13 ***
    ## insuranceMedicare                                           0.004956 ** 
    ## insuranceMedicaid                                           1.34e-12 ***
    ## genderMale                                                  0.000159 ***
    ## first_dr_age                                                2.59e-12 ***
    ## smoke_statusFormer / No longer active / Past History / Quit 0.075725 .  
    ## smoke_statusYes / Active                                    0.000673 ***
    ## baseline_va_letter                                          3.22e-09 ***
    ## severity_score                                               < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 39165  on 28478  degrees of freedom
    ## Residual deviance: 38550  on 28470  degrees of freedom
    ## AIC: 38568
    ## 
    ## Number of Fisher Scoring iterations: 4

``` r
Black_avastin_logreg <-
  glm(
    avastin ~ insurance + gender + first_dr_age + smoke_status + baseline_va_letter +  severity_score,
    data = timeseries_analysis %>%
      filter(
        insurance %in% c("Medicare", "Medicaid", "Private"),
        race_ethnicity %in% c("Black"),
        gender %in% c("Male", "Female"),
        smoke_status %in% c("Yes / Active", "Former / No longer active / Past History / Quit", "No / Never")
       ) %>% 
      mutate(
        # unorder for appropriate regression interpretation
        insurance = factor(insurance, ordered = FALSE),
        insurance = relevel(insurance, ref = "Private"),
        smoke_status = factor(smoke_status),
        smoke_status = relevel(smoke_status, ref = "No / Never"),
        avastin = if_else(vegf_group_365 == "Avastin", 1, 0)
      ),
    family = binomial
  )

Hispanic_avastin_logreg <-
  glm(
    avastin ~ insurance + gender + first_dr_age + smoke_status + baseline_va_letter +  severity_score,
    data = timeseries_analysis %>%
      filter(
        insurance %in% c("Medicare", "Medicaid", "Private"),
        race_ethnicity %in% c("Hispanic"),
        gender %in% c("Male", "Female"),
        smoke_status %in% c("Yes / Active", "Former / No longer active / Past History / Quit", "No / Never")
       ) %>% 
      mutate(
        # unorder for appropriate regression interpretation
        insurance = factor(insurance, ordered = FALSE),
        insurance = relevel(insurance, ref = "Private"),
        smoke_status = factor(smoke_status),
        smoke_status = relevel(smoke_status, ref = "No / Never"),
        avastin = if_else(vegf_group_365 == "Avastin", 1, 0)
      ),
    family = binomial
  )

#Extract values from regression output
bev_white <- summary(White_avastin_logreg)$coefficients[1:9,]

#Convert values into data frame
bev_white = as.data.frame(bev_white)
bev_white <- tibble::rownames_to_column(bev_white, "FACTOR")

#Create limits for values
bev_white$up_error = bev_white$Estimate + 1.96*bev_white$`Std. Error`
bev_white$down_error = bev_white$Estimate - 1.96*bev_white$`Std. Error`

#Calculate odds ratios and confidence intervals
bev_white$odds <- exp(bev_white$Estimate)

bev_white$upper_limit = exp(bev_white$up_error)
bev_white$lower_limit = exp(bev_white$down_error)

#Add a group name
bev_white$group <- "white"
bev_white$FACTOR_clean <- c("Intercept", "Medicare (rel to Private)", "Medicaid (rel to Private)", "Male (rel to Female)", "DR Diag. Age", "Former Smoker (rel to Non-Smoker)", "Current Smoker (rel to Non-Smoker)", "Baseline VA", "Severity Score")

View(bev_private)

#MEDICARE DATA ASSEMBLY

bev_black <- summary(Black_avastin_logreg)$coefficients[1:9,]
bev_black = as.data.frame(bev_black)
bev_black <- tibble::rownames_to_column(bev_black, "FACTOR")

#Create limits for values
bev_black$up_error = bev_black$Estimate + 1.96*bev_black$`Std. Error`
bev_black$down_error = bev_black$Estimate - 1.96*bev_black$`Std. Error`

#Calculate odds ratios and confidence intervals
bev_black$odds <- exp(bev_black$Estimate)

bev_black$upper_limit = exp(bev_black$up_error)
bev_black$lower_limit = exp(bev_black$down_error)

#Add a group name
bev_black$group <- "black"
bev_black$FACTOR_clean <- c("Intercept", "Medicare (rel to Private)", "Medicaid (rel to Private)", "Male (rel to Female)", "DR Diag. Age", "Former Smoker (rel to Non-Smoker)", "Current Smoker (rel to Non-Smoker)", "Baseline VA", "Severity Score")


#MEDICAID DATA ASSEMBLY
bev_hispanic <- summary(Hispanic_avastin_logreg)$coefficients[1:9,]
bev_hispanic = as.data.frame(bev_hispanic)
bev_hispanic <- tibble::rownames_to_column(bev_hispanic, "FACTOR")

#Create limits for values
bev_hispanic$up_error = bev_hispanic$Estimate + 1.96*bev_hispanic$`Std. Error`
bev_hispanic$down_error = bev_hispanic$Estimate - 1.96*bev_hispanic$`Std. Error`

#Calculate odds ratios and confidence intervals
bev_hispanic$odds <- exp(bev_hispanic$Estimate)

bev_hispanic$upper_limit = exp(bev_hispanic$up_error)
bev_hispanic$lower_limit = exp(bev_hispanic$down_error)


#Add a group name
bev_hispanic$group <- "hispanic"
bev_hispanic$FACTOR_clean <- c("Intercept", "Medicare (rel to Private)", "Medicaid (rel to Private)", "Male (rel to Female)", "DR Diag. Age", "Former Smoker (rel to Non-Smoker)", "Current Smoker (rel to Non-Smoker)", "Baseline VA", "Severity Score")


View(bev_white)
View(bev_black)
View(bev_hispanic)

#Combine all group output information
combined_bev_race_output = rbind(bev_hispanic,bev_black,bev_white)

#Remove original factor names that created unaesthetic graph
combined_bev_race_output = subset(combined_bev_race_output, select = -c(FACTOR) )

#Remove intercept values to allow for graph interpretation
combined_bev_race_output<-combined_bev_race_output[!(combined_bev_race_output$FACTOR_clean=="Intercept"),]

#Create factor for group name to order the facet grids
combined_bev_race_output$group = factor(combined_bev_race_output$group, levels=c("white","black","hispanic"), labels = c("White", "Black", "Hispanic")) 
combined_bev_race_output$FACTOR_clean = factor(combined_bev_race_output$FACTOR_clean, levels = c("Severity Score",  "Baseline VA", "Current Smoker (rel to Non-Smoker)", "Former Smoker (rel to Non-Smoker)", "DR Diag. Age", "Male (rel to Female)","Medicaid (rel to Private)", "Medicare (rel to Private)", "Intercept"))
```

``` r
plot_forest_bev_race <- 
  ggplot(combined_bev_race_output,aes(y = FACTOR_clean, x = odds))+
  geom_point()+
  facet_wrap(~group)+
  geom_segment(aes(x = lower_limit, xend = upper_limit, yend = FACTOR_clean))+
  geom_vline(lty=2, aes(xintercept=1), colour = 'red') +
  labs(y= "Factor", x = "Odds of Receiving Bevacizumab", title = "Factors Associated with Bevacizumab Treatment by Race") + 
  theme_bw() +
  theme(
    plot.title = element_text(hjust = 0.5),
    # remove background lines
    panel.grid.minor = element_blank(),
    panel.grid.major = element_blank()
  ) 


plot_forest_bev_race 
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-24-1.png)<!-- -->

``` r
#ggsave("graphs/plot_forest_bev_race.png", plot_forest_bev_race, width = 10, height = 5)
```

## Supplement: Differences in Rates of Bevacizumab, Aflibercept, and Ranibizumab Injection by Severity Category, Race and Insurance Status

``` r
plot_drug_sev_cat_race_all <- 
# Graphing likelihood of Bevacizumab by severity and race
drug_sev_cat_race %>% 
  # remove individuals with no vision category
  filter(!is.na(vision_category), !vision_category %in% c("NA")) %>% 
  # round proportion to nearest 100th
  mutate(pct_round = round(prop, 2)) %>% 
  ggplot(aes(x = vision_category, y = pct_round, fill = race_ethnicity)) +
  geom_col(position = "dodge") +
  theme_bw() +
  theme(
    axis.text.x = element_text(angle = 45, hjust=1),
    # remove background lines
    panel.grid.minor = element_blank(),
    panel.grid.major.x = element_blank()
  ) +  facet_grid(vegf_group_365 ~ .) +
  #geom_text(aes(label = paste0(pct_round, "%")), vjust = -0.5, position = position_dodge(width = 1)) +
  #scale_x_continuous()
  scale_y_continuous(labels = scales::label_percent(accuracy = 1), limits = c(0, 1)) +
  scale_fill_manual(values = race_colors) +
  labs(
    x = "Severity category",
    y = "Percentage of people receiving Bevacizumab",
    fill = "Race"
  )

plot_drug_sev_cat_race_all
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-25-1.png)<!-- -->

``` r
#ggsave("graphs/SUPPLEMENT_plot_drug_sev_cat_race_all.png", plot_drug_sev_cat_race_all, width = 10, height = 7)
```

``` r
plot_drug_sev_cat_insurance_all <-
# Graphing likelihood of first drug type by severity and insurance
drug_sev_cat_insurance %>% 
  # remove individuals with no vision category
  filter(!is.na(vision_category), !vision_category %in% c("NA")) %>% 
  # round proportion to nearest 100th
  mutate(pct_round = round(prop, 2)) %>%
  ggplot(aes(x = vision_category, y = prop, fill = insurance)) +
  geom_col(position = "dodge") +
  theme_bw() +
  theme(
    axis.text.x = element_text(angle = 45, hjust=1),
    # remove background lines
    panel.grid.minor = element_blank(),
    panel.grid.major.x = element_blank()
  ) +  facet_grid(vegf_group_365 ~ .) +
  facet_grid(vegf_group_365~.) +
  scale_y_continuous(labels = scales::label_percent(accuracy = 1), limits = c(0, 1)) +
  # different colors for insurance
  scale_fill_manual(values = insurance_colors) +
  labs(
    x = "Severity category",
    y = "Percentage of people receiving Bevacizumab",
    fill = "Insurance"
  )

plot_drug_sev_cat_insurance_all
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-26-1.png)<!-- -->

``` r
#ggsave("graphs/SUPPLEMENT_plot_drug_sev_cat_insurance_all.png", plot_drug_sev_cat_insurance_all, width = 10, height = 7)
```

## Figure 3A and 3B: Visual Acuity Change from Baseline at One and Two Years, by Race

``` r
change_15 <- 
  timeseries_analysis %>% 
  mutate(
    one_year_change = one_year - baseline_va_letter,
    two_year_change = two_year - baseline_va_letter,
    baseline_change = 0
  ) %>% 
  mutate(
    one_year_15_pt_loss = if_else(one_year_change <= -15, 1, 0),
    two_year_15_pt_loss = if_else(two_year_change <= -15, 1, 0),
    one_year_15_pt_gain = if_else(one_year_change >= 15, 1, 0),
    two_year_15_pt_gain = if_else(two_year_change >= 15, 1, 0),
  ) %>% 
  #gather(key = "time_diff", value = "va_change", baseline_change, one_year_change, two_year_change) %>% 
  ungroup()

# z value for p = .05 
z = 1.96
```

``` r
loss_15_race <-
  change_15 %>% 
  group_by(race_ethnicity) %>% 
  summarize(
    one_year_loss = sum(one_year_15_pt_loss) / n(),
    two_year_loss = sum(two_year_15_pt_loss) / n(),
    one_year_sd = sqrt(one_year_loss * (1 - one_year_loss) / n()),
    two_year_sd = sqrt(two_year_loss * (1 - two_year_loss) / n())
  ) %>% 
  gather(key = "time_diff", value = "pct_15pt_loss", one_year_loss, two_year_loss) %>% 
  mutate(
    sd = if_else(str_detect(string = time_diff, pattern = "one_year"), one_year_sd, two_year_sd),
    upper_bound = pct_15pt_loss + z * sd,
    lower_bound = pct_15pt_loss - z * sd
  ) %>%
  mutate(
    time_diff = case_when(
      time_diff == "one_year_loss" ~ "One year",
      time_diff == "two_year_loss" ~ "Two year"
    )
  )

View(loss_15_race)

plot_loss_15_race <-
loss_15_race %>%
  ggplot(aes(x = time_diff, y = pct_15pt_loss, fill = race_ethnicity)) +
  geom_col(position = "dodge") +
  geom_errorbar(aes(ymin = lower_bound, ymax = upper_bound), position = position_dodge(width = .9), width = .3) +
  theme_light() +
  theme(
    panel.grid.minor = element_blank(),
    panel.grid.major.x = element_blank()
  ) +
  scale_y_continuous(labels = scales::label_percent(accuracy = 1)) +
  scale_fill_manual(values = race_colors) +
  labs(
    x = "Time after baseline VA measurement",
    y = "Percentage of eyes that lost 15 or more pts of VA",
    fill = "Race"
  )

plot_loss_15_race
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-28-1.png)<!-- -->

``` r
ggsave("graphs/3A_plot_loss_15_race.png", plot_loss_15_race, width = 7, height = 5)
```

``` r
# plot change in visual acuity by race at one and two years

va_race <-
  timeseries_analysis %>% 
  group_by(race_ethnicity) %>% 
  mutate(
    one_year_change_new = one_year - baseline_va_letter,
    two_year_change_new = two_year - baseline_va_letter
  ) %>% 
  summarize(
    baseline_va = mean(baseline_va_letter), 
    one_year_va = mean(one_year), 
    two_year_va = mean(two_year),
    baseline_sd = sd(baseline_va_letter),
    one_year_sd = sd(one_year),
    two_year_sd = sd(two_year),
    one_year_change_sd = sd(one_year_change_new),
    two_year_change_sd= sd(two_year_change_new),
    count = n()) %>% 
  gather(key = "timepoint", value = "va_plot", baseline_va, one_year_va, two_year_va) %>% 
  ungroup()

va_race_diff <-
  va_race %>% 
  spread(key = timepoint, value = va_plot) %>% 
  mutate(
    one_year_change = one_year_va - baseline_va,
    two_year_change = two_year_va - baseline_va,
    baseline = 0
  ) %>% 
  gather(key = "time_diff", value = "va_change", baseline, one_year_change, two_year_change) %>% 
  mutate(
    time_diff = case_when(
      time_diff == "baseline" ~ "Baseline",
      time_diff == "one_year_change" ~ "One year",
      time_diff == "two_year_change" ~ "Two years"
    )
  ) %>% 
  ungroup()

plot_va_race_diff <- 
  va_race_diff %>% 
  ggplot(aes(x = time_diff, y = va_change, color = race_ethnicity)) +
  geom_point() +
  geom_line(aes(group = race_ethnicity)) +
  # ggrepel::geom_text_repel(
  #   data = va_race_diff %>% filter(time_diff == "two_year_change"),
  #   aes(label = count)
  # ) +
  theme_light() +
  theme(
    panel.grid.minor.x = element_blank(),
    panel.grid.major.x = element_blank(),
    legend.position = "bottom",
    #legend.spacing.x = unit(1.0, "cm")
    legend.margin = margin(c(5, 5, 5, 0)),
    legend.text = element_text(margin = margin(r = 10, unit = "pt"))
  ) +
  scale_color_manual(values = race_colors) +
  labs(
    x = "",
    y = "Change in visual acuity (VA)",
    color = "Race"
  ) 
  # guides(
  #   fill = guide_legend(
  #     label.position = "top",
  #     #title.position = "left", 
  #   )
  # )

plot_va_race_diff
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-29-1.png)<!-- -->

``` r
#ggsave("graphs/3B_plot_va_race_diff.png", plot_va_race_diff, width = 7, height = 5)
```

## Figure 4A, 4B, 4C: Visual Acuity Change from Baseline at One and Two Years, by Insurance and Race

``` r
# z value for p = .05 
z = 1.96

loss_15_insurance <-
  change_15 %>% 
  group_by(insurance) %>%
  filter(!insurance %in% c("Unknown/Missing", "Military", "Govt")) %>% 
  summarize(
    one_year_loss = sum(one_year_15_pt_loss) / n(),
    two_year_loss = sum(two_year_15_pt_loss) / n(),
    one_year_sd = sqrt(one_year_loss * (1 - one_year_loss) / n()),
    two_year_sd = sqrt(two_year_loss * (1 - two_year_loss) / n())
  ) %>% 
  gather(key = "time_diff", value = "pct_15pt_loss", one_year_loss, two_year_loss) %>% 
  mutate(
    sd = if_else(str_detect(string = time_diff, pattern = "one_year"), one_year_sd, two_year_sd),
    upper_bound = pct_15pt_loss + z * sd,
    lower_bound = pct_15pt_loss - z * sd
  ) %>% 
  mutate(
    time_diff = case_when(
      time_diff == "one_year_loss" ~ "One year",
      time_diff == "two_year_loss" ~ "Two year"
    )
  )


View(loss_15_insurance)


plot_loss_15_insurance <-
loss_15_insurance %>% 
  ggplot(aes(x = time_diff, y = pct_15pt_loss, fill = insurance)) +
  geom_col(position = "dodge") +
  geom_errorbar(aes(ymin = lower_bound, ymax = upper_bound), position = position_dodge(width = .9), width = .4) +
  theme_light() +
  theme(
    panel.grid.minor = element_blank(),
    panel.grid.major.x = element_blank()
  ) +
  scale_y_continuous(labels = scales::label_percent(accuracy = 1)) +
  scale_fill_manual(values = insurance_colors) +
  labs(
    x = "Time after baseline VA measurement",
    y = "Percentage of eyes that lost 15 or more points of VA",
    fill = "Insurance"
  )

plot_loss_15_insurance
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-30-1.png)<!-- -->

``` r
ggsave("graphs/4A_plot_loss_15_insurance.png", plot_loss_15_insurance, width = 7, height = 5)
```

``` r
va_insurance <-
  timeseries_analysis %>% 
  group_by(insurance) %>%
  filter(!insurance %in% c("Unknown/Missing", "Military", "Govt")) %>% 
  summarize(
    baseline_va = mean(baseline_va_letter), 
    one_year_va = mean(one_year), 
    two_year_va = mean(two_year),
    baseline_sd = sd(baseline_va_letter),
    one_year_sd = sd(one_year),
    two_year_sd = sd(two_year),
    count = n()) %>% 
  gather(key = "timepoint", value = "va_plot", baseline_va, one_year_va, two_year_va) %>% 
  ungroup()

va_insurance_diff <-
  va_insurance %>% 
  spread(key = timepoint, value = va_plot) %>% 
  mutate(
    one_year_change = one_year_va - baseline_va,
    two_year_change = two_year_va - baseline_va,
    baseline = 0
  ) %>% 
  gather(key = "time_diff", value = "va_change", baseline, one_year_change, two_year_change) %>% 
  mutate(
    time_diff = case_when(
      time_diff == "baseline" ~ "Baseline",
      time_diff == "one_year_change" ~ "One year",
      time_diff == "two_year_change" ~ "Two years"
    )
  ) %>% 
  ungroup()

plot_va_insurance_diff <-
va_insurance_diff %>% 
  ggplot(aes(x = time_diff, y = va_change, color = insurance)) +
  geom_point() +
  geom_line(aes(group = insurance)) +
  # ggrepel::geom_text_repel(
  #   data = va_insurance_diff %>% filter(time_diff == "two_year_change"),
  #   aes(label = count)
  # ) +
  theme_light() + 
  theme(
    panel.grid.minor.x = element_blank(),
    panel.grid.major.x = element_blank()
  ) +
  scale_color_manual(values = insurance_colors) +
  labs(
    x = "Time point",
    y = "Change in visual acuity",
    color = "Insurance"
  )

plot_va_insurance_diff
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-31-1.png)<!-- -->

``` r
#ggsave("graphs/4B_plot_va_insurance_diff.png", plot_va_insurance_diff, width = 7, height = 5)
```

``` r
insurance_race_va <-
  timeseries_analysis %>% 
  group_by(race_ethnicity, insurance) %>% 
  summarize(baseline_va = mean(baseline_va_letter), one_year_va = mean(one_year), two_year_va = mean(two_year), count = n()) %>% 
  gather(key = "timepoint", value = "va_plot", baseline_va, one_year_va, two_year_va)
```

    ## `summarise()` has grouped output by 'race_ethnicity'. You can override using the `.groups` argument.

``` r
va_insurance_race_diff_tbl <-
  insurance_race_va %>%
  filter(!insurance %in% c("Unknown/Missing", "Military", "Govt")) %>%
  spread(key = timepoint, value = va_plot) %>%
  mutate(
    one_year_change = one_year_va - baseline_va,
    two_year_change = two_year_va - baseline_va,
    baseline = 0
  )

va_insurance_race_diff <-
  insurance_race_va %>%
  filter(!insurance %in% c("Unknown/Missing", "Military", "Govt")) %>%
  spread(key = timepoint, value = va_plot) %>%
  mutate(
    one_year_change = one_year_va - baseline_va,
    two_year_change = two_year_va - baseline_va,
    baseline = 0
  ) %>%
  gather(key = "time_diff", value = "va_change", baseline, one_year_change, two_year_change) %>%
  mutate(
    time_diff = case_when(
      time_diff == "baseline" ~ "Baseline",
      time_diff == "one_year_change" ~ "One year",
      time_diff == "two_year_change" ~ "Two years"
    )
  ) %>% 
  ungroup()

va_insurance_race_diff_ci <-
  timeseries_analysis %>%
  mutate(
    one_year_change = one_year - baseline_va_letter,
    two_year_change = two_year - baseline_va_letter,
    baseline = 0
  ) %>%
  group_by(race_ethnicity, insurance) %>%
  summarize(
    mean_one_year_change = mean(one_year_change),
    mean_two_year_change = mean(two_year_change),
    sd_one_year_change = sd(one_year_change),
    sd_two_year_change = sd(two_year_change)
  )
```

    ## `summarise()` has grouped output by 'race_ethnicity'. You can override using the `.groups` argument.

``` r
plot_va_insurance_race_diff <-
va_insurance_race_diff %>%
  ggplot(aes(x = time_diff, y = va_change, color = race_ethnicity)) +
  geom_point() +
  geom_line(aes(group = interaction(insurance, race_ethnicity))) +
  # to display counts
  # ggrepel::geom_text_repel(
  #   data = va_insurance_race_diff %>% filter(time_diff == "two_year_change", insurance %in% c("Private", "Medicaid", "Medicare")),
  #   aes(label = count, hjust = -2)
  # ) +
  facet_grid(.~insurance) +
  theme_bw() +
  theme(
    axis.text.x = element_text(angle = 45, hjust=1),
    # remove background lines
    panel.grid.minor = element_blank(),
    panel.grid.major.x = element_blank()
  ) +
  scale_color_manual(values = race_colors) +
  labs(
    x = "Time point",
    y = "Visual acuity (VA) change",
    color = "Race"
  )

View(va_insurance_race_diff)
plot_va_insurance_race_diff
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-32-1.png)<!-- -->

``` r
#ggsave("graphs/4C_plot_va_insurance_race_diff.png", plot_va_insurance_race_diff, width = 10, height = 5)
```

## Regression X: Variables that influence change in vision after one year

``` r
# linear regression with race insurance interaction and severity score
one_year_va_delta_4.lm <- 
  lm(one_year_va_delta ~ race_ethnicity + insurance + race_ethnicity * insurance + gender + first_dr_age + smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
     data = timeseries_analysis %>% 
       filter(
         insurance %in% c("Medicare", "Medicaid", "Private")
       ) %>% 
       mutate(
         race_ethnicity = factor(race_ethnicity, ordered = FALSE),
         race_ethnicity = relevel(race_ethnicity, ref = "White"),
         insurance = factor(insurance, ordered = FALSE), 
         insurance = relevel(insurance, ref = "Private"),
         smoke_status = factor(smoke_status),
         smoke_status = relevel(smoke_status, ref = "No / Never")
       )
  ) 

summary(one_year_va_delta_4.lm)
```

    ## 
    ## Call:
    ## lm(formula = one_year_va_delta ~ race_ethnicity + insurance + 
    ##     race_ethnicity * insurance + gender + first_dr_age + smoke_status + 
    ##     baseline_va_letter + vegf_group_365 + severity_score, data = timeseries_analysis %>% 
    ##     filter(insurance %in% c("Medicare", "Medicaid", "Private")) %>% 
    ##     mutate(race_ethnicity = factor(race_ethnicity, ordered = FALSE), 
    ##         race_ethnicity = relevel(race_ethnicity, ref = "White"), 
    ##         insurance = factor(insurance, ordered = FALSE), insurance = relevel(insurance, 
    ##             ref = "Private"), smoke_status = factor(smoke_status), 
    ##         smoke_status = relevel(smoke_status, ref = "No / Never")))
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -96.894  -4.372   1.666   6.483  42.577 
    ## 
    ## Coefficients:
    ##                                                              Estimate
    ## (Intercept)                                                 36.411416
    ## race_ethnicityBlack                                         -1.456907
    ## race_ethnicityHispanic                                      -1.316521
    ## insuranceMedicare                                           -1.780485
    ## insuranceMedicaid                                           -2.128057
    ## genderMale                                                   0.405336
    ## genderUnknown                                               -1.214586
    ## first_dr_age                                                -0.057780
    ## smoke_statusFormer / No longer active / Past History / Quit  0.125248
    ## smoke_statusUnknown / Unclassified                           0.601629
    ## smoke_statusYes / Active                                    -0.658016
    ## baseline_va_letter                                          -0.395993
    ## vegf_group_365combo                                         -0.027556
    ## vegf_group_365Eylea                                          0.561849
    ## vegf_group_365Lucentis                                       0.767506
    ## severity_score                                              -0.063302
    ## race_ethnicityBlack:insuranceMedicare                        1.348333
    ## race_ethnicityHispanic:insuranceMedicare                    -0.078540
    ## race_ethnicityBlack:insuranceMedicaid                       -0.067253
    ## race_ethnicityHispanic:insuranceMedicaid                     0.602269
    ##                                                             Std. Error
    ## (Intercept)                                                   0.634959
    ## race_ethnicityBlack                                           0.368723
    ## race_ethnicityHispanic                                        0.338773
    ## insuranceMedicare                                             0.179176
    ## insuranceMedicaid                                             0.355968
    ## genderMale                                                    0.124870
    ## genderUnknown                                                 0.930065
    ## first_dr_age                                                  0.006362
    ## smoke_statusFormer / No longer active / Past History / Quit   0.143352
    ## smoke_statusUnknown / Unclassified                            0.602615
    ## smoke_statusYes / Active                                      0.202426
    ## baseline_va_letter                                            0.004856
    ## vegf_group_365combo                                           0.163024
    ## vegf_group_365Eylea                                           0.169126
    ## vegf_group_365Lucentis                                        0.192941
    ## severity_score                                                0.004181
    ## race_ethnicityBlack:insuranceMedicare                         0.427327
    ## race_ethnicityHispanic:insuranceMedicare                      0.408718
    ## race_ethnicityBlack:insuranceMedicaid                         0.801029
    ## race_ethnicityHispanic:insuranceMedicaid                      0.655178
    ##                                                             t value
    ## (Intercept)                                                  57.345
    ## race_ethnicityBlack                                          -3.951
    ## race_ethnicityHispanic                                       -3.886
    ## insuranceMedicare                                            -9.937
    ## insuranceMedicaid                                            -5.978
    ## genderMale                                                    3.246
    ## genderUnknown                                                -1.306
    ## first_dr_age                                                 -9.082
    ## smoke_statusFormer / No longer active / Past History / Quit   0.874
    ## smoke_statusUnknown / Unclassified                            0.998
    ## smoke_statusYes / Active                                     -3.251
    ## baseline_va_letter                                          -81.539
    ## vegf_group_365combo                                          -0.169
    ## vegf_group_365Eylea                                           3.322
    ## vegf_group_365Lucentis                                        3.978
    ## severity_score                                              -15.142
    ## race_ethnicityBlack:insuranceMedicare                         3.155
    ## race_ethnicityHispanic:insuranceMedicare                     -0.192
    ## race_ethnicityBlack:insuranceMedicaid                        -0.084
    ## race_ethnicityHispanic:insuranceMedicaid                      0.919
    ##                                                             Pr(>|t|)    
    ## (Intercept)                                                  < 2e-16 ***
    ## race_ethnicityBlack                                         7.79e-05 ***
    ## race_ethnicityHispanic                                      0.000102 ***
    ## insuranceMedicare                                            < 2e-16 ***
    ## insuranceMedicaid                                           2.27e-09 ***
    ## genderMale                                                  0.001171 ** 
    ## genderUnknown                                               0.191589    
    ## first_dr_age                                                 < 2e-16 ***
    ## smoke_statusFormer / No longer active / Past History / Quit 0.382281    
    ## smoke_statusUnknown / Unclassified                          0.318110    
    ## smoke_statusYes / Active                                    0.001152 ** 
    ## baseline_va_letter                                           < 2e-16 ***
    ## vegf_group_365combo                                         0.865775    
    ## vegf_group_365Eylea                                         0.000894 ***
    ## vegf_group_365Lucentis                                      6.96e-05 ***
    ## severity_score                                               < 2e-16 ***
    ## race_ethnicityBlack:insuranceMedicare                       0.001605 ** 
    ## race_ethnicityHispanic:insuranceMedicare                    0.847617    
    ## race_ethnicityBlack:insuranceMedicaid                       0.933090    
    ## race_ethnicityHispanic:insuranceMedicaid                    0.357973    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 12.17 on 39888 degrees of freedom
    ## Multiple R-squared:  0.1459, Adjusted R-squared:  0.1455 
    ## F-statistic: 358.6 on 19 and 39888 DF,  p-value: < 2.2e-16

``` r
vif(one_year_va_delta_4.lm)
```

    ##                               GVIF Df GVIF^(1/(2*Df))
    ## race_ethnicity           15.244230  2        1.975952
    ## insurance                 3.059205  2        1.322520
    ## gender                    1.042914  2        1.010560
    ## first_dr_age              1.464462  1        1.210150
    ## smoke_status              1.059461  3        1.009673
    ## baseline_va_letter        1.037146  1        1.018404
    ## vegf_group_365            1.050078  3        1.008177
    ## severity_score            1.136036  1        1.065850
    ## race_ethnicity:insurance 29.006857  4        1.523395

## Regression Y: Variables that influence change in vision after one year: stratified by race and insurance status

``` r
#Race analysis: Regress data across only privately-insured patients to see effect of race on vision: 
private_only_one_year_va_delta.lm <-
  lm(one_year_va_delta ~ race_ethnicity + gender + first_dr_age + smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
     data = timeseries_analysis %>% 
       filter(
         insurance %in% c("Private"), 
         race_ethnicity %in% c("White", "Black", "Hispanic")
       ) %>% 
       mutate(
         race_ethnicity = factor(race_ethnicity, ordered = FALSE),
         race_ethnicity = relevel(race_ethnicity, ref = "White"),
         smoke_status = factor(smoke_status),
         smoke_status = relevel(smoke_status, ref = "No / Never")
       )
  ) 
summary(private_only_one_year_va_delta.lm)
```

    ## 
    ## Call:
    ## lm(formula = one_year_va_delta ~ race_ethnicity + gender + first_dr_age + 
    ##     smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
    ##     data = timeseries_analysis %>% filter(insurance %in% c("Private"), 
    ##         race_ethnicity %in% c("White", "Black", "Hispanic")) %>% 
    ##         mutate(race_ethnicity = factor(race_ethnicity, ordered = FALSE), 
    ##             race_ethnicity = relevel(race_ethnicity, ref = "White"), 
    ##             smoke_status = factor(smoke_status), smoke_status = relevel(smoke_status, 
    ##                 ref = "No / Never")))
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -93.375  -3.952   1.733   6.103  38.660 
    ## 
    ## Coefficients:
    ##                                                              Estimate
    ## (Intercept)                                                 43.899482
    ## race_ethnicityBlack                                         -1.398298
    ## race_ethnicityHispanic                                      -1.365116
    ## genderMale                                                   0.546065
    ## genderUnknown                                               -2.682725
    ## first_dr_age                                                -0.129478
    ## smoke_statusFormer / No longer active / Past History / Quit  0.036922
    ## smoke_statusUnknown / Unclassified                          -0.906734
    ## smoke_statusYes / Active                                    -0.923314
    ## baseline_va_letter                                          -0.454402
    ## vegf_group_365combo                                          0.277906
    ## vegf_group_365Eylea                                          0.708741
    ## vegf_group_365Lucentis                                       0.954690
    ## severity_score                                              -0.058369
    ##                                                             Std. Error
    ## (Intercept)                                                   1.145367
    ## race_ethnicityBlack                                           0.347399
    ## race_ethnicityHispanic                                        0.320933
    ## genderMale                                                    0.225860
    ## genderUnknown                                                 1.889086
    ## first_dr_age                                                  0.011341
    ## smoke_statusFormer / No longer active / Past History / Quit   0.277729
    ## smoke_statusUnknown / Unclassified                            0.959857
    ## smoke_statusYes / Active                                      0.365577
    ## baseline_va_letter                                            0.008943
    ## vegf_group_365combo                                           0.290068
    ## vegf_group_365Eylea                                           0.305916
    ## vegf_group_365Lucentis                                        0.352139
    ## severity_score                                                0.007636
    ##                                                             t value
    ## (Intercept)                                                  38.328
    ## race_ethnicityBlack                                          -4.025
    ## race_ethnicityHispanic                                       -4.254
    ## genderMale                                                    2.418
    ## genderUnknown                                                -1.420
    ## first_dr_age                                                -11.417
    ## smoke_statusFormer / No longer active / Past History / Quit   0.133
    ## smoke_statusUnknown / Unclassified                           -0.945
    ## smoke_statusYes / Active                                     -2.526
    ## baseline_va_letter                                          -50.811
    ## vegf_group_365combo                                           0.958
    ## vegf_group_365Eylea                                           2.317
    ## vegf_group_365Lucentis                                        2.711
    ## severity_score                                               -7.644
    ##                                                             Pr(>|t|)    
    ## (Intercept)                                                  < 2e-16 ***
    ## race_ethnicityBlack                                         5.74e-05 ***
    ## race_ethnicityHispanic                                      2.12e-05 ***
    ## genderMale                                                   0.01564 *  
    ## genderUnknown                                                0.15560    
    ## first_dr_age                                                 < 2e-16 ***
    ## smoke_statusFormer / No longer active / Past History / Quit  0.89424    
    ## smoke_statusUnknown / Unclassified                           0.34486    
    ## smoke_statusYes / Active                                     0.01156 *  
    ## baseline_va_letter                                           < 2e-16 ***
    ## vegf_group_365combo                                          0.33805    
    ## vegf_group_365Eylea                                          0.02053 *  
    ## vegf_group_365Lucentis                                       0.00672 ** 
    ## severity_score                                              2.28e-14 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 11.43 on 10760 degrees of freedom
    ## Multiple R-squared:  0.1983, Adjusted R-squared:  0.1973 
    ## F-statistic: 204.7 on 13 and 10760 DF,  p-value: < 2.2e-16

``` r
vif(private_only_one_year_va_delta.lm)
```

    ##                        GVIF Df GVIF^(1/(2*Df))
    ## race_ethnicity     1.032155  2        1.007944
    ## gender             1.023887  2        1.005919
    ## first_dr_age       1.115473  1        1.056159
    ## smoke_status       1.029688  3        1.004888
    ## baseline_va_letter 1.030087  1        1.014932
    ## vegf_group_365     1.041352  3        1.006776
    ## severity_score     1.119685  1        1.058152

``` r
#Significant for both Black and Hispanic patients

Medicare_only_one_year_va_delta.lm <-
  lm(one_year_va_delta ~ race_ethnicity + gender + first_dr_age + smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
     data = timeseries_analysis %>% 
       filter(
         insurance %in% c("Medicare"), 
         race_ethnicity %in% c("White", "Black", "Hispanic")
       ) %>% 
       mutate(
         race_ethnicity = factor(race_ethnicity, ordered = FALSE),
         race_ethnicity = relevel(race_ethnicity, ref = "White"),
         smoke_status = factor(smoke_status),
         smoke_status = relevel(smoke_status, ref = "No / Never")
       )
  ) 
summary(Medicare_only_one_year_va_delta.lm)
```

    ## 
    ## Call:
    ## lm(formula = one_year_va_delta ~ race_ethnicity + gender + first_dr_age + 
    ##     smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
    ##     data = timeseries_analysis %>% filter(insurance %in% c("Medicare"), 
    ##         race_ethnicity %in% c("White", "Black", "Hispanic")) %>% 
    ##         mutate(race_ethnicity = factor(race_ethnicity, ordered = FALSE), 
    ##             race_ethnicity = relevel(race_ethnicity, ref = "White"), 
    ##             smoke_status = factor(smoke_status), smoke_status = relevel(smoke_status, 
    ##                 ref = "No / Never")))
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -97.200  -4.584   1.515   6.548  43.372 
    ## 
    ## Coefficients:
    ##                                                              Estimate
    ## (Intercept)                                                 30.678234
    ## race_ethnicityBlack                                         -0.037361
    ## race_ethnicityHispanic                                      -1.237036
    ## genderMale                                                   0.386604
    ## genderUnknown                                               -0.446759
    ## first_dr_age                                                -0.027565
    ## smoke_statusFormer / No longer active / Past History / Quit  0.163537
    ## smoke_statusUnknown / Unclassified                           1.151950
    ## smoke_statusYes / Active                                    -0.703175
    ## baseline_va_letter                                          -0.366463
    ## vegf_group_365combo                                         -0.065183
    ## vegf_group_365Eylea                                          0.493189
    ## vegf_group_365Lucentis                                       0.722907
    ## severity_score                                              -0.063018
    ##                                                             Std. Error
    ## (Intercept)                                                   0.814791
    ## race_ethnicityBlack                                           0.218238
    ## race_ethnicityHispanic                                        0.234182
    ## genderMale                                                    0.153414
    ## genderUnknown                                                 1.095833
    ## first_dr_age                                                  0.007935
    ## smoke_statusFormer / No longer active / Past History / Quit   0.170677
    ## smoke_statusUnknown / Unclassified                            0.776508
    ## smoke_statusYes / Active                                      0.253981
    ## baseline_va_letter                                            0.005945
    ## vegf_group_365combo                                           0.201192
    ## vegf_group_365Eylea                                           0.204698
    ## vegf_group_365Lucentis                                        0.231756
    ## severity_score                                                0.005097
    ##                                                             t value
    ## (Intercept)                                                  37.652
    ## race_ethnicityBlack                                          -0.171
    ## race_ethnicityHispanic                                       -5.282
    ## genderMale                                                    2.520
    ## genderUnknown                                                -0.408
    ## first_dr_age                                                 -3.474
    ## smoke_statusFormer / No longer active / Past History / Quit   0.958
    ## smoke_statusUnknown / Unclassified                            1.484
    ## smoke_statusYes / Active                                     -2.769
    ## baseline_va_letter                                          -61.641
    ## vegf_group_365combo                                          -0.324
    ## vegf_group_365Eylea                                           2.409
    ## vegf_group_365Lucentis                                        3.119
    ## severity_score                                              -12.365
    ##                                                             Pr(>|t|)    
    ## (Intercept)                                                  < 2e-16 ***
    ## race_ethnicityBlack                                         0.864072    
    ## race_ethnicityHispanic                                      1.29e-07 ***
    ## genderMale                                                  0.011741 *  
    ## genderUnknown                                               0.683505    
    ## first_dr_age                                                0.000514 ***
    ## smoke_statusFormer / No longer active / Past History / Quit 0.337987    
    ## smoke_statusUnknown / Unclassified                          0.137953    
    ## smoke_statusYes / Active                                    0.005633 ** 
    ## baseline_va_letter                                           < 2e-16 ***
    ## vegf_group_365combo                                         0.745951    
    ## vegf_group_365Eylea                                         0.015988 *  
    ## vegf_group_365Lucentis                                      0.001815 ** 
    ## severity_score                                               < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 12.19 on 26645 degrees of freedom
    ## Multiple R-squared:  0.1266, Adjusted R-squared:  0.1262 
    ## F-statistic: 297.2 on 13 and 26645 DF,  p-value: < 2.2e-16

``` r
vif(Medicare_only_one_year_va_delta.lm)
```

    ##                        GVIF Df GVIF^(1/(2*Df))
    ## race_ethnicity     1.060166  2        1.014714
    ## gender             1.050819  2        1.012470
    ## first_dr_age       1.117137  1        1.056947
    ## smoke_status       1.056458  3        1.009196
    ## baseline_va_letter 1.027315  1        1.013565
    ## vegf_group_365     1.044021  3        1.007206
    ## severity_score     1.129783  1        1.062912

``` r
#Significant for Hispanic patients (***)

Medicaid_only_one_year_va_delta.lm <-
  lm(one_year_va_delta ~ race_ethnicity + gender + first_dr_age + smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
     data = timeseries_analysis %>% 
       filter(
         insurance %in% c("Medicaid"), 
         race_ethnicity %in% c("White", "Black", "Hispanic")
       ) %>% 
       mutate(
         race_ethnicity = factor(race_ethnicity, ordered = FALSE),
         race_ethnicity = relevel(race_ethnicity, ref = "White"),
         smoke_status = factor(smoke_status),
         smoke_status = relevel(smoke_status, ref = "No / Never")
       )
  ) 
summary(Medicaid_only_one_year_va_delta.lm)
```

    ## 
    ## Call:
    ## lm(formula = one_year_va_delta ~ race_ethnicity + gender + first_dr_age + 
    ##     smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
    ##     data = timeseries_analysis %>% filter(insurance %in% c("Medicaid"), 
    ##         race_ethnicity %in% c("White", "Black", "Hispanic")) %>% 
    ##         mutate(race_ethnicity = factor(race_ethnicity, ordered = FALSE), 
    ##             race_ethnicity = relevel(race_ethnicity, ref = "White"), 
    ##             smoke_status = factor(smoke_status), smoke_status = relevel(smoke_status, 
    ##                 ref = "No / Never")))
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -90.277  -3.999   2.641   8.007  37.517 
    ## 
    ## Coefficients:
    ##                                                             Estimate
    ## (Intercept)                                                 41.19569
    ## race_ethnicityBlack                                         -1.53871
    ## race_ethnicityHispanic                                      -0.69874
    ## genderMale                                                   0.25909
    ## genderUnknown                                               -3.55764
    ## first_dr_age                                                -0.06422
    ## smoke_statusFormer / No longer active / Past History / Quit  0.05926
    ## smoke_statusUnknown / Unclassified                           5.01472
    ## smoke_statusYes / Active                                     0.62724
    ## baseline_va_letter                                          -0.48018
    ## vegf_group_365combo                                         -0.98410
    ## vegf_group_365Eylea                                          0.59873
    ## vegf_group_365Lucentis                                      -0.23930
    ## severity_score                                              -0.07816
    ##                                                             Std. Error
    ## (Intercept)                                                    2.79012
    ## race_ethnicityBlack                                            0.86336
    ## race_ethnicityHispanic                                         0.69685
    ## genderMale                                                     0.59321
    ## genderUnknown                                                  4.41945
    ## first_dr_age                                                   0.02783
    ## smoke_statusFormer / No longer active / Past History / Quit    0.74490
    ## smoke_statusUnknown / Unclassified                             3.28962
    ## smoke_statusYes / Active                                       0.80646
    ## baseline_va_letter                                             0.02213
    ## vegf_group_365combo                                            0.78374
    ## vegf_group_365Eylea                                            0.97294
    ## vegf_group_365Lucentis                                         1.19213
    ## severity_score                                                 0.02046
    ##                                                             t value
    ## (Intercept)                                                  14.765
    ## race_ethnicityBlack                                          -1.782
    ## race_ethnicityHispanic                                       -1.003
    ## genderMale                                                    0.437
    ## genderUnknown                                                -0.805
    ## first_dr_age                                                 -2.308
    ## smoke_statusFormer / No longer active / Past History / Quit   0.080
    ## smoke_statusUnknown / Unclassified                            1.524
    ## smoke_statusYes / Active                                      0.778
    ## baseline_va_letter                                          -21.697
    ## vegf_group_365combo                                          -1.256
    ## vegf_group_365Eylea                                           0.615
    ## vegf_group_365Lucentis                                       -0.201
    ## severity_score                                               -3.819
    ##                                                             Pr(>|t|)    
    ## (Intercept)                                                  < 2e-16 ***
    ## race_ethnicityBlack                                         0.074833 .  
    ## race_ethnicityHispanic                                      0.316096    
    ## genderMale                                                  0.662321    
    ## genderUnknown                                               0.420899    
    ## first_dr_age                                                0.021073 *  
    ## smoke_statusFormer / No longer active / Past History / Quit 0.936596    
    ## smoke_statusUnknown / Unclassified                          0.127535    
    ## smoke_statusYes / Active                                    0.436779    
    ## baseline_va_letter                                           < 2e-16 ***
    ## vegf_group_365combo                                         0.209362    
    ## vegf_group_365Eylea                                         0.538356    
    ## vegf_group_365Lucentis                                      0.840922    
    ## severity_score                                              0.000137 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 14.58 on 2461 degrees of freedom
    ## Multiple R-squared:  0.1635, Adjusted R-squared:  0.1591 
    ## F-statistic:    37 on 13 and 2461 DF,  p-value: < 2.2e-16

``` r
vif(Medicaid_only_one_year_va_delta.lm)
```

    ##                        GVIF Df GVIF^(1/(2*Df))
    ## race_ethnicity     1.091364  2        1.022098
    ## gender             1.020834  2        1.005168
    ## first_dr_age       1.123947  1        1.060164
    ## smoke_status       1.065120  3        1.010570
    ## baseline_va_letter 1.023087  1        1.011478
    ## vegf_group_365     1.053620  3        1.008743
    ## severity_score     1.095844  1        1.046826

``` r
#Not significant for either Hispanic and Black patients. 


#Insurance analysis: Regress data across only White patients to see effect of insurance on vision: 
white_only_one_year_va_delta.lm <-
  lm(one_year_va_delta ~ insurance + gender + first_dr_age + smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
     data = timeseries_analysis %>% 
       filter(
         insurance %in% c("Medicare", "Medicaid", "Private"), 
         race_ethnicity %in% c("White")
       ) %>% 
       mutate(
         insurance = factor(insurance, ordered = FALSE), 
         insurance = relevel(insurance, ref = "Private"),
         smoke_status = factor(smoke_status),
         smoke_status = relevel(smoke_status, ref = "No / Never")
       )
  ) 
summary(white_only_one_year_va_delta.lm)
```

    ## 
    ## Call:
    ## lm(formula = one_year_va_delta ~ insurance + gender + first_dr_age + 
    ##     smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
    ##     data = timeseries_analysis %>% filter(insurance %in% c("Medicare", 
    ##         "Medicaid", "Private"), race_ethnicity %in% c("White")) %>% 
    ##         mutate(insurance = factor(insurance, ordered = FALSE), 
    ##             insurance = relevel(insurance, ref = "Private"), 
    ##             smoke_status = factor(smoke_status), smoke_status = relevel(smoke_status, 
    ##                 ref = "No / Never")))
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -96.673  -4.310   1.593   6.293  41.544 
    ## 
    ## Coefficients:
    ##                                                              Estimate
    ## (Intercept)                                                 36.678568
    ## insuranceMedicare                                           -1.814743
    ## insuranceMedicaid                                           -2.107611
    ## genderMale                                                   0.545557
    ## genderUnknown                                               -0.893200
    ## first_dr_age                                                -0.055449
    ## smoke_statusFormer / No longer active / Past History / Quit  0.101759
    ## smoke_statusUnknown / Unclassified                           0.721122
    ## smoke_statusYes / Active                                    -0.824296
    ## baseline_va_letter                                          -0.400982
    ## vegf_group_365combo                                         -0.057933
    ## vegf_group_365Eylea                                          0.520591
    ## vegf_group_365Lucentis                                       0.592619
    ## severity_score                                              -0.064501
    ##                                                             Std. Error
    ## (Intercept)                                                   0.723341
    ## insuranceMedicare                                             0.179604
    ## insuranceMedicaid                                             0.344032
    ## genderMale                                                    0.141007
    ## genderUnknown                                                 1.123991
    ## first_dr_age                                                  0.007353
    ## smoke_statusFormer / No longer active / Past History / Quit   0.160224
    ## smoke_statusUnknown / Unclassified                            0.673494
    ## smoke_statusYes / Active                                      0.225337
    ## baseline_va_letter                                            0.005573
    ## vegf_group_365combo                                           0.184154
    ## vegf_group_365Eylea                                           0.186347
    ## vegf_group_365Lucentis                                        0.211954
    ## severity_score                                                0.004751
    ##                                                             t value
    ## (Intercept)                                                  50.707
    ## insuranceMedicare                                           -10.104
    ## insuranceMedicaid                                            -6.126
    ## genderMale                                                    3.869
    ## genderUnknown                                                -0.795
    ## first_dr_age                                                 -7.541
    ## smoke_statusFormer / No longer active / Past History / Quit   0.635
    ## smoke_statusUnknown / Unclassified                            1.071
    ## smoke_statusYes / Active                                     -3.658
    ## baseline_va_letter                                          -71.946
    ## vegf_group_365combo                                          -0.315
    ## vegf_group_365Eylea                                           2.794
    ## vegf_group_365Lucentis                                        2.796
    ## severity_score                                              -13.577
    ##                                                             Pr(>|t|)    
    ## (Intercept)                                                  < 2e-16 ***
    ## insuranceMedicare                                            < 2e-16 ***
    ## insuranceMedicaid                                           9.12e-10 ***
    ## genderMale                                                  0.000110 ***
    ## genderUnknown                                               0.426813    
    ## first_dr_age                                                4.80e-14 ***
    ## smoke_statusFormer / No longer active / Past History / Quit 0.525366    
    ## smoke_statusUnknown / Unclassified                          0.284305    
    ## smoke_statusYes / Active                                    0.000255 ***
    ## baseline_va_letter                                           < 2e-16 ***
    ## vegf_group_365combo                                         0.753076    
    ## vegf_group_365Eylea                                         0.005215 ** 
    ## vegf_group_365Lucentis                                      0.005178 ** 
    ## severity_score                                               < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 11.74 on 28884 degrees of freedom
    ## Multiple R-squared:  0.155,  Adjusted R-squared:  0.1546 
    ## F-statistic: 407.5 on 13 and 28884 DF,  p-value: < 2.2e-16

``` r
vif(white_only_one_year_va_delta.lm)
```

    ##                        GVIF Df GVIF^(1/(2*Df))
    ## insurance          1.412262  2        1.090131
    ## gender             1.028620  2        1.007079
    ## first_dr_age       1.527044  1        1.235736
    ## smoke_status       1.056349  3        1.009178
    ## baseline_va_letter 1.034779  1        1.017241
    ## vegf_group_365     1.028407  3        1.004679
    ## severity_score     1.129030  1        1.062558

``` r
#significant worse for both Medicaid and Medicare patients (***)

black_only_one_year_va_delta.lm <-
  lm(one_year_va_delta ~ insurance + gender + first_dr_age + smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
     data = timeseries_analysis %>% 
       filter(
         insurance %in% c("Medicare", "Medicaid", "Private"), 
         race_ethnicity %in% c("Black")
       ) %>% 
       mutate(
         insurance = factor(insurance, ordered = FALSE), 
         insurance = relevel(insurance, ref = "Private"),
         smoke_status = factor(smoke_status),
         smoke_status = relevel(smoke_status, ref = "No / Never")
       )
  ) 
summary(black_only_one_year_va_delta.lm)
```

    ## 
    ## Call:
    ## lm(formula = one_year_va_delta ~ insurance + gender + first_dr_age + 
    ##     smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
    ##     data = timeseries_analysis %>% filter(insurance %in% c("Medicare", 
    ##         "Medicaid", "Private"), race_ethnicity %in% c("Black")) %>% 
    ##         mutate(insurance = factor(insurance, ordered = FALSE), 
    ##             insurance = relevel(insurance, ref = "Private"), 
    ##             smoke_status = factor(smoke_status), smoke_status = relevel(smoke_status, 
    ##                 ref = "No / Never")))
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -93.414  -4.607   1.671   6.713  41.185 
    ## 
    ## Coefficients:
    ##                                                             Estimate
    ## (Intercept)                                                 31.68939
    ## insuranceMedicare                                           -0.58305
    ## insuranceMedicaid                                           -2.14705
    ## genderMale                                                  -0.09339
    ## genderUnknown                                               -0.78896
    ## first_dr_age                                                -0.04156
    ## smoke_statusFormer / No longer active / Past History / Quit  0.61751
    ## smoke_statusUnknown / Unclassified                           0.89748
    ## smoke_statusYes / Active                                    -0.81305
    ## baseline_va_letter                                          -0.37244
    ## vegf_group_365combo                                         -0.16526
    ## vegf_group_365Eylea                                          0.63878
    ## vegf_group_365Lucentis                                       1.22107
    ## severity_score                                              -0.04859
    ##                                                             Std. Error
    ## (Intercept)                                                    1.75279
    ## insuranceMedicare                                              0.44362
    ## insuranceMedicaid                                              0.75399
    ## genderMale                                                     0.35681
    ## genderUnknown                                                  2.59774
    ## first_dr_age                                                   0.01734
    ## smoke_statusFormer / No longer active / Past History / Quit    0.41343
    ## smoke_statusUnknown / Unclassified                             1.70826
    ## smoke_statusYes / Active                                       0.59132
    ## baseline_va_letter                                             0.01338
    ## vegf_group_365combo                                            0.46055
    ## vegf_group_365Eylea                                            0.48782
    ## vegf_group_365Lucentis                                         0.55560
    ## severity_score                                                 0.01152
    ##                                                             t value
    ## (Intercept)                                                  18.079
    ## insuranceMedicare                                            -1.314
    ## insuranceMedicaid                                            -2.848
    ## genderMale                                                   -0.262
    ## genderUnknown                                                -0.304
    ## first_dr_age                                                 -2.396
    ## smoke_statusFormer / No longer active / Past History / Quit   1.494
    ## smoke_statusUnknown / Unclassified                            0.525
    ## smoke_statusYes / Active                                     -1.375
    ## baseline_va_letter                                          -27.830
    ## vegf_group_365combo                                          -0.359
    ## vegf_group_365Eylea                                           1.309
    ## vegf_group_365Lucentis                                        2.198
    ## severity_score                                               -4.217
    ##                                                             Pr(>|t|)    
    ## (Intercept)                                                  < 2e-16 ***
    ## insuranceMedicare                                            0.18880    
    ## insuranceMedicaid                                            0.00442 ** 
    ## genderMale                                                   0.79353    
    ## genderUnknown                                                0.76136    
    ## first_dr_age                                                 0.01659 *  
    ## smoke_statusFormer / No longer active / Past History / Quit  0.13533    
    ## smoke_statusUnknown / Unclassified                           0.59935    
    ## smoke_statusYes / Active                                     0.16919    
    ## baseline_va_letter                                           < 2e-16 ***
    ## vegf_group_365combo                                          0.71973    
    ## vegf_group_365Eylea                                          0.19043    
    ## vegf_group_365Lucentis                                       0.02801 *  
    ## severity_score                                              2.52e-05 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 12.67 on 5426 degrees of freedom
    ## Multiple R-squared:  0.1271, Adjusted R-squared:  0.125 
    ## F-statistic: 60.75 on 13 and 5426 DF,  p-value: < 2.2e-16

``` r
vif(black_only_one_year_va_delta.lm) 
```

    ##                        GVIF Df GVIF^(1/(2*Df))
    ## insurance          1.226919  2        1.052456
    ## gender             1.036648  2        1.009039
    ## first_dr_age       1.324696  1        1.150954
    ## smoke_status       1.050856  3        1.008302
    ## baseline_va_letter 1.036080  1        1.017880
    ## vegf_group_365     1.035608  3        1.005849
    ## severity_score     1.102943  1        1.050211

``` r
#barely significant worse for Medicaid patients (*)

hispanic_only_one_year_va_delta.lm <-
  lm(one_year_va_delta ~ insurance + gender + first_dr_age + smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
     data = timeseries_analysis %>% 
       filter(
         insurance %in% c("Medicare", "Medicaid", "Private"), 
         race_ethnicity %in% c("Hispanic")
       ) %>% 
       mutate(
         insurance = factor(insurance, ordered = FALSE), 
         insurance = relevel(insurance, ref = "Private"),
         smoke_status = factor(smoke_status),
         smoke_status = relevel(smoke_status, ref = "No / Never")
       )
  ) 
summary(hispanic_only_one_year_va_delta.lm)
```

    ## 
    ## Call:
    ## lm(formula = one_year_va_delta ~ insurance + gender + first_dr_age + 
    ##     smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
    ##     data = timeseries_analysis %>% filter(insurance %in% c("Medicare", 
    ##         "Medicaid", "Private"), race_ethnicity %in% c("Hispanic")) %>% 
    ##         mutate(insurance = factor(insurance, ordered = FALSE), 
    ##             insurance = relevel(insurance, ref = "Private"), 
    ##             smoke_status = factor(smoke_status), smoke_status = relevel(smoke_status, 
    ##                 ref = "No / Never")))
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -92.706  -4.615   2.099   7.195  43.092 
    ## 
    ## Coefficients:
    ##                                                             Estimate
    ## (Intercept)                                                 37.22589
    ## insuranceMedicare                                           -1.61947
    ## insuranceMedicaid                                           -1.58585
    ## genderMale                                                   0.15789
    ## genderUnknown                                               -2.48154
    ## first_dr_age                                                -0.08614
    ## smoke_statusFormer / No longer active / Past History / Quit -0.31908
    ## smoke_statusUnknown / Unclassified                          -0.56906
    ## smoke_statusYes / Active                                     0.58603
    ## baseline_va_letter                                          -0.39655
    ## vegf_group_365combo                                          0.18711
    ## vegf_group_365Eylea                                          0.60123
    ## vegf_group_365Lucentis                                       1.54138
    ## severity_score                                              -0.07149
    ##                                                             Std. Error
    ## (Intercept)                                                    1.85642
    ## insuranceMedicare                                              0.45297
    ## insuranceMedicaid                                              0.62845
    ## genderMale                                                     0.38394
    ## genderUnknown                                                  2.22813
    ## first_dr_age                                                   0.01820
    ## smoke_statusFormer / No longer active / Past History / Quit    0.46129
    ## smoke_statusUnknown / Unclassified                             1.96602
    ## smoke_statusYes / Active                                       0.65720
    ## baseline_va_letter                                             0.01418
    ## vegf_group_365combo                                            0.50524
    ## vegf_group_365Eylea                                            0.61109
    ## vegf_group_365Lucentis                                         0.71267
    ## severity_score                                                 0.01282
    ##                                                             t value
    ## (Intercept)                                                  20.052
    ## insuranceMedicare                                            -3.575
    ## insuranceMedicaid                                            -2.523
    ## genderMale                                                    0.411
    ## genderUnknown                                                -1.114
    ## first_dr_age                                                 -4.733
    ## smoke_statusFormer / No longer active / Past History / Quit  -0.692
    ## smoke_statusUnknown / Unclassified                           -0.289
    ## smoke_statusYes / Active                                      0.892
    ## baseline_va_letter                                          -27.965
    ## vegf_group_365combo                                           0.370
    ## vegf_group_365Eylea                                           0.984
    ## vegf_group_365Lucentis                                        2.163
    ## severity_score                                               -5.576
    ##                                                             Pr(>|t|)    
    ## (Intercept)                                                  < 2e-16 ***
    ## insuranceMedicare                                           0.000353 ***
    ## insuranceMedicaid                                           0.011650 *  
    ## genderMale                                                  0.680916    
    ## genderUnknown                                               0.265442    
    ## first_dr_age                                                2.26e-06 ***
    ## smoke_statusFormer / No longer active / Past History / Quit 0.489152    
    ## smoke_statusUnknown / Unclassified                          0.772250    
    ## smoke_statusYes / Active                                    0.372589    
    ## baseline_va_letter                                           < 2e-16 ***
    ## vegf_group_365combo                                         0.711150    
    ## vegf_group_365Eylea                                         0.325231    
    ## vegf_group_365Lucentis                                      0.030596 *  
    ## severity_score                                              2.57e-08 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 13.79 on 5556 degrees of freedom
    ## Multiple R-squared:  0.1271, Adjusted R-squared:  0.1251 
    ## F-statistic: 62.25 on 13 and 5556 DF,  p-value: < 2.2e-16

``` r
vif(hispanic_only_one_year_va_delta.lm)
```

    ##                        GVIF Df GVIF^(1/(2*Df))
    ## insurance          1.200338  2        1.046709
    ## gender             1.068676  2        1.016744
    ## first_dr_age       1.274551  1        1.128960
    ## smoke_status       1.068853  3        1.011159
    ## baseline_va_letter 1.030526  1        1.015148
    ## vegf_group_365     1.029944  3        1.004930
    ## severity_score     1.097858  1        1.047787

``` r
#significant worse for both Medicaid and Medicare patients (**)


# Analysis:
#   - Hispanic patients have significantly worse VA outcomes after 1-year in privately-insured and Medicare populations, relative to White patients. Black patients hve significantly worse VA outcomes after 1-year in only privately-insured populations. Medicaid-based Hispanic patients and Medicaid-based White patients show no significant difference in 1-year VA outcomes.
# - Across all individual racial groups, Medicaid patients have worse VA outcomes after 1 year relative to privately-insured patients. In White and Hispanic populations, Medicare patients have significantly worse VA outcomes after 1 year relative to privately insured patients.
# 
#   Racial differences in VA change, after controlling for insurance, are evident in the privately-insured population for both Black/Hispanic patients and the Medicaid population for Hispanic patients.
```

## Forest Plots on VA change

#### Forest Plot to Determine Factors Associated with VA Changes within Each Insurance Group

``` r
library(tibble)

#Race analysis: Regress data across only privately-insured patients to see effect of race on vision: 
GRAPHprivate_only_one_year_va_delta.lm <-
  lm(one_year_va_delta ~ race_ethnicity + gender + first_dr_age + smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
     data = timeseries_analysis %>% 
       filter(
         insurance %in% c("Private"), 
         race_ethnicity %in% c("White", "Black", "Hispanic"), 
         gender %in% c("Male", "Female"),
         smoke_status %in% c("Yes / Active", "Former / No longer active / Past History / Quit", "No / Never")
       ) %>% 
       mutate(
         race_ethnicity = factor(race_ethnicity, ordered = FALSE),
         race_ethnicity = relevel(race_ethnicity, ref = "White"),
         smoke_status = factor(smoke_status),
         smoke_status = relevel(smoke_status, ref = "No / Never")
       )
  ) 
summary(GRAPHprivate_only_one_year_va_delta.lm)
```

    ## 
    ## Call:
    ## lm(formula = one_year_va_delta ~ race_ethnicity + gender + first_dr_age + 
    ##     smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
    ##     data = timeseries_analysis %>% filter(insurance %in% c("Private"), 
    ##         race_ethnicity %in% c("White", "Black", "Hispanic"), 
    ##         gender %in% c("Male", "Female"), smoke_status %in% c("Yes / Active", 
    ##             "Former / No longer active / Past History / Quit", 
    ##             "No / Never")) %>% mutate(race_ethnicity = factor(race_ethnicity, 
    ##         ordered = FALSE), race_ethnicity = relevel(race_ethnicity, 
    ##         ref = "White"), smoke_status = factor(smoke_status), 
    ##         smoke_status = relevel(smoke_status, ref = "No / Never")))
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -93.328  -3.976   1.734   6.120  38.607 
    ## 
    ## Coefficients:
    ##                                                              Estimate
    ## (Intercept)                                                 44.022946
    ## race_ethnicityBlack                                         -1.385677
    ## race_ethnicityHispanic                                      -1.310448
    ## genderMale                                                   0.553595
    ## first_dr_age                                                -0.129471
    ## smoke_statusFormer / No longer active / Past History / Quit  0.036103
    ## smoke_statusYes / Active                                    -0.893833
    ## baseline_va_letter                                          -0.456108
    ## vegf_group_365combo                                          0.294099
    ## vegf_group_365Eylea                                          0.703746
    ## vegf_group_365Lucentis                                       0.923121
    ## severity_score                                              -0.058730
    ##                                                             Std. Error
    ## (Intercept)                                                   1.157521
    ## race_ethnicityBlack                                           0.350807
    ## race_ethnicityHispanic                                        0.324795
    ## genderMale                                                    0.227560
    ## first_dr_age                                                  0.011451
    ## smoke_statusFormer / No longer active / Past History / Quit   0.279049
    ## smoke_statusYes / Active                                      0.366991
    ## baseline_va_letter                                            0.009047
    ## vegf_group_365combo                                           0.292897
    ## vegf_group_365Eylea                                           0.309521
    ## vegf_group_365Lucentis                                        0.355699
    ## severity_score                                                0.007713
    ##                                                             t value
    ## (Intercept)                                                  38.032
    ## race_ethnicityBlack                                          -3.950
    ## race_ethnicityHispanic                                       -4.035
    ## genderMale                                                    2.433
    ## first_dr_age                                                -11.307
    ## smoke_statusFormer / No longer active / Past History / Quit   0.129
    ## smoke_statusYes / Active                                     -2.436
    ## baseline_va_letter                                          -50.415
    ## vegf_group_365combo                                           1.004
    ## vegf_group_365Eylea                                           2.274
    ## vegf_group_365Lucentis                                        2.595
    ## severity_score                                               -7.614
    ##                                                             Pr(>|t|)    
    ## (Intercept)                                                  < 2e-16 ***
    ## race_ethnicityBlack                                         7.87e-05 ***
    ## race_ethnicityHispanic                                      5.51e-05 ***
    ## genderMale                                                   0.01500 *  
    ## first_dr_age                                                 < 2e-16 ***
    ## smoke_statusFormer / No longer active / Past History / Quit  0.89706    
    ## smoke_statusYes / Active                                     0.01488 *  
    ## baseline_va_letter                                           < 2e-16 ***
    ## vegf_group_365combo                                          0.31535    
    ## vegf_group_365Eylea                                          0.02301 *  
    ## vegf_group_365Lucentis                                       0.00947 ** 
    ## severity_score                                              2.88e-14 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 11.45 on 10580 degrees of freedom
    ## Multiple R-squared:  0.1981, Adjusted R-squared:  0.1973 
    ## F-statistic: 237.6 on 11 and 10580 DF,  p-value: < 2.2e-16

``` r
vif(GRAPHprivate_only_one_year_va_delta.lm)
```

    ##                        GVIF Df GVIF^(1/(2*Df))
    ## race_ethnicity     1.030891  2        1.007635
    ## gender             1.020152  1        1.010026
    ## first_dr_age       1.114218  1        1.055565
    ## smoke_status       1.027619  2        1.006834
    ## baseline_va_letter 1.030409  1        1.015091
    ## vegf_group_365     1.040503  3        1.006639
    ## severity_score     1.118573  1        1.057626

``` r
#Significant for both Black and Hispanic patients

GRAPHMedicare_only_one_year_va_delta.lm <-
  lm(one_year_va_delta ~ race_ethnicity + gender + first_dr_age + smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
     data = timeseries_analysis %>% 
       filter(
         insurance %in% c("Medicare"), 
         race_ethnicity %in% c("White", "Black", "Hispanic"),
          gender %in% c("Male", "Female"),
         smoke_status %in% c("Yes / Active", "Former / No longer active / Past History / Quit", "No / Never")
       ) %>% 
       mutate(
         race_ethnicity = factor(race_ethnicity, ordered = FALSE),
         race_ethnicity = relevel(race_ethnicity, ref = "White"),
         smoke_status = factor(smoke_status),
         smoke_status = relevel(smoke_status, ref = "No / Never")
       )
  ) 
summary(GRAPHMedicare_only_one_year_va_delta.lm)
```

    ## 
    ## Call:
    ## lm(formula = one_year_va_delta ~ race_ethnicity + gender + first_dr_age + 
    ##     smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
    ##     data = timeseries_analysis %>% filter(insurance %in% c("Medicare"), 
    ##         race_ethnicity %in% c("White", "Black", "Hispanic"), 
    ##         gender %in% c("Male", "Female"), smoke_status %in% c("Yes / Active", 
    ##             "Former / No longer active / Past History / Quit", 
    ##             "No / Never")) %>% mutate(race_ethnicity = factor(race_ethnicity, 
    ##         ordered = FALSE), race_ethnicity = relevel(race_ethnicity, 
    ##         ref = "White"), smoke_status = factor(smoke_status), 
    ##         smoke_status = relevel(smoke_status, ref = "No / Never")))
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -97.178  -4.580   1.514   6.552  43.393 
    ## 
    ## Coefficients:
    ##                                                              Estimate
    ## (Intercept)                                                 30.526717
    ## race_ethnicityBlack                                         -0.044052
    ## race_ethnicityHispanic                                      -1.237605
    ## genderMale                                                   0.400696
    ## first_dr_age                                                -0.025807
    ## smoke_statusFormer / No longer active / Past History / Quit  0.150485
    ## smoke_statusYes / Active                                    -0.683870
    ## baseline_va_letter                                          -0.365665
    ## vegf_group_365combo                                         -0.074501
    ## vegf_group_365Eylea                                          0.482869
    ## vegf_group_365Lucentis                                       0.697120
    ## severity_score                                              -0.063248
    ##                                                             Std. Error
    ## (Intercept)                                                   0.820830
    ## race_ethnicityBlack                                           0.219948
    ## race_ethnicityHispanic                                        0.235887
    ## genderMale                                                    0.154145
    ## first_dr_age                                                  0.007998
    ## smoke_statusFormer / No longer active / Past History / Quit   0.171186
    ## smoke_statusYes / Active                                      0.254749
    ## baseline_va_letter                                            0.005994
    ## vegf_group_365combo                                           0.202651
    ## vegf_group_365Eylea                                           0.206214
    ## vegf_group_365Lucentis                                        0.233334
    ## severity_score                                                0.005130
    ##                                                             t value
    ## (Intercept)                                                  37.190
    ## race_ethnicityBlack                                          -0.200
    ## race_ethnicityHispanic                                       -5.247
    ## genderMale                                                    2.599
    ## first_dr_age                                                 -3.227
    ## smoke_statusFormer / No longer active / Past History / Quit   0.879
    ## smoke_statusYes / Active                                     -2.684
    ## baseline_va_letter                                          -61.001
    ## vegf_group_365combo                                          -0.368
    ## vegf_group_365Eylea                                           2.342
    ## vegf_group_365Lucentis                                        2.988
    ## severity_score                                              -12.329
    ##                                                             Pr(>|t|)    
    ## (Intercept)                                                  < 2e-16 ***
    ## race_ethnicityBlack                                          0.84126    
    ## race_ethnicityHispanic                                      1.56e-07 ***
    ## genderMale                                                   0.00934 ** 
    ## first_dr_age                                                 0.00125 ** 
    ## smoke_statusFormer / No longer active / Past History / Quit  0.37937    
    ## smoke_statusYes / Active                                     0.00727 ** 
    ## baseline_va_letter                                           < 2e-16 ***
    ## vegf_group_365combo                                          0.71315    
    ## vegf_group_365Eylea                                          0.01921 *  
    ## vegf_group_365Lucentis                                       0.00281 ** 
    ## severity_score                                               < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 12.19 on 26272 degrees of freedom
    ## Multiple R-squared:  0.1259, Adjusted R-squared:  0.1255 
    ## F-statistic:   344 on 11 and 26272 DF,  p-value: < 2.2e-16

``` r
vif(GRAPHMedicare_only_one_year_va_delta.lm)
```

    ##                        GVIF Df GVIF^(1/(2*Df))
    ## race_ethnicity     1.059675  2        1.014596
    ## gender             1.049820  1        1.024607
    ## first_dr_age       1.116705  1        1.056743
    ## smoke_status       1.055533  2        1.013603
    ## baseline_va_letter 1.027051  1        1.013435
    ## vegf_group_365     1.043583  3        1.007135
    ## severity_score     1.128220  1        1.062177

``` r
#Significant for Hispanic patients (***)

GRAPHMedicaid_only_one_year_va_delta.lm <-
  lm(one_year_va_delta ~ race_ethnicity + gender + first_dr_age + smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
     data = timeseries_analysis %>% 
       filter(
         insurance %in% c("Medicaid"), 
         race_ethnicity %in% c("White", "Black", "Hispanic"),
        gender %in% c("Male", "Female"),
         smoke_status %in% c("Yes / Active", "Former / No longer active / Past History / Quit", "No / Never")
       ) %>% 
       mutate(
         race_ethnicity = factor(race_ethnicity, ordered = FALSE),
         race_ethnicity = relevel(race_ethnicity, ref = "White"),
         smoke_status = factor(smoke_status),
         smoke_status = relevel(smoke_status, ref = "No / Never")
       )
  ) 
summary(GRAPHMedicaid_only_one_year_va_delta.lm)
```

    ## 
    ## Call:
    ## lm(formula = one_year_va_delta ~ race_ethnicity + gender + first_dr_age + 
    ##     smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
    ##     data = timeseries_analysis %>% filter(insurance %in% c("Medicaid"), 
    ##         race_ethnicity %in% c("White", "Black", "Hispanic"), 
    ##         gender %in% c("Male", "Female"), smoke_status %in% c("Yes / Active", 
    ##             "Former / No longer active / Past History / Quit", 
    ##             "No / Never")) %>% mutate(race_ethnicity = factor(race_ethnicity, 
    ##         ordered = FALSE), race_ethnicity = relevel(race_ethnicity, 
    ##         ref = "White"), smoke_status = factor(smoke_status), 
    ##         smoke_status = relevel(smoke_status, ref = "No / Never")))
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -90.218  -3.983   2.656   8.017  37.569 
    ## 
    ## Coefficients:
    ##                                                             Estimate
    ## (Intercept)                                                 40.86120
    ## race_ethnicityBlack                                         -1.44693
    ## race_ethnicityHispanic                                      -0.60882
    ## genderMale                                                   0.22206
    ## first_dr_age                                                -0.06296
    ## smoke_statusFormer / No longer active / Past History / Quit  0.13186
    ## smoke_statusYes / Active                                     0.67077
    ## baseline_va_letter                                          -0.47767
    ## vegf_group_365combo                                         -0.90933
    ## vegf_group_365Eylea                                          0.67655
    ## vegf_group_365Lucentis                                      -0.21172
    ## severity_score                                              -0.07763
    ##                                                             Std. Error
    ## (Intercept)                                                    2.81509
    ## race_ethnicityBlack                                            0.87017
    ## race_ethnicityHispanic                                         0.70384
    ## genderMale                                                     0.59728
    ## first_dr_age                                                   0.02805
    ## smoke_statusFormer / No longer active / Past History / Quit    0.74978
    ## smoke_statusYes / Active                                       0.80960
    ## baseline_va_letter                                             0.02235
    ## vegf_group_365combo                                            0.79115
    ## vegf_group_365Eylea                                            0.98398
    ## vegf_group_365Lucentis                                         1.19648
    ## severity_score                                                 0.02063
    ##                                                             t value
    ## (Intercept)                                                  14.515
    ## race_ethnicityBlack                                          -1.663
    ## race_ethnicityHispanic                                       -0.865
    ## genderMale                                                    0.372
    ## first_dr_age                                                 -2.244
    ## smoke_statusFormer / No longer active / Past History / Quit   0.176
    ## smoke_statusYes / Active                                      0.829
    ## baseline_va_letter                                          -21.375
    ## vegf_group_365combo                                          -1.149
    ## vegf_group_365Eylea                                           0.688
    ## vegf_group_365Lucentis                                       -0.177
    ## severity_score                                               -3.763
    ##                                                             Pr(>|t|)    
    ## (Intercept)                                                  < 2e-16 ***
    ## race_ethnicityBlack                                         0.096477 .  
    ## race_ethnicityHispanic                                      0.387128    
    ## genderMale                                                  0.710079    
    ## first_dr_age                                                0.024898 *  
    ## smoke_statusFormer / No longer active / Past History / Quit 0.860416    
    ## smoke_statusYes / Active                                    0.407457    
    ## baseline_va_letter                                           < 2e-16 ***
    ## vegf_group_365combo                                         0.250510    
    ## vegf_group_365Eylea                                         0.491794    
    ## vegf_group_365Lucentis                                      0.859558    
    ## severity_score                                              0.000172 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 14.63 on 2432 degrees of freedom
    ## Multiple R-squared:  0.1598, Adjusted R-squared:  0.156 
    ## F-statistic: 42.06 on 11 and 2432 DF,  p-value: < 2.2e-16

``` r
vif(GRAPHMedicaid_only_one_year_va_delta.lm)
```

    ##                        GVIF Df GVIF^(1/(2*Df))
    ## race_ethnicity     1.091406  2        1.022108
    ## gender             1.016730  1        1.008330
    ## first_dr_age       1.119974  1        1.058288
    ## smoke_status       1.060295  2        1.014744
    ## baseline_va_letter 1.024062  1        1.011959
    ## vegf_group_365     1.054134  3        1.008825
    ## severity_score     1.095238  1        1.046536

``` r
#Not significant for either Hispanic and Black patients. 



#Extract values from regression output
private_output <- summary(GRAPHprivate_only_one_year_va_delta.lm)$coefficients[1:12,]

#Convert values into data frame
private_output = as.data.frame(private_output)
private_output <- tibble::rownames_to_column(private_output, "FACTOR")

#Create limits for values
private_output$upper_limit = private_output$Estimate + 1.96*private_output$`Std. Error`
private_output$lower_limit = private_output$Estimate - 1.96*private_output$`Std. Error`

#Add a group name
private_output$group <- "private"
private_output$FACTOR_clean <- c("Intercept", "Black (rel to White)", "Hispanic (rel to White)", "Male (rel to Female)", "DR Diag. Age", "Former Smoker (rel to Non-Smoker)", "Current Smoker (rel to Non-Smoker)", "Baseline VA", "Combo (rel to Avastin)", "Aflibercept (rel to Bevacizumab)", "Ranibizumab (rel to Bevacizumab)", "Severity Score")

View(private_output)


#Repeat steps for other groups:

medicare_output <- summary(GRAPHMedicare_only_one_year_va_delta.lm)$coefficients[1:12,]
medicare_output = as.data.frame(medicare_output)
medicare_output <- tibble::rownames_to_column(medicare_output, "FACTOR")
medicare_output$upper_limit = medicare_output$Estimate + 1.96*medicare_output$`Std. Error`
medicare_output$lower_limit = medicare_output$Estimate - 1.96*medicare_output$`Std. Error`
medicare_output$group <- "medicare"
medicare_output$FACTOR_clean <- c("Intercept", "Black (rel to White)", "Hispanic (rel to White)", "Male (rel to Female)", "DR Diag. Age", "Former Smoker (rel to Non-Smoker)", "Current Smoker (rel to Non-Smoker)", "Baseline VA", "Combo (rel to Avastin)", "Aflibercept (rel to Bevacizumab)", "Ranibizumab (rel to Bevacizumab)", "Severity Score")
#Convert group name into a factor

View(medicare_output)


medicaid_output <- summary(GRAPHMedicaid_only_one_year_va_delta.lm)$coefficients[1:12,]
medicaid_output = as.data.frame(medicaid_output)
medicaid_output <- tibble::rownames_to_column(medicaid_output, "FACTOR")
medicaid_output$upper_limit = medicaid_output$Estimate + 1.96*medicaid_output$`Std. Error`
medicaid_output$lower_limit = medicaid_output$Estimate - 1.96*medicaid_output$`Std. Error`
medicaid_output$group <- "medicaid"
medicaid_output$FACTOR_clean <- c("Intercept", "Black (rel to White)", "Hispanic (rel to White)", "Male (rel to Female)", "DR Diag. Age", "Former Smoker (rel to Non-Smoker)", "Current Smoker (rel to Non-Smoker)", "Baseline VA", "Combo (rel to Avastin)", "Aflibercept (rel to Bevacizumab)", "Ranibizumab (rel to Bevacizumab)", "Severity Score")
#Convert group name into a factor


#Combine all group output information
combined_output = rbind(medicaid_output,medicare_output, private_output)

#Remove original factor names that created unaesthetic graph
combined_output = subset(combined_output, select = -c(FACTOR) )
combined_output$FACTOR_clean = factor(medicaid_output$FACTOR_clean, levels = c("Severity Score", "Ranibizumab (rel to Bevacizumab)", "Aflibercept (rel to Bevacizumab)", "Combo (rel to Avastin)", "Baseline VA", "Current Smoker (rel to Non-Smoker)", "Former Smoker (rel to Non-Smoker)",  "DR Diag. Age", "Male (rel to Female)","Hispanic (rel to White)", "Black (rel to White)", "Intercept"))

#Remove intercept values to allow for graph interpretation
combined_output<-combined_output[!(combined_output$FACTOR_clean=="Intercept"),]

#Create factor for group name to order the facet grids
combined_output$group = factor(combined_output$group, levels=c("private","medicare","medicaid"), labels = c("Private", "Medicare", "Medicaid")) 
```

``` r
forest_va_change_insurance <- 
  ggplot(combined_output, aes(y = FACTOR_clean, x = Estimate))+
  geom_point() +
  facet_wrap(~group)+
  geom_segment(aes(x = lower_limit, xend = upper_limit, yend = FACTOR_clean))+
  geom_vline(lty = 2, aes(xintercept=0), colour = 'red') +
  theme_bw() +
  theme(
    plot.title = element_text(hjust = 0.5),
    # remove background lines
    panel.grid.minor = element_blank(),
    panel.grid.major = element_blank()
  ) +
  labs(
    y = "Factor", 
    x = "Change in VA after 1 year", 
    title = "Factors Associated with Changes in VA by Insurance"
  )

forest_va_change_insurance
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-36-1.png)<!-- -->

``` r
#ggsave("graphs/forest_va_change_insurance.png", forest_va_change_insurance, width = 10, height = 5)
```

#### Forest Plot to Determine Factors Associated with VA Changes within Each Race

``` r
#Insurance analysis: Regress data across only privately-insured patients to see effect of race on vision: 
GRAPHwhite_only_one_year_va_delta.lm <-
  lm(one_year_va_delta ~ insurance + gender + first_dr_age + smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
     data = timeseries_analysis %>% 
       filter(
         insurance %in% c("Medicare", "Medicaid", "Private"), 
         race_ethnicity %in% c("White"), 
         gender %in% c("Male", "Female"),
         smoke_status %in% c("Yes / Active", "Former / No longer active / Past History / Quit", "No / Never")
       ) %>% 
       mutate(
         insurance = factor(insurance, ordered = FALSE), 
         insurance = relevel(insurance, ref = "Private"),
         smoke_status = factor(smoke_status),
         smoke_status = relevel(smoke_status, ref = "No / Never")
       )
  ) 

# COUNT_for_Regression <-
#   timeseries_analysis %>%
#        filter(
#          insurance %in% c("Medicare", "Medicaid", "Private"),
#          race_ethnicity %in% c("White", "Black", "Hispanic"),
#          gender %in% c("Male", "Female"),
#          smoke_status %in% c("Yes / Active", "Former / No longer active / Past History / Quit", "No / Never")
#        ) %>%
#        mutate(
#          insurance = factor(insurance, ordered = FALSE),
#          insurance = relevel(insurance, ref = "Private"),
#          smoke_status = factor(smoke_status),
#          smoke_status = relevel(smoke_status, ref = "No / Never")
#        )
# 
# count(COUNT_for_Regression)

summary(GRAPHwhite_only_one_year_va_delta.lm)
```

    ## 
    ## Call:
    ## lm(formula = one_year_va_delta ~ insurance + gender + first_dr_age + 
    ##     smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
    ##     data = timeseries_analysis %>% filter(insurance %in% c("Medicare", 
    ##         "Medicaid", "Private"), race_ethnicity %in% c("White"), 
    ##         gender %in% c("Male", "Female"), smoke_status %in% c("Yes / Active", 
    ##             "Former / No longer active / Past History / Quit", 
    ##             "No / Never")) %>% mutate(insurance = factor(insurance, 
    ##         ordered = FALSE), insurance = relevel(insurance, ref = "Private"), 
    ##         smoke_status = factor(smoke_status), smoke_status = relevel(smoke_status, 
    ##             ref = "No / Never")))
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -96.635  -4.312   1.597   6.295  41.523 
    ## 
    ## Coefficients:
    ##                                                              Estimate
    ## (Intercept)                                                 36.597481
    ## insuranceMedicare                                           -1.855450
    ## insuranceMedicaid                                           -2.188447
    ## genderMale                                                   0.549090
    ## first_dr_age                                                -0.053679
    ## smoke_statusFormer / No longer active / Past History / Quit  0.091660
    ## smoke_statusYes / Active                                    -0.786128
    ## baseline_va_letter                                          -0.400699
    ## vegf_group_365combo                                         -0.055629
    ## vegf_group_365Eylea                                          0.506237
    ## vegf_group_365Lucentis                                       0.576628
    ## severity_score                                              -0.064761
    ##                                                             Std. Error
    ## (Intercept)                                                   0.729228
    ## insuranceMedicare                                             0.181213
    ## insuranceMedicaid                                             0.346726
    ## genderMale                                                    0.141740
    ## first_dr_age                                                  0.007413
    ## smoke_statusFormer / No longer active / Past History / Quit   0.160723
    ## smoke_statusYes / Active                                      0.225874
    ## baseline_va_letter                                            0.005622
    ## vegf_group_365combo                                           0.185592
    ## vegf_group_365Eylea                                           0.187919
    ## vegf_group_365Lucentis                                        0.213541
    ## severity_score                                                0.004787
    ##                                                             t value
    ## (Intercept)                                                  50.187
    ## insuranceMedicare                                           -10.239
    ## insuranceMedicaid                                            -6.312
    ## genderMale                                                    3.874
    ## first_dr_age                                                 -7.241
    ## smoke_statusFormer / No longer active / Past History / Quit   0.570
    ## smoke_statusYes / Active                                     -3.480
    ## baseline_va_letter                                          -71.272
    ## vegf_group_365combo                                          -0.300
    ## vegf_group_365Eylea                                           2.694
    ## vegf_group_365Lucentis                                        2.700
    ## severity_score                                              -13.528
    ##                                                             Pr(>|t|)    
    ## (Intercept)                                                  < 2e-16 ***
    ## insuranceMedicare                                            < 2e-16 ***
    ## insuranceMedicaid                                           2.80e-10 ***
    ## genderMale                                                  0.000107 ***
    ## first_dr_age                                                4.56e-13 ***
    ## smoke_statusFormer / No longer active / Past History / Quit 0.568483    
    ## smoke_statusYes / Active                                    0.000501 ***
    ## baseline_va_letter                                           < 2e-16 ***
    ## vegf_group_365combo                                         0.764379    
    ## vegf_group_365Eylea                                         0.007066 ** 
    ## vegf_group_365Lucentis                                      0.006932 ** 
    ## severity_score                                               < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 11.75 on 28467 degrees of freedom
    ## Multiple R-squared:  0.1544, Adjusted R-squared:  0.1541 
    ## F-statistic: 472.7 on 11 and 28467 DF,  p-value: < 2.2e-16

``` r
#Extract values from regression output
white_output <- summary(GRAPHwhite_only_one_year_va_delta.lm)$coefficients[1:12,]

#Convert values into data frame
white_output = as.data.frame(white_output)
white_output <- tibble::rownames_to_column(white_output, "FACTOR")

#Create limits for values
white_output$upper_limit = white_output$Estimate + 1.96*white_output$`Std. Error`
white_output$lower_limit = white_output$Estimate - 1.96*white_output$`Std. Error`

#Add a group name
white_output$group <- "white"
white_output$FACTOR_clean <- c("Intercept", "Medicare (rel to Private)", "Medicaid (rel to Private)", "Male (rel to Female)", "DR Diag. Age", "Former Smoker (rel to Non-Smoker)", "Current Smoker (rel to Non-Smoker)", "Baseline VA", "Combo (rel to Bevacizumab)", "Aflibercept (rel to Bevacizumab)", "Ranibizumab (rel to Bevacizumab)", "Severity Score")


#Repeat steps for other groups:

GRAPHblack_only_one_year_va_delta.lm <-
  lm(one_year_va_delta ~ insurance + gender + first_dr_age + smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
     data = timeseries_analysis %>% 
       filter(
         insurance %in% c("Medicare", "Medicaid", "Private"), 
         race_ethnicity %in% c("Black"), 
         gender %in% c("Male", "Female"),
         smoke_status %in% c("Yes / Active", "Former / No longer active / Past History / Quit", "No / Never")
       ) %>% 
       mutate(
         insurance = factor(insurance, ordered = FALSE), 
         insurance = relevel(insurance, ref = "Private"),
         smoke_status = factor(smoke_status),
         smoke_status = relevel(smoke_status, ref = "No / Never")
       )
  ) 
summary(GRAPHblack_only_one_year_va_delta.lm)
```

    ## 
    ## Call:
    ## lm(formula = one_year_va_delta ~ insurance + gender + first_dr_age + 
    ##     smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
    ##     data = timeseries_analysis %>% filter(insurance %in% c("Medicare", 
    ##         "Medicaid", "Private"), race_ethnicity %in% c("Black"), 
    ##         gender %in% c("Male", "Female"), smoke_status %in% c("Yes / Active", 
    ##             "Former / No longer active / Past History / Quit", 
    ##             "No / Never")) %>% mutate(insurance = factor(insurance, 
    ##         ordered = FALSE), insurance = relevel(insurance, ref = "Private"), 
    ##         smoke_status = factor(smoke_status), smoke_status = relevel(smoke_status, 
    ##             ref = "No / Never")))
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -93.351  -4.609   1.661   6.726  41.145 
    ## 
    ## Coefficients:
    ##                                                             Estimate
    ## (Intercept)                                                 31.45010
    ## insuranceMedicare                                           -0.66277
    ## insuranceMedicaid                                           -2.12972
    ## genderMale                                                  -0.05514
    ## first_dr_age                                                -0.03718
    ## smoke_statusFormer / No longer active / Past History / Quit  0.62566
    ## smoke_statusYes / Active                                    -0.81742
    ## baseline_va_letter                                          -0.37356
    ## vegf_group_365combo                                         -0.17280
    ## vegf_group_365Eylea                                          0.65992
    ## vegf_group_365Lucentis                                       1.16720
    ## severity_score                                              -0.04714
    ##                                                             Std. Error
    ## (Intercept)                                                    1.77072
    ## insuranceMedicare                                              0.44839
    ## insuranceMedicaid                                              0.75943
    ## genderMale                                                     0.35963
    ## first_dr_age                                                   0.01753
    ## smoke_statusFormer / No longer active / Past History / Quit    0.41569
    ## smoke_statusYes / Active                                       0.59437
    ## baseline_va_letter                                             0.01353
    ## vegf_group_365combo                                            0.46442
    ## vegf_group_365Eylea                                            0.49254
    ## vegf_group_365Lucentis                                         0.56101
    ## severity_score                                                 0.01162
    ##                                                             t value
    ## (Intercept)                                                  17.761
    ## insuranceMedicare                                            -1.478
    ## insuranceMedicaid                                            -2.804
    ## genderMale                                                   -0.153
    ## first_dr_age                                                 -2.121
    ## smoke_statusFormer / No longer active / Past History / Quit   1.505
    ## smoke_statusYes / Active                                     -1.375
    ## baseline_va_letter                                          -27.607
    ## vegf_group_365combo                                          -0.372
    ## vegf_group_365Eylea                                           1.340
    ## vegf_group_365Lucentis                                        2.081
    ## severity_score                                               -4.056
    ##                                                             Pr(>|t|)    
    ## (Intercept)                                                  < 2e-16 ***
    ## insuranceMedicare                                            0.13944    
    ## insuranceMedicaid                                            0.00506 ** 
    ## genderMale                                                   0.87816    
    ## first_dr_age                                                 0.03397 *  
    ## smoke_statusFormer / No longer active / Past History / Quit  0.13235    
    ## smoke_statusYes / Active                                     0.16911    
    ## baseline_va_letter                                           < 2e-16 ***
    ## vegf_group_365combo                                          0.70985    
    ## vegf_group_365Eylea                                          0.18036    
    ## vegf_group_365Lucentis                                       0.03752 *  
    ## severity_score                                              5.07e-05 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 12.7 on 5348 degrees of freedom
    ## Multiple R-squared:  0.1268, Adjusted R-squared:  0.125 
    ## F-statistic: 70.62 on 11 and 5348 DF,  p-value: < 2.2e-16

``` r
#Extract values from regression output
black_output <- summary(GRAPHblack_only_one_year_va_delta.lm)$coefficients[1:12,]

#Convert values into data frame
black_output = as.data.frame(black_output)
black_output <- tibble::rownames_to_column(black_output, "FACTOR")

#Create limits for values
black_output$upper_limit = black_output$Estimate + 1.96*black_output$`Std. Error`
black_output$lower_limit = black_output$Estimate - 1.96*black_output$`Std. Error`

#Add a group name
black_output$group <- "black"
black_output$FACTOR_clean <- c("Intercept", "Medicare (rel to Private)", "Medicaid (rel to Private)", "Male (rel to Female)", "DR Diag. Age", "Former Smoker (rel to Non-Smoker)", "Current Smoker (rel to Non-Smoker)", "Baseline VA", "Combo (rel to Bevacizumab)", "Aflibercept (rel to Bevacizumab)", "Ranibizumab (rel to Bevacizumab)", "Severity Score")

GRAPHhispanic_only_one_year_va_delta.lm <-
  lm(one_year_va_delta ~ insurance + gender + first_dr_age + smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
     data = timeseries_analysis %>% 
       filter(
         insurance %in% c("Medicare", "Medicaid", "Private"), 
         race_ethnicity %in% c("Hispanic"), 
         gender %in% c("Male", "Female"),
         smoke_status %in% c("Yes / Active", "Former / No longer active / Past History / Quit", "No / Never")
       ) %>% 
       mutate(
         insurance = factor(insurance, ordered = FALSE), 
         insurance = relevel(insurance, ref = "Private"),
         smoke_status = factor(smoke_status),
         smoke_status = relevel(smoke_status, ref = "No / Never")
       )
  ) 
summary(GRAPHhispanic_only_one_year_va_delta.lm)
```

    ## 
    ## Call:
    ## lm(formula = one_year_va_delta ~ insurance + gender + first_dr_age + 
    ##     smoke_status + baseline_va_letter + vegf_group_365 + severity_score, 
    ##     data = timeseries_analysis %>% filter(insurance %in% c("Medicare", 
    ##         "Medicaid", "Private"), race_ethnicity %in% c("Hispanic"), 
    ##         gender %in% c("Male", "Female"), smoke_status %in% c("Yes / Active", 
    ##             "Former / No longer active / Past History / Quit", 
    ##             "No / Never")) %>% mutate(insurance = factor(insurance, 
    ##         ordered = FALSE), insurance = relevel(insurance, ref = "Private"), 
    ##         smoke_status = factor(smoke_status), smoke_status = relevel(smoke_status, 
    ##             ref = "No / Never")))
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -92.750  -4.592   2.082   7.150  43.141 
    ## 
    ## Coefficients:
    ##                                                             Estimate
    ## (Intercept)                                                 37.34084
    ## insuranceMedicare                                           -1.66855
    ## insuranceMedicaid                                           -1.61652
    ## genderMale                                                   0.15340
    ## first_dr_age                                                -0.08835
    ## smoke_statusFormer / No longer active / Past History / Quit -0.29726
    ## smoke_statusYes / Active                                     0.55467
    ## baseline_va_letter                                          -0.39415
    ## vegf_group_365combo                                          0.19229
    ## vegf_group_365Eylea                                          0.62016
    ## vegf_group_365Lucentis                                       1.49365
    ## severity_score                                              -0.07319
    ##                                                             Std. Error
    ## (Intercept)                                                    1.86951
    ## insuranceMedicare                                              0.45793
    ## insuranceMedicaid                                              0.63389
    ## genderMale                                                     0.38618
    ## first_dr_age                                                   0.01833
    ## smoke_statusFormer / No longer active / Past History / Quit    0.46393
    ## smoke_statusYes / Active                                       0.66157
    ## baseline_va_letter                                             0.01433
    ## vegf_group_365combo                                            0.51006
    ## vegf_group_365Eylea                                            0.61778
    ## vegf_group_365Lucentis                                         0.71701
    ## severity_score                                                 0.01291
    ##                                                             t value
    ## (Intercept)                                                  19.974
    ## insuranceMedicare                                            -3.644
    ## insuranceMedicaid                                            -2.550
    ## genderMale                                                    0.397
    ## first_dr_age                                                 -4.820
    ## smoke_statusFormer / No longer active / Past History / Quit  -0.641
    ## smoke_statusYes / Active                                      0.838
    ## baseline_va_letter                                          -27.514
    ## vegf_group_365combo                                           0.377
    ## vegf_group_365Eylea                                           1.004
    ## vegf_group_365Lucentis                                        2.083
    ## severity_score                                               -5.669
    ##                                                             Pr(>|t|)    
    ## (Intercept)                                                  < 2e-16 ***
    ## insuranceMedicare                                           0.000271 ***
    ## insuranceMedicaid                                           0.010794 *  
    ## genderMale                                                  0.691212    
    ## first_dr_age                                                1.48e-06 ***
    ## smoke_statusFormer / No longer active / Past History / Quit 0.521711    
    ## smoke_statusYes / Active                                    0.401836    
    ## baseline_va_letter                                           < 2e-16 ***
    ## vegf_group_365combo                                         0.706191    
    ## vegf_group_365Eylea                                         0.315497    
    ## vegf_group_365Lucentis                                      0.037283 *  
    ## severity_score                                              1.51e-08 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 13.8 on 5469 degrees of freedom
    ## Multiple R-squared:  0.1254, Adjusted R-squared:  0.1236 
    ## F-statistic: 71.28 on 11 and 5469 DF,  p-value: < 2.2e-16

``` r
#Extract values from regression output
hispanic_output <- summary(GRAPHhispanic_only_one_year_va_delta.lm)$coefficients[1:12,]

#Convert values into data frame
hispanic_output = as.data.frame(hispanic_output)
hispanic_output <- tibble::rownames_to_column(hispanic_output, "FACTOR")

#Create limits for values
hispanic_output$upper_limit = hispanic_output$Estimate + 1.96*hispanic_output$`Std. Error`
hispanic_output$lower_limit = hispanic_output$Estimate - 1.96*hispanic_output$`Std. Error`

#Add a group name
hispanic_output$group <- "hispanic"
hispanic_output$FACTOR_clean <- c("Intercept", "Medicare (rel to Private)", "Medicaid (rel to Private)", "Male (rel to Female)", "DR Diag. Age", "Former Smoker (rel to Non-Smoker)", "Current Smoker (rel to Non-Smoker)", "Baseline VA", "Combo (rel to Bevacizumab)", "Aflibercept (rel to Bevacizumab)", "Ranibizumab (rel to Bevacizumab)", "Severity Score")


#Combine all group output information
combined_race_output = rbind(white_output, black_output, hispanic_output)

#Remove original factor names that created unaesthetic graph
combined_race_output = subset(combined_race_output, select = -c(FACTOR) )

#Remove intercept values to allow for graph interpretation
combined_race_output<-combined_race_output[!(combined_race_output$FACTOR_clean=="Intercept"),]

#Create factor for group name to order the facet grids
combined_race_output$group = factor(combined_race_output$group, levels=c("white","black","hispanic"), labels = c("White", "Black", "Hispanic")) 
combined_race_output$FACTOR_clean = factor(combined_race_output$FACTOR_clean, levels = c("Severity Score", "Ranibizumab (rel to Bevacizumab)", "Aflibercept (rel to Bevacizumab)", "Combo (rel to Bevacizumab)", "Baseline VA", "Current Smoker (rel to Non-Smoker)", "Former Smoker (rel to Non-Smoker)", "DR Diag. Age", "Male (rel to Female)","Medicaid (rel to Private)", "Medicare (rel to Private)", "Intercept"))
```

``` r
vif(GRAPHwhite_only_one_year_va_delta.lm)
```

    ##                        GVIF Df GVIF^(1/(2*Df))
    ## insurance          1.412988  2        1.090271
    ## gender             1.026969  1        1.013395
    ## first_dr_age       1.527236  1        1.235814
    ## smoke_status       1.055012  2        1.013478
    ## baseline_va_letter 1.034570  1        1.017138
    ## vegf_group_365     1.028014  3        1.004615
    ## severity_score     1.128246  1        1.062189

``` r
vif(GRAPHblack_only_one_year_va_delta.lm)
```

    ##                        GVIF Df GVIF^(1/(2*Df))
    ## insurance          1.228659  2        1.052829
    ## gender             1.033992  1        1.016854
    ## first_dr_age       1.328362  1        1.152546
    ## smoke_status       1.048800  2        1.011983
    ## baseline_va_letter 1.036600  1        1.018135
    ## vegf_group_365     1.034459  3        1.005662
    ## severity_score     1.102111  1        1.049815

``` r
vif(GRAPHhispanic_only_one_year_va_delta.lm)
```

    ##                        GVIF Df GVIF^(1/(2*Df))
    ## insurance          1.201247  2        1.046907
    ## gender             1.067911  1        1.033398
    ## first_dr_age       1.273472  1        1.128482
    ## smoke_status       1.066041  2        1.016116
    ## baseline_va_letter 1.030726  1        1.015247
    ## vegf_group_365     1.029631  3        1.004879
    ## severity_score     1.095979  1        1.046890

``` r
forest_va_change_race <- 
  ggplot(combined_race_output,aes(y = FACTOR_clean, x = Estimate))+
  geom_point()+
  facet_wrap(~group)+
  geom_segment(aes(x = lower_limit, xend = upper_limit, yend = FACTOR_clean))+
  geom_vline(lty = 2, aes(xintercept=0), colour = 'red') +
  theme_bw() +
  theme(
    plot.title = element_text(hjust = 0.5),
    # remove background lines
    panel.grid.minor = element_blank(),
    panel.grid.major = element_blank()
  ) +
  labs(
    y= "Factor", 
    x = "Change in VA after 1 year",
    title = "Factors Associated with Changes in VA by Race"
  )

forest_va_change_race
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-39-1.png)<!-- -->

``` r
#ggsave("graphs/forest_va_change_race.png", forest_va_change_race, width = 10, height = 5)
```

``` r
#Old Forest Plot attempts
# 
# race_colors <- c("#54AE55", "#189CD9", "#A167A5")
# insurance_colors <- c("#A00033", "#D1603D", "#F7D002")
# 
# dotCOLS = c("#a6d8f0","#f9b282")
# barCOLS = c("#A00033","#D1603D")
# 
# p <- ggplot(combined_output, aes(x=FACTOR_clean, y=Estimate, ymin=lower_limit, ymax=upper_limit,col=group,fill=group)) + 
# #specify position here
#   geom_errorbar(size=2,position=position_dodge(width = 0.5)) +
#   geom_hline(yintercept=0, lty=2) +
# #specify position here too
#   geom_point(size=1, shape=21, colour="white", stroke = 0.5,position=position_dodge(width = 0.5)) +
#   scale_fill_manual(values = insurance_colors) +
#   scale_color_manual(values=insurance_colors)+
#   scale_y_continuous(name="Change in VA", limits = c(-7, 3)) +
#   coord_flip() +
#   theme_minimal()
# 
# p
```

## Calculation X: Number of patients including the multivariate linear regression

``` r
timeseries_analysis %>% 
  filter(
    insurance %in% c("Medicare", "Medicaid", "Private")
  ) %>% 
  mutate(
    race_ethnicity = factor(race_ethnicity, ordered = FALSE),
    race_ethnicity = relevel(race_ethnicity, ref = "White"),
    insurance = factor(insurance, ordered = FALSE), 
    insurance = relevel(insurance, ref = "Private"),
    smoke_status = factor(smoke_status),
    smoke_status = relevel(smoke_status, ref = "No / Never")
  ) %>% 
  count()
```

    ## # A tibble: 1 × 1
    ##       n
    ##   <int>
    ## 1 39908

## Figure 5: Average severity score at each baseline VA (rounded to nearest 5) by race

``` r
#calculate severity by race and age

severity_race_vision <-
  timeseries_analysis %>% 
  mutate(baseline_va_r5 = plyr::round_any(baseline_va_letter, 5)) %>% 
  group_by(race_ethnicity, baseline_va_r5) %>%
  summarize(avg_severity = mean(severity_score), count = n()) %>% 
  filter(count > 100) %>% 
  arrange(desc(avg_severity))
```

    ## `summarise()` has grouped output by 'race_ethnicity'. You can override using the `.groups` argument.

``` r
severity_race_vision
```

    ## # A tibble: 31 × 4
    ## # Groups:   race_ethnicity [3]
    ##    race_ethnicity baseline_va_r5 avg_severity count
    ##    <ord>                   <dbl>        <dbl> <int>
    ##  1 Hispanic                   40         65.9   144
    ##  2 Hispanic                   35         64.1   373
    ##  3 Hispanic                   50         64.0   359
    ##  4 Hispanic                   55         63.8   361
    ##  5 Hispanic                   60         62.9  1024
    ##  6 White                      25         62.6   147
    ##  7 Hispanic                   85         62.5   374
    ##  8 White                      45         62.3   175
    ##  9 Black                      35         62.2   317
    ## 10 Hispanic                   65         62.2   866
    ## # … with 21 more rows

``` r
# graph severity by race and age group

plot_sev_race_va <-
  severity_race_vision %>%
  filter(race_ethnicity %in% c("White", "Black", "Hispanic")) %>% 
  ggplot(aes(x = baseline_va_r5, y = avg_severity, color = race_ethnicity)) +
  geom_line() +
  geom_point() +
  theme_light() +
  theme(
    panel.grid.minor = element_blank(),
    panel.grid.major.x = element_blank()
  ) +
  scale_color_manual(values = race_colors) +
  labs(
    y = "Average severity score", 
    x = "Baseline VA", 
    color = "Race"
  )

plot_sev_race_va
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-43-1.png)<!-- -->

``` r
#ggsave("graphs/5_plot_sev_race_va.png", plot_sev_race_va, width = 7, height = 5)
```

## Figure 6: Average severity score at each baseline VA (rounded to nearest 5) by insurance

``` r
#calculate severity by race and age

severity_insurance_vision <-
  timeseries_analysis %>% 
  mutate(baseline_va_r5 = plyr::round_any(baseline_va_letter, 5)) %>% 
  group_by(insurance, baseline_va_r5) %>%
  summarize(avg_severity = mean(severity_score), count = n()) %>% 
  filter(count > 100) %>% 
  arrange(desc(avg_severity))
```

    ## `summarise()` has grouped output by 'insurance'. You can override using the `.groups` argument.

``` r
severity_insurance_vision
```

    ## # A tibble: 44 × 4
    ## # Groups:   insurance [6]
    ##    insurance       baseline_va_r5 avg_severity count
    ##    <ord>                    <dbl>        <dbl> <int>
    ##  1 Medicaid                    35         65.9   133
    ##  2 Medicaid                    50         65.8   132
    ##  3 Medicaid                    55         63.4   103
    ##  4 Medicare                    25         63.3   155
    ##  5 Govt                        60         63.0   144
    ##  6 Unknown/Missing             65         62.8   203
    ##  7 Medicaid                    65         62.6   340
    ##  8 Private                     40         62.6   137
    ##  9 Private                     35         62.6   422
    ## 10 Medicaid                    70         62.4   457
    ## # … with 34 more rows

``` r
# graph severity by race and age group
plot_sev_insurance_va <-
  severity_insurance_vision %>%
  filter(insurance %in% c("Medicaid", "Medicare", "Private")) %>% 
  ggplot(aes(x = baseline_va_r5, y = avg_severity, color = insurance)) +
  geom_line() +
  geom_point() +
  theme_light() +
  theme(
    panel.grid.minor = element_blank(),
    panel.grid.major.x = element_blank()
  ) +
  scale_color_manual(values = insurance_colors) +
  labs(
    title = "Severity by insurance and baseline VA", 
    y = "Average severity score", 
    x = "Baseline VA", 
    color = "Insurance"
  )

plot_sev_insurance_va
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-45-1.png)<!-- -->

``` r
#ggsave("graphs/6_plot_sev_insurance_va.png", plot_sev_insurance_va, width = 7, height = 5)
```

## Timeseries analysis of visual acuity

#### Distribution of VA by race

``` r
# table of percentage distribution to baseline letter va by race
race_distribution <-
  timeseries_analysis %>% 
  group_by(race_ethnicity, baseline_va_r10) %>%   
  summarize(count = n()) %>% 
  ungroup() %>% 
  group_by(race_ethnicity) %>% 
  mutate(prop = count / sum(count)) %>% 
  ungroup()
```

    ## `summarise()` has grouped output by 'race_ethnicity'. You can override using the `.groups` argument.

``` r
race_distribution %>% 
  group_by(race_ethnicity) %>% 
  summarize(total_race = sum(count))
```

    ## # A tibble: 3 × 2
    ##   race_ethnicity total_race
    ##   <ord>               <int>
    ## 1 White               30989
    ## 2 Black                5922
    ## 3 Hispanic             6363

``` r
timeseries_analysis %>% 
  ggplot(aes(x = baseline_va_letter)) +
  geom_histogram(binwidth = 10, aes(fill = race_ethnicity)) +
  theme_light() +
  scale_x_continuous(breaks = seq(0, 100, 10)) +
  labs(
    title = "Histogram of eye count by race"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-47-1.png)<!-- -->

``` r
race_distribution %>% 
  ggplot(aes(x = baseline_va_r10, y = prop, color = race_ethnicity)) + 
  geom_line() +
  geom_point() +
  theme_light()  +
  scale_y_continuous() +
  labs(
    title = "Propotion of eyes in each group of baseline visual acuity, by race",
    y = "Proportion", 
    x = "Baseline VA, rounded to nearest multiple of 10"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-48-1.png)<!-- -->

#### Timeseries VA by race and severity

``` r
## calculate change in va over time for people in each severity group/race
severity_race_va <-
  timeseries_analysis %>% 
  group_by(race_ethnicity, severity_score) %>% 
  summarize(baseline_va = mean(baseline_va_letter), one_year_va = mean(one_year), two_year_va = mean(two_year), count = n()) %>% 
  gather(key = "timepoint", value = "va_plot", baseline_va, one_year_va, two_year_va)
```

    ## `summarise()` has grouped output by 'race_ethnicity'. You can override using the `.groups` argument.

``` r
timeseries_analysis %>% 
  count(severity_score, race_ethnicity) %>% 
  group_by(race_ethnicity) %>% 
  mutate(prop = n / sum(n))
```

    ## # A tibble: 33 × 4
    ## # Groups:   race_ethnicity [3]
    ##    severity_score race_ethnicity     n   prop
    ##             <dbl> <ord>          <int>  <dbl>
    ##  1             35 White           3349 0.108 
    ##  2             35 Black            596 0.101 
    ##  3             35 Hispanic         442 0.0695
    ##  4             40 White           1887 0.0609
    ##  5             40 Black            323 0.0545
    ##  6             40 Hispanic         226 0.0355
    ##  7             43 White            840 0.0271
    ##  8             43 Black            148 0.0250
    ##  9             43 Hispanic         141 0.0222
    ## 10             44 White           4545 0.147 
    ## # … with 23 more rows

``` r
## density chart of distribution of severity scores, by race
timeseries_analysis %>% 
  ggplot() +
  geom_density(aes(x = severity_score, group = race_ethnicity, color = race_ethnicity), alpha = .2) +
  theme_light()
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-51-1.png)<!-- -->

This chart shows the density of patients by severity score. There are
two peaks - first at around severity scores between 40-48, and a second
at severity scores betewen 73-78. These numbers are generally aligned to
categorizations of moderate NPDR (with and without edema), and PDR (with
and without edema).

#### Timeseries VA by race

``` r
# plot change in visual acuity by race at one and two yeaars

va_race <-
  timeseries_analysis %>% 
  group_by(race_ethnicity) %>% 
  summarize(
    baseline_va = mean(baseline_va_letter), 
    one_year_va = mean(one_year), 
    two_year_va = mean(two_year),
    baseline_sd = sd(baseline_va_letter),
    one_year_sd = sd(one_year),
    two_year_sd = sd(two_year),
    count = n()) %>% 
  gather(key = "timepoint", value = "va_plot", baseline_va, one_year_va, two_year_va) %>% 
  ungroup()
  
va_race %>% 
  ggplot(aes(x = timepoint, y = va_plot, color = race_ethnicity)) +
  geom_point() +
  geom_line(aes(group = race_ethnicity)) +
  ggrepel::geom_text_repel(
    data = va_race %>% filter(timepoint == "two_year_va"),
    aes(label = count)
  ) +
  theme_light() + 
  labs(
    title = "VA at baseline, one, and two years by race",
    y = "Visual acuity"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-52-1.png)<!-- -->

``` r
va_race_diff <-
  va_race %>% 
  spread(key = timepoint, value = va_plot) %>% 
  mutate(
    one_year_change = one_year_va - baseline_va,
    two_year_change = two_year_va - baseline_va,
    baseline = 0
  ) %>% 
  gather(key = "time_diff", value = "va_change", baseline, one_year_change, two_year_change) %>% 
  ungroup()

va_race_diff %>% 
  ggplot(aes(x = time_diff, y = va_change, color = race_ethnicity)) +
  geom_point() +
  geom_line(aes(group = race_ethnicity)) +
  ggrepel::geom_text_repel(
    data = va_race_diff %>% filter(time_diff == "two_year_change"),
    aes(label = count)
  ) +
  theme_light() + 
  labs(
    title = "VA change from baseline, one, and two years by race",
    y = "Visual acuity (VA) change"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-53-1.png)<!-- -->

``` r
# plot change in visual acuity by race at one and two yeaars
severity_race_va %>% 
  filter(severity_score == 73) %>% 
  ggplot(aes(x = timepoint, y = va_plot, color = race_ethnicity)) +
  geom_point() +
  geom_line(aes(group = race_ethnicity)) +
  ggrepel::geom_text_repel(
    data = severity_race_va %>% filter(timepoint == "two_year_va", severity_score %in% c(73)),
    aes(label = count)
  ) +
  theme_light() + 
  labs(
    title = "VA at baseline, one, and two years by race at severity score 73",
    y = "Visual acuity"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-54-1.png)<!-- -->

Patients with starting severity score of 73, split by race, with visual
acuity measured over time. It appears that visual acuity changes seem to
follow similar trends (i.e., net change is quite similar), but White
people start with a visual acuity 1 letter higher than Black people and
two letters higher than Hispanic people.

``` r
severity_race_va %>% 
  filter(severity_score < 80) %>% 
  ggplot(aes(x = timepoint, y = va_plot, color = severity_score, linetype = race_ethnicity)) +
  geom_point() +
  geom_line(aes(group = interaction(severity_score, race_ethnicity))) +
  theme_light() + 
  ggrepel::geom_text_repel(
    data = severity_race_va %>% filter(timepoint == "two_year_va", severity_score < 80),
    aes(label = count)
  ) +
  scale_color_viridis_c() +
  labs(
    title = "VA at baseline, one, and two years by race and severity score"
  ) 
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-55-1.png)<!-- -->

``` r
# average two year visual acuity by starting severity and race

severity_race_va %>% 
  filter(timepoint == "two_year_va") %>% 
  # filtering out severity scores larger than 79, as there are less than 50 people in each category
  filter(severity_score < 80) %>% 
  ggplot(aes(x = severity_score, y = va_plot, color = race_ethnicity)) +
  geom_point() +
  geom_line(aes(group = race_ethnicity)) +
  ggrepel::geom_text_repel(
    data = severity_race_va %>% filter(timepoint == "two_year_va", severity_score %in% c(35, 78)),
    aes(label = count)
  ) +
  theme_light() + 
  labs(
    title = "VA at two years by race and severity score", 
    x = "Severity score", 
    y = "Two-year visual acuity"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-56-1.png)<!-- -->

This chart confirms our expectation that visual acuity two years after
index date typically is lower for patients with a higher starting
severity score.

#### Timeseries VA by insurance

``` r
# plot change in visual acuity by race at one and two yeaars

va_insurance <-
  timeseries_analysis %>% 
  group_by(insurance) %>%
  filter(!insurance %in% c("Unknown/Missing", "Military", "Govt")) %>% 
  summarize(
    baseline_va = mean(baseline_va_letter), 
    one_year_va = mean(one_year), 
    two_year_va = mean(two_year),
    baseline_sd = sd(baseline_va_letter),
    one_year_sd = sd(one_year),
    two_year_sd = sd(two_year),
    count = n()) %>% 
  gather(key = "timepoint", value = "va_plot", baseline_va, one_year_va, two_year_va) %>% 
  ungroup()
  
va_insurance %>% 
  ggplot(aes(x = timepoint, y = va_plot, color = insurance)) +
  geom_point() +
  geom_line(aes(group = insurance)) +
  ggrepel::geom_text_repel(
    data = va_insurance %>% filter(timepoint == "two_year_va"),
    aes(label = count)
  ) +
  theme_light() + 
  labs(
    title = "VA at baseline, one, and two years by insurance",
    y = "Visual acuity"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-57-1.png)<!-- -->

``` r
va_insurance_diff <-
  va_insurance %>% 
  spread(key = timepoint, value = va_plot) %>% 
  mutate(
    one_year_change = one_year_va - baseline_va,
    two_year_change = two_year_va - baseline_va,
    baseline = 0
  ) %>% 
  gather(key = "time_diff", value = "va_change", baseline, one_year_change, two_year_change) %>% 
  ungroup()

va_insurance_diff %>% 
  ggplot(aes(x = time_diff, y = va_change, color = insurance)) +
  geom_point() +
  geom_line(aes(group = insurance)) +
  ggrepel::geom_text_repel(
    data = va_insurance_diff %>% filter(time_diff == "two_year_change"),
    aes(label = count)
  ) +
  theme_light() + 
  labs(
    title = "VA change from baseline, one, and two years by insurance",
    y = "Visual acuity (VA) change"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-58-1.png)<!-- -->

#### Percentage with 15 letter loss by race

``` r
change_15 <- 
  timeseries_analysis %>% 
  mutate(
    one_year_change = one_year - baseline_va_letter,
    two_year_change = two_year - baseline_va_letter,
    baseline_change = 0
  ) %>% 
  mutate(
    one_year_15_pt_loss = if_else(one_year_change <= -15, 1, 0),
    two_year_15_pt_loss = if_else(two_year_change <= -15, 1, 0),
    one_year_15_pt_gain = if_else(one_year_change >= 15, 1, 0),
    two_year_15_pt_gain = if_else(two_year_change >= 15, 1, 0),
  ) %>% 
  #gather(key = "time_diff", value = "va_change", baseline_change, one_year_change, two_year_change) %>% 
  ungroup()


# z value for p = .05 
z = 1.96


loss_15_race <-
  change_15 %>% 
  group_by(race_ethnicity) %>% 
  summarize(
    one_year_loss = sum(one_year_15_pt_loss) / n(),
    two_year_loss = sum(two_year_15_pt_loss) / n(),
    one_year_sd = sqrt(one_year_loss * (1 - one_year_loss) / n()),
    two_year_sd = sqrt(two_year_loss * (1 - two_year_loss) / n())
  ) %>% 
  gather(key = "time_diff", value = "pct_15pt_loss", one_year_loss, two_year_loss) %>% 
  mutate(
    sd = if_else(str_detect(string = time_diff, pattern = "one_year"), one_year_sd, two_year_sd),
    upper_bound = pct_15pt_loss + z * sd,
    lower_bound = pct_15pt_loss - z * sd
  )
  
loss_15_race %>% 
  ggplot(aes(x = time_diff, y = pct_15pt_loss, fill = race_ethnicity)) +
  geom_col(position = "dodge") +
  geom_errorbar(aes(ymin = lower_bound, ymax = upper_bound), position = position_dodge(width = .9), width = .6) +
  theme_light() +
  labs(
    title = "Percentage of eyes that lost 15 or more pts of VA, by race"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-59-1.png)<!-- -->

``` r
gain_15_race <-
  change_15 %>% 
  group_by(race_ethnicity) %>% 
  summarize(
    one_year_gain = sum(one_year_15_pt_gain) / n(),
    two_year_gain = sum(two_year_15_pt_gain) / n(),
    one_year_sd = sqrt(one_year_gain * (1 - one_year_gain) / n()),
    two_year_sd = sqrt(two_year_gain * (1 - two_year_gain) / n())
  ) %>% 
  gather(key = "time_diff", value = "pct_15pt_gain", one_year_gain, two_year_gain) %>% 
  mutate(
    sd = if_else(str_detect(string = time_diff, pattern = "one_year"), one_year_sd, two_year_sd),
    upper_bound = pct_15pt_gain + z * sd,
    lower_bound = pct_15pt_gain - z * sd
  )

gain_15_race %>% 
  ggplot(aes(x = time_diff, y = pct_15pt_gain, fill = race_ethnicity)) +
  geom_col(position = "dodge") +
  geom_errorbar(aes(ymin = lower_bound, ymax = upper_bound), position = position_dodge(width = .9), width = .6) +
  theme_light() +
  labs(
    title = "Percentage of eyes that gained 15 or more pts of VA, by race"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-59-2.png)<!-- -->

#### Percentage with 15 letter loss by insurance

``` r
# z value for p = .05 
z = 1.96


loss_15_insurance <-
  change_15 %>% 
  group_by(insurance) %>%
  filter(!insurance %in% c("Unknown/Missing", "Military", "Govt")) %>% 
  summarize(
    one_year_loss = sum(one_year_15_pt_loss) / n(),
    two_year_loss = sum(two_year_15_pt_loss) / n(),
    one_year_sd = sqrt(one_year_loss * (1 - one_year_loss) / n()),
    two_year_sd = sqrt(two_year_loss * (1 - two_year_loss) / n())
  ) %>% 
  gather(key = "time_diff", value = "pct_15pt_loss", one_year_loss, two_year_loss) %>% 
  mutate(
    sd = if_else(str_detect(string = time_diff, pattern = "one_year"), one_year_sd, two_year_sd),
    upper_bound = pct_15pt_loss + z * sd,
    lower_bound = pct_15pt_loss - z * sd
  )
  
loss_15_insurance %>% 
  ggplot(aes(x = time_diff, y = pct_15pt_loss, fill = insurance)) +
  geom_col(position = "dodge") +
  geom_errorbar(aes(ymin = lower_bound, ymax = upper_bound), position = position_dodge(width = .9), width = .6) +
  theme_light() +
  labs(
    title = "Percentage of eyes that lost 15 or more pts of VA, by insurance"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-60-1.png)<!-- -->

``` r
gain_15_insurance <-
  change_15 %>% 
  group_by(insurance) %>% 
  filter(!insurance %in% c("Unknown/Missing", "Military", "Govt")) %>% 
  summarize(
    one_year_gain = sum(one_year_15_pt_gain) / n(),
    two_year_gain = sum(two_year_15_pt_gain) / n(),
    one_year_sd = sqrt(one_year_gain * (1 - one_year_gain) / n()),
    two_year_sd = sqrt(two_year_gain * (1 - two_year_gain) / n())
  ) %>% 
  gather(key = "time_diff", value = "pct_15pt_gain", one_year_gain, two_year_gain) %>% 
  mutate(
    sd = if_else(str_detect(string = time_diff, pattern = "one_year"), one_year_sd, two_year_sd),
    upper_bound = pct_15pt_gain + z * sd,
    lower_bound = pct_15pt_gain - z * sd
  )

gain_15_insurance %>% 
  ggplot(aes(x = time_diff, y = pct_15pt_gain, fill = insurance)) +
  geom_col(position = "dodge") +
  geom_errorbar(aes(ymin = lower_bound, ymax = upper_bound), position = position_dodge(width = .9), width = .6) +
  theme_light() +
  labs(
    title = "Percentage of eyes that gained 15 or more pts of VA, by insurance"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-60-2.png)<!-- -->

#### Timeseries VA by insurance and race

``` r
## NOTE - be sure to combine the two medicare datasets rather than eliminating one

# insurance_race_va %>% 
#   filter(!insurance %in% c("Unknown/Missing", "Military", "Govt")) %>% 
#   ggplot(aes(x = timepoint, y = va_plot, color = race_ethnicity, linetype = insurance)) +
#   geom_point() +
#   geom_line(aes(group = interaction(insurance, race_ethnicity))) +
#   ggrepel::geom_text_repel(
#     data = insurance_race_va %>% filter(timepoint == "two_year_va", insurance %in% c("Private", "Medicaid", "Medicare")), 
#     aes(label = count, hjust = -1)
#   ) +
#   theme_light() + 
#   labs(
#     title = "VA at baseline, one, and two years by race and insurance"
#   ) 
```

There are disparities in baseline VA that persist over time for Black
and Hispanic patients as compared to White patients. The type of
insurance appears to have strong correlation to the baseline VA, but
does not fully explain the difference in race.

``` r
# 
# va_insurance_race_diff_tbl <-
#   insurance_race_va %>% 
#   filter(!insurance %in% c("Unknown/Missing", "Military", "Govt")) %>% 
#   spread(key = timepoint, value = va_plot) %>% 
#   mutate(
#     one_year_change = one_year_va - baseline_va,
#     two_year_change = two_year_va - baseline_va,
#     baseline = 0
#   )
# 
# va_insurance_race_diff <-
#   insurance_race_va %>% 
#   filter(!insurance %in% c("Unknown/Missing", "Military", "Govt")) %>% 
#   spread(key = timepoint, value = va_plot) %>% 
#   mutate(
#     one_year_change = one_year_va - baseline_va,
#     two_year_change = two_year_va - baseline_va,
#     baseline = 0
#   ) %>% 
#   gather(key = "time_diff", value = "va_change", baseline, one_year_change, two_year_change) %>% 
#   ungroup()
# 
# va_insurance_race_diff_ci <-
#   timeseries_analysis %>% 
#   mutate(
#     one_year_change = one_year - baseline_va_letter,
#     two_year_change = two_year - baseline_va_letter,
#     baseline = 0
#   ) %>% 
#   group_by(race_ethnicity, insurance) %>% 
#   summarize(
#     mean_one_year_change = mean(one_year_change),
#     mean_two_year_change = mean(two_year_change),
#     sd_one_year_change = sd(one_year_change),
#     sd_two_year_change = sd(two_year_change)
#   )
# 
# va_insurance_race_diff %>% 
#   ggplot(aes(x = time_diff, y = va_change, color = race_ethnicity, linetype = insurance)) +
#   geom_point() +
#   geom_line(aes(group = interaction(insurance, race_ethnicity))) +
#   ggrepel::geom_text_repel(
#     data = va_insurance_race_diff %>% filter(time_diff == "two_year_change", insurance %in% c("Private", "Medicaid", "Medicare")), 
#     aes(label = count, hjust = -2)
#   ) +
#   facet_grid(insurance~.) +
#   theme_light() + 
#   labs(
#     title = "VA change from baseline, one, and two years by insurance and race",
#     y = "Visual acuity (VA) change"
#   )
```

#### Timeseries VA by smoking and race

``` r
smoking_race_va <-
  timeseries_analysis %>% 
  group_by(race_ethnicity, smoke_status) %>% 
  summarize(baseline_va = mean(baseline_va_letter), one_year_va = mean(one_year), two_year_va = mean(two_year), count = n()) %>% 
  gather(key = "timepoint", value = "va_plot", baseline_va, one_year_va, two_year_va)
```

    ## `summarise()` has grouped output by 'race_ethnicity'. You can override using the `.groups` argument.

``` r
## NOTE - be sure to combine the two medicare datasets rather than eliminating one

smoking_race_va %>% 
  #filter(!smoke_status %in% c("Unknown / Unclassified")) %>% 
  ggplot(aes(x = timepoint, y = va_plot, color = race_ethnicity, linetype = smoke_status)) +
  geom_point() +
  geom_line(aes(group = interaction(smoke_status, race_ethnicity))) +
  ggrepel::geom_text_repel(
    data = smoking_race_va %>% filter(timepoint == "two_year_va"), #!smoke_status %in% c("Unknown / Unclassified") 
    aes(label = count, hjust = -1)
  ) +
  theme_light() + 
  labs(
    title = "VA at baseline, one, and two years by race and smoking status"
  ) 
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-64-1.png)<!-- -->

#### Timeseries VA by gender and race

``` r
gender_race_va <-
  timeseries_analysis %>% 
  group_by(race_ethnicity, gender) %>% 
  summarize(baseline_va = mean(baseline_va_letter), one_year_va = mean(one_year), two_year_va = mean(two_year), count = n()) %>% 
  gather(key = "timepoint", value = "va_plot", baseline_va, one_year_va, two_year_va)
```

    ## `summarise()` has grouped output by 'race_ethnicity'. You can override using the `.groups` argument.

``` r
gender_race_va %>% 
  filter(gender %in% c("Male", "Female")) %>% 
  ggplot(aes(x = timepoint, y = va_plot, color = race_ethnicity, linetype = gender)) +
  geom_point() +
  geom_line(aes(group = interaction(gender, race_ethnicity))) +
  ggrepel::geom_text_repel(
    data = gender_race_va %>% filter(timepoint == "two_year_va", gender %in% c("Male", "Female")), 
    aes(label = count, hjust = -1)
  ) +
  theme_light() + 
  labs(
    title = "VA at baseline, one, and two years by race and gender"
  ) 
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-66-1.png)<!-- -->

Female patients appear to have lower baseline VA as compared to their
male counterparts of the same race. The raw gain or loss in VA over time
appears to be similar between genders within racial groups.

#### Race VA timeseries

``` r
va_race_tbl <-
  timeseries_analysis %>% 
  group_by(race_ethnicity) %>% 
  summarize(
    mean_baseline_va = mean(baseline_va_letter),
    mean_one_year = mean(one_year),
    mean_two_year = mean(two_year),
    count = n()
  )
```

``` r
race_va <-
  timeseries_analysis %>% 
  # round baseline data to nearest multiple of 5
  mutate(
    baseline_va_r = plyr::round_any(baseline_va_letter, 5)
  ) %>% 
  # group patients by race and same baseline va data
  group_by(baseline_va_r, race_ethnicity) %>% 
  summarize(baseline_va = mean(baseline_va_letter), one_year_va = mean(one_year), two_year_va = mean(two_year), count = n()) %>% 
  gather(key = "timepoint", value = "va_plot", baseline_va, one_year_va, two_year_va) %>% 
  ungroup()
```

    ## `summarise()` has grouped output by 'baseline_va_r'. You can override using the `.groups` argument.

``` r
race_va %>%
  filter(count > 25) %>% 
  ggplot(aes(x = timepoint, y = va_plot, color = baseline_va_r, linetype = race_ethnicity)) +
  geom_point() +
  geom_line(aes(group = interaction(baseline_va_r, race_ethnicity))) +
  theme_light() + 
  scale_color_viridis_c() +
  labs(
    title = "VA at baseline, one, and two years by race for people starting at the same baseline visual acuity"
  ) 
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-69-1.png)<!-- -->

This graph is hard to interpret, but is showing the differential
progression of va for people of different races that start at the same
level of baseline visual acuity. The next graphs dive into this daata
more closely.

This graph takes on an interesting shape. It indicates that people that
start with higher visual acuity are likely to experience a decline in
visual acuity, aand those starting at the lowest end of the spectrum of
visuala acuity are likely to make gains.

``` r
race_va %>%
  filter(count > 25) %>% 
  filter(baseline_va_r %in% c(65, 70, 75)) %>% 
  mutate(baseline_va_r = as.factor(baseline_va_r)) %>% 
  ggplot(aes(x = timepoint, y = va_plot, linetype = baseline_va_r, color = race_ethnicity)) +
  geom_point() +
  geom_line(aes(group = interaction(baseline_va_r, race_ethnicity))) +
  theme_light() + 
  ggrepel::geom_text_repel(
    data = race_va %>% filter(timepoint %in% c("two_year_va"), baseline_va_r %in% c(65, 70, 75)) %>% mutate(baseline_va_r = as.factor(baseline_va_r)), 
    aes(label = count, hjust = -1)
  ) +
  scale_y_continuous(breaks = seq(64, 78, 1)) +
  labs(
    title = "VA at baseline, one, and two years by race for people starting at ~65, ~70, ~75 baseline va",
    y = "visual acuity (letter)"
  ) 
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-70-1.png)<!-- -->

We observe that Hispanic patients experience starting with \~65 visual
acuity, they experiences much less gain in vision over two years as
compared to their White and Black counterprats. White people appear to
gain two letters, while Hispanic people gain less than hallf a letter.

Exemplifying the trends from above, we also se that those who start with
VA at \~75 all experience substantial decline, with those at other
starting points experiencing less severe declines or even making gains.

``` r
race_va %>%
  filter(count > 25) %>% 
  filter(baseline_va_r %in% c(50, 55, 60)) %>% 
  mutate(baseline_va_r = as.factor(baseline_va_r)) %>% 
  ggplot(aes(x = timepoint, y = va_plot, linetype = baseline_va_r, color = race_ethnicity)) +
  geom_point() +
  geom_line(aes(group = interaction(baseline_va_r, race_ethnicity))) +
  theme_light() + 
  ggrepel::geom_text_repel(
    data = race_va %>% filter(timepoint %in% c("two_year_va"), baseline_va_r %in% c(50, 55, 60)) %>% mutate(baseline_va_r = as.factor(baseline_va_r)), 
    aes(label = count, hjust = -1)
  ) +
  scale_y_continuous(breaks = seq(40, 65, 1)) +
  labs(
    title = "VA at baseline, one, and two years by race for people starting at ~50, ~55, ~60 baseline va",
    y = "visual acuity (letter)"
  ) 
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-71-1.png)<!-- -->

At this level, the dispairities in improvements are much more stark.
Whie people with baseline va of 50 are 3 additional points of VA gain as
compared to their Black and Hispanic counterparts. Similar, though not
as stark disparities are present for people with starting VA at 55 and
60.

Further analysis is required to control for the specific condition
associated that the patient is diagnosed with.

``` r
race_va %>%
  filter(count > 25) %>% 
  filter(baseline_va_r %in% c(35, 40, 45)) %>% 
  mutate(baseline_va_r = as.factor(baseline_va_r)) %>% 
  ggplot(aes(x = timepoint, y = va_plot, linetype = baseline_va_r, color = race_ethnicity)) +
  geom_point() +
  geom_line(aes(group = interaction(baseline_va_r, race_ethnicity))) +
  theme_light() + 
  ggrepel::geom_text_repel(
    data = race_va %>% filter(timepoint %in% c("two_year_va"), baseline_va_r %in% c(35, 40, 45)) %>% mutate(baseline_va_r = as.factor(baseline_va_r)), 
    aes(label = count, hjust = -1)
  ) +
  scale_y_continuous(breaks = seq(30, 60, 1)) +
  labs(
    title = "VA at baseline, one, and two years by race for people starting at ~35, ~40, ~45 baseline va",
    y = "visual acuity (letter)"
  ) 
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-72-1.png)<!-- -->

#### Race VA timeseries bucketed by 10

``` r
race_va_10 <-
  timeseries_analysis %>% 
  # group patients by race and same baseline va data
  group_by(baseline_va_r10, race_ethnicity) %>% 
  summarize(baseline_va = mean(baseline_va_letter), one_year_va = mean(one_year), two_year_va = mean(two_year), count = n()) %>% 
  gather(key = "timepoint", value = "va_plot", baseline_va, one_year_va, two_year_va) %>% 
  ungroup()
```

    ## `summarise()` has grouped output by 'baseline_va_r10'. You can override using the `.groups` argument.

``` r
## Same as above - split by ten 
race_va_10 %>%
  filter(count > 25) %>% 
  filter(baseline_va_r10 %in% c(50, 60, 70)) %>% 
  mutate(baseline_va_r10 = as.factor(baseline_va_r10)) %>% 
  ggplot(aes(x = timepoint, y = va_plot, linetype = baseline_va_r10, color = race_ethnicity)) +
  geom_point() +
  geom_line(aes(group = interaction(baseline_va_r10, race_ethnicity))) +
  theme_light() + 
  ggrepel::geom_text_repel(
    data = race_va_10 %>% filter(timepoint %in% c("two_year_va"), baseline_va_r10 %in% c(50, 60, 70)) %>% mutate(baseline_va_r10 = as.factor(baseline_va_r10)), 
    aes(label = count, hjust = -1)
  ) +
  labs(
    title = "VA at baseline, one, and two years by race for people starting at ~50, ~60, ~70 baseline va",
    y = "visual acuity (letter)"
  ) 
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-73-1.png)<!-- -->

``` r
severity_race_va_10 <-
  timeseries_analysis %>% 
  # group patients by race, severity, and same baseline va data
  group_by(baseline_va_r10, severity_score, race_ethnicity) %>% 
  summarize(baseline_va = mean(baseline_va_letter), one_year_va = mean(one_year), two_year_va = mean(two_year), count = n()) %>% 
  gather(key = "timepoint", value = "va_plot", baseline_va, one_year_va, two_year_va) %>% 
  ungroup()
```

    ## `summarise()` has grouped output by 'baseline_va_r10', 'severity_score'. You can override using the `.groups` argument.

``` r
severity_race_va_10_sd <-
  timeseries_analysis %>% 
  # group patients by race, severity, and same baseline va data
  group_by(baseline_va_r10, severity_score, race_ethnicity) %>% 
  summarize(baseline_va = sd(baseline_va_letter), one_year_va = sd(one_year), two_year_va = sd(two_year), count = n()) %>% 
  gather(key = "timepoint", value = "va_plot_sd", baseline_va, one_year_va, two_year_va) %>% 
  ungroup()
```

    ## `summarise()` has grouped output by 'baseline_va_r10', 'severity_score'. You can override using the `.groups` argument.

``` r
severity_race_va_10 %>% 
  arrange(desc(count))
```

    ## # A tibble: 669 × 6
    ##    baseline_va_r10 severity_score race_ethnicity count timepoint   va_plot
    ##              <dbl>          <dbl> <ord>          <int> <chr>         <dbl>
    ##  1              80             73 White           2863 baseline_va    79.3
    ##  2              80             73 White           2863 one_year_va    76.2
    ##  3              80             73 White           2863 two_year_va    74.8
    ##  4              60             73 White           2648 baseline_va    61.1
    ##  5              60             73 White           2648 one_year_va    64.0
    ##  6              60             73 White           2648 two_year_va    64.1
    ##  7              80             44 White           1729 baseline_va    78.9
    ##  8              80             44 White           1729 one_year_va    76.9
    ##  9              80             44 White           1729 two_year_va    75.4
    ## 10              70             73 White           1532 baseline_va    70.0
    ## # … with 659 more rows

``` r
severity_race_va_10 %>%
  filter(
    baseline_va_r10 %in% c(80),
    severity_score %in% c(73)
  ) %>% 
  left_join(severity_race_va_10_sd %>% select(-count), by = c("baseline_va_r10", "severity_score", "race_ethnicity", "timepoint")) %>% 
  mutate(severity_score = as.factor(severity_score)) %>% 
  ggplot(aes(x = timepoint, y = va_plot, linetype = severity_score, color = race_ethnicity)) +
  geom_point() +
  geom_line(aes(group = interaction(severity_score, race_ethnicity))) +
  theme_light() + 
  ggrepel::geom_text_repel(
    data = severity_race_va_10 %>% filter(timepoint %in% c("two_year_va"), baseline_va_r10 %in% c(80), severity_score %in% c(73)) %>% mutate(severity_score = as.factor(severity_score)), 
    aes(label = count, hjust = -1)
  ) +
  geom_errorbar(aes(ymin = va_plot - va_plot_sd, ymax = va_plot + va_plot_sd)) +
  labs(
    title = "VA at baseline, one, and two years by race for people starting at ~80 baseline VA",
    subtitle = "Grouped by starting severity score and race",
    y = "visual acuity (letter)"
  ) 
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-75-1.png)<!-- -->

#### Severity race VA timeseries

``` r
severity_race_va <-
  timeseries_analysis %>% 
  # round baseline data to nearest multiple of 5
  mutate(
    baseline_va_r = plyr::round_any(baseline_va_letter, 5)
  ) %>% 
  # group patients by race, severity, and same baseline va data
  group_by(baseline_va_r, severity_score, race_ethnicity) %>% 
  summarize(baseline_va = mean(baseline_va_letter), one_year_va = mean(one_year), two_year_va = mean(two_year), count = n()) %>% 
  gather(key = "timepoint", value = "va_plot", baseline_va, one_year_va, two_year_va) %>% 
  ungroup()
```

    ## `summarise()` has grouped output by 'baseline_va_r', 'severity_score'. You can override using the `.groups` argument.

``` r
severity_race_va %>%
  filter(
    baseline_va_r %in% c(50),
    severity_score %in% c(73, 78)
  ) %>% 
  mutate(severity_score = as.factor(severity_score)) %>% 
  ggplot(aes(x = timepoint, y = va_plot, linetype = severity_score, color = race_ethnicity)) +
  geom_point() +
  geom_line(aes(group = interaction(severity_score, race_ethnicity))) +
  theme_light() + 
  ggrepel::geom_text_repel(
    data = severity_race_va %>% filter(timepoint %in% c("two_year_va"), baseline_va_r %in% c(50), severity_score %in% c(73, 78)) %>% mutate(severity_score = as.factor(severity_score)), 
    aes(label = count, hjust = -1)
  ) +
  scale_y_continuous(breaks = seq(40, 65, 1)) +
  labs(
    title = "VA at baseline, one, and two years by race for people starting at ~50 baseline VA",
    subtitle = "Grouped by starting severity score and race",
    y = "visual acuity (letter)"
  ) 
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-77-1.png)<!-- -->

For patients that start with a baseline visual acuity close to 50, the
change in visual acuity over one to two years is associated with race.
White patients with starting severity at 73 experience \~6pt gain in
visual acuity, as compared to \~5pt gain for Black and Hispanic
counterparts. After two years, white patients maintain that gain in
visual acuity; however, Black and Hispanic patients experiences a
moderate drop in visual acuity by .5pts (Hispanics) and 1.5pts (Black).

For those patients with starting severity at 78, gains in visual acuity
the first year are again similar across races, \~3 to 3.5 points.
However, in year two, white patients experience another substantial gain
in visual acuity (\~3pts), while Hispanic paitents experience no gain in
visual acuity and Black patients experience a small decline in visual
acuity (\~.5 pts).

This analysis suggests that there may be factors between the first and
second year of treatment that are creating disparate treatment outcomes
for Black and Hispanic patients as compared to white patients.

``` r
severity_race_va %>%
  filter(
    baseline_va_r %in% c(35),
    severity_score %in% c(73, 78)
  ) %>% 
  mutate(severity_score = as.factor(severity_score)) %>% 
  ggplot(aes(x = timepoint, y = va_plot, linetype = severity_score, color = race_ethnicity)) +
  geom_point() +
  geom_line(aes(group = interaction(severity_score, race_ethnicity))) +
  theme_light() + 
  theme(panel.grid.minor = element_blank()) +
  ggrepel::geom_text_repel(
    data = severity_race_va %>% filter(timepoint %in% c("two_year_va"), baseline_va_r %in% c(35), severity_score %in% c(73, 78)) %>% mutate(severity_score = as.factor(severity_score)), 
    aes(label = count, hjust = -1)
  ) +
  scale_y_continuous(breaks = seq(33, 55, 1)) +
  labs(
    title = "VA at baseline, one, and two years by race for people starting at ~35 baseline VA",
    subtitle = "Grouped by starting severity score and race",
    y = "visual acuity (letter)"
  ) 
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-78-1.png)<!-- -->

The trend identified for patients with a baseline VA of 50 (the prior
chart) partially holds for those patients with starting VA of 35. We see
that, for patients with starting severity of 73, gains in visual acuity
after one year are consistent by race. However, aafter two yers, Black
patients lost abaout 3pts of visual acutiy, while white patients gained
\~1pt in visual acuity and Hispanic patients did not experience any
change.

This trend does not hold for patients with starting severity of 78. We
see that in year one, white patients make substantially higher gains
(16pts) in VA as compared to Black patients (14 pts) and Hipanic
patients (11 pts). After two years, however, white patients experience a
drop in visual acuity (1 pts) while Black patients experience another
gain of three points.

``` r
severity_race_va %>%
  filter(
    baseline_va_r %in% c(70),
    severity_score %in% c(73, 78)
  ) %>% 
  mutate(severity_score = as.factor(severity_score)) %>% 
  ggplot(aes(x = timepoint, y = va_plot, linetype = severity_score, color = race_ethnicity)) +
  geom_point() +
  geom_line(aes(group = interaction(severity_score, race_ethnicity))) +
  theme_light() + 
  theme(panel.grid.minor = element_blank()) +
  ggrepel::geom_text_repel(
    data = severity_race_va %>% filter(timepoint %in% c("two_year_va"), baseline_va_r %in% c(70), severity_score %in% c(73, 78)) %>% mutate(severity_score = as.factor(severity_score)), 
    aes(label = count, hjust = -1)
  ) +
  labs(
    title = "VA at baseline, one, and two years by race for people starting at ~70 baseline VA",
    subtitle = "Grouped by starting severity score and race",
    y = "visual acuity (letter)"
  ) 
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-79-1.png)<!-- -->

#### Severity insurance race VA timeseries

``` r
severity_ins_race_va <-
  timeseries_analysis %>% 
  # round baseline data to nearest multiple of 5
  mutate(
    baseline_va_r = plyr::round_any(baseline_va_letter, 5)
  ) %>% 
  # group patients by race, severity, and same baseline va data
  group_by(baseline_va_r, severity_score, insurance, race_ethnicity) %>% 
  summarize(baseline_va = mean(baseline_va_letter), one_year_va = mean(one_year), two_year_va = mean(two_year), count = n()) %>% 
  gather(key = "timepoint", value = "va_plot", baseline_va, one_year_va, two_year_va) %>% 
  ungroup()
```

    ## `summarise()` has grouped output by 'baseline_va_r', 'severity_score', 'insurance'. You can override using the `.groups` argument.

``` r
severity_ins_race_va %>%
  filter(
    baseline_va_r %in% c(50),
    severity_score %in% c(78),
    insurance %in% c("Medicaid", "Medicare", "Private")
  ) %>% 
  mutate(severity_score = as.factor(severity_score)) %>% 
  ggplot(aes(x = timepoint, y = va_plot, linetype = insurance, color = race_ethnicity)) +
  geom_point() +
  geom_line(aes(group = interaction(insurance, race_ethnicity))) +
  theme_light() + 
  ggrepel::geom_text_repel(
    data = severity_ins_race_va %>% filter(timepoint %in% c("two_year_va"), baseline_va_r %in% c(50), severity_score %in% c(78), insurance %in% c("Medicaid", "Medicare", "Private")) %>% mutate(severity_score = as.factor(severity_score)), 
    aes(label = count, hjust = -1)
  ) +
  labs(
    title = "VA at baseline, one, and two years by race for people starting at ~50 baseline VA",
    subtitle = "Grouped by insuraance and race; starting severity = 78 ",
    y = "visual acuity (letter)"
  ) 
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-81-1.png)<!-- -->

## Injections by VA analysis

``` r
## calculate change in va over time for people in each severity group/race
inj_race_va <-
  timeseries_analysis %>% 
  group_by(race_ethnicity, inj_year_1) %>% 
  summarize(baseline_va = mean(baseline_va_letter), one_year_va = mean(one_year), two_year_va = mean(two_year), count = n()) %>% 
  gather(key = "timepoint", value = "va_plot", baseline_va, one_year_va, two_year_va) %>%
  filter(inj_year_1 < 13) %>% 
  mutate(inj_year_1 = as.factor(inj_year_1)) %>% 
  ungroup()


str(inj_race_va)
```

``` r
inj_race_va %>%
  filter(inj_year_1 %in% c(1, 8)) %>% 
  ggplot(aes(x = timepoint, y = va_plot, color = race_ethnicity, linetype = inj_year_1)) +
  geom_point() +
  geom_line(aes(group = interaction(inj_year_1, race_ethnicity))) +
  ggrepel::geom_text_repel(
    data = inj_race_va %>% filter(timepoint == "two_year_va", inj_year_1 %in% c(1, 8)),
    aes(label = count, hjust = -1)
  ) +
  theme_light() + 
  labs(
    title = "VA at baseline, one, and two years by race and number of injections"
  ) 
```

``` r
severity_race_inj_va_quart <-
  timeseries_analysis %>% 
  # group patients by race, severity, and same baseline va data
  group_by(baseline_va_quart, severity_score, race_ethnicity, inj_year_1) %>% 
  summarize(baseline_va = mean(baseline_va_letter), one_year_va = mean(one_year), two_year_va = mean(two_year), count = n()) %>% 
  gather(key = "timepoint", value = "va_plot", baseline_va, one_year_va, two_year_va) %>% 
  ungroup()


severity_race_inj_va_quart_sd <-
  timeseries_analysis %>% 
  # group patients by race, severity, and same baseline va data
  group_by(baseline_va_quart, severity_score, race_ethnicity, inj_year_1) %>% 
  summarize(baseline_va = sd(baseline_va_letter), one_year_va = sd(one_year), two_year_va = sd(two_year), count = n()) %>% 
  gather(key = "timepoint", value = "va_plot_sd", baseline_va, one_year_va, two_year_va) %>% 
  ungroup()


severity_race_inj_va_quart %>% 
  arrange(desc(count))
```

``` r
severity_race_inj_va_quart %>%
  filter(
    baseline_va_quart %in% c(4),
    severity_score %in% c(73),
    inj_year_1 %in% c(1)
  ) %>% 
  mutate(severity_score = as.factor(severity_score)) %>% 
  ggplot(aes(x = timepoint, y = va_plot, linetype = severity_score, color = race_ethnicity)) +
  geom_point() +
  geom_line(aes(group = interaction(severity_score, race_ethnicity))) +
  theme_light() + 
  ggrepel::geom_text_repel(
    data = 
      severity_race_inj_va_quart %>% 
      filter(
        timepoint %in% c("two_year_va"), 
        baseline_va_quart %in% c(4), 
        severity_score %in% c(73), 
        inj_year_1 %in% c(1)) %>% 
      mutate(severity_score = as.factor(severity_score)), 
    aes(label = count, hjust = -1)
  ) +
  labs(
    title = "VA at baseline, one, and two years by race for people at ~4th VA quart, 73 severity",
    subtitle = "Grouped by starting severity score, race, and number of injections = 1",
    y = "visual acuity (letter)"
  ) 
```

Hispanic patients with the same starting severity and baseline VA
quartile that receive the same number of injections in the first year (1
injection) do substantially worse than their white counterparts

## Baseline analysis

#### Severity by race

``` r
# Calculate severity by race
severity_race <-
  timeseries_analysis %>% 
  group_by(race_ethnicity) %>% 
  summarize(average = mean(severity_score), count = n()) %>% 
  arrange(desc(average))

# Output severity by race to a cleaner table
severity_race %>% kable()
```

| race_ethnicity |  average | count |
|:---------------|---------:|------:|
| Hispanic       | 62.39353 |  6363 |
| Black          | 58.56974 |  5922 |
| White          | 57.03453 | 30989 |

Hispanic people have the highest severity of all racial groups, nearaly
5 points higher than White people. Black, Asian, and Caucasian people
have similar severity at timme of diagnosis.

#### Severity by race and region

``` r
# Calculate severity by race and region
severity_race_reg <-
  timeseries_analysis %>% 
  group_by(race_ethnicity, region) %>% 
  summarize(average = mean(severity_score), count = n()) %>% 
  arrange(desc(average))
```

    ## `summarise()` has grouped output by 'race_ethnicity'. You can override using the `.groups` argument.

``` r
# Print severity by race and region table
severity_race_reg
```

    ## # A tibble: 15 × 4
    ## # Groups:   race_ethnicity [3]
    ##    race_ethnicity region    average count
    ##    <ord>          <chr>       <dbl> <int>
    ##  1 Hispanic       <NA>         68.3    98
    ##  2 Hispanic       Midwest      63.7   491
    ##  3 Hispanic       West         63.1  2269
    ##  4 Hispanic       South        62.3  2633
    ##  5 Hispanic       Northeast    59.6   872
    ##  6 Black          South        58.8  3409
    ##  7 Black          Northeast    58.4  1125
    ##  8 Black          Midwest      58.1   981
    ##  9 Black          West         58.0   339
    ## 10 White          South        57.8 10792
    ## 11 White          West         57.3  5058
    ## 12 Black          <NA>         56.8    68
    ## 13 White          Midwest      56.5  8652
    ## 14 White          <NA>         56.4   334
    ## 15 White          Northeast    56.3  6153

``` r
# graph severity by race and region

severity_race_reg %>% 
  filter(!race_ethnicity == "Unknown") %>% 
  ggplot(aes(x = region, y = average)) + 
  geom_col(aes(fill  = race_ethnicity), position = "dodge") +
  scale_fill_viridis_d() + 
  theme_light() +
  labs(
    title = "Severity by race and region"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-88-1.png)<!-- -->

While there is some regional variation, Hispanic people are most likely
among all racial groups to have higher severity at time of diagnosis as
compared to other racial groups.

#### Severity by race and age

``` r
#calculate severity by race and age

severity_race_age <-
  timeseries_analysis %>% 
  group_by(race_ethnicity, age_group) %>% 
  summarize(avg_severity = mean(severity_score), count = n()) %>% 
  arrange(desc(avg_severity))
```

    ## `summarise()` has grouped output by 'race_ethnicity'. You can override using the `.groups` argument.

``` r
severity_race_age %>% head(10) %>% kable()
```

| race_ethnicity | age_group | avg_severity | count |
|:---------------|----------:|-------------:|------:|
| Black          |        25 |     72.25926 |    27 |
| Hispanic       |        35 |     68.89744 |   156 |
| White          |        35 |     68.50963 |   571 |
| White          |        20 |     68.33333 |    33 |
| Hispanic       |        45 |     68.32414 |   435 |
| Hispanic       |        25 |     68.27273 |    33 |
| Hispanic       |        40 |     68.19355 |   279 |
| Black          |        20 |     68.00000 |     3 |
| Hispanic       |        20 |     68.00000 |     7 |
| White          |        30 |     67.95183 |   436 |

``` r
# graph severity by race and age group

severity_race_age %>%
  filter(race_ethnicity %in% c("Caucasian", "Black or African American", "Hispanic", "Asian")) %>% 
  ggplot(aes(age_group, avg_severity)) +
  geom_line(aes(color = race_ethnicity)) +
  geom_point(aes(color = race_ethnicity)) +
  theme_light() +
  labs(
    title = "Severity by race and age group"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-90-1.png)<!-- -->

\[All data\] Analysis

\[New classification\] We see that at all age levels between 45 to 80,
Hispanic people have higher severity at time of diagnosis. Asian, Black,
and Caucasian people appear to have similar severity at time of
diagnosis, though a slight gap emerges at around age 75.

``` r
# close-up look at severity by race and age
severity_race_age %>%
  filter(race_ethnicity %in% c("Caucasian", "Black or African American", "Hispanic")) %>% 
  filter(age_group > 65, age_group < 95) %>% 
  ggplot(aes(age_group, avg_severity)) +
  geom_line(aes(color = race_ethnicity)) +
  geom_point(aes(color = race_ethnicity)) +
  theme_light() +
  theme(
    panel.grid.minor = element_blank()
  ) +
  scale_y_continuous(breaks = c(48, 50, 52, 54, 56)) + 
  labs(
    title = "Severity by race and age group, between 70 to 90 years old", 
    y = "Average severity score at time of diagnosis", 
    x = "Age group (nearest multiple of 5)"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-91-1.png)<!-- -->

\[New classification\] Average severity at time of diagnosis is \~1.5-2
pts higher for Black people vs. Caucasian people, and \~3-4 pts higher
for Hispanic poeple as compared to Caucasian people

#### Severity by region

``` r
# severity by region

severity_reg <-
  timeseries_analysis %>% 
  group_by(region) %>% 
  summarize(average = mean(severity_score), n = n()) %>% 
  arrange(desc(average))

severity_reg %>% kable()
```

| region    |  average |     n |
|:----------|---------:|------:|
| West      | 59.06822 |  7666 |
| NA        | 58.78200 |   500 |
| South     | 58.68968 | 16834 |
| Midwest   | 57.01274 | 10124 |
| Northeast | 56.92221 |  8150 |

Severity in the West does appear to be a few points higher than in other
regions - perhaps due to a higher number of Hispanic people being from
the West.

#### Severity by race and insurance

``` r
# severity by race and insurace

severity_race_ins <-
  timeseries_analysis %>% 
  group_by(race_ethnicity, insurance) %>% 
  summarize(average = mean(severity_score), count = n()) %>% 
  arrange(desc(average))
```

    ## `summarise()` has grouped output by 'race_ethnicity'. You can override using the `.groups` argument.

``` r
severity_race_ins
```

    ## # A tibble: 18 × 4
    ## # Groups:   race_ethnicity [3]
    ##    race_ethnicity insurance       average count
    ##    <ord>          <ord>             <dbl> <int>
    ##  1 Hispanic       Govt               65.2   123
    ##  2 Hispanic       Unknown/Missing    63.3   640
    ##  3 Hispanic       Medicaid           63.2   712
    ##  4 Hispanic       Private            62.5  1553
    ##  5 White          Medicaid           61.9  1392
    ##  6 Hispanic       Medicare           61.9  3305
    ##  7 Black          Medicaid           61.6   371
    ##  8 White          Unknown/Missing    60.5   794
    ##  9 Black          Unknown/Missing    60.1   229
    ## 10 Hispanic       Military           59.3    30
    ## 11 White          Govt               59.2   862
    ## 12 Black          Private            59.0  1266
    ## 13 White          Private            58.9  7955
    ## 14 Black          Govt               58.7   180
    ## 15 Black          Medicare           58.0  3803
    ## 16 Black          Military           57.3    73
    ## 17 White          Military           55.9   435
    ## 18 White          Medicare           55.7 19551

``` r
# graph severity by race and insurance 

severity_race_ins %>% 
  filter(!race_ethnicity %in% c("Unknown", "Other")) %>% 
  ggplot(aes(x = insurance, y = average)) + 
  geom_col(aes(fill  = race_ethnicity), position = "dodge") +
  theme_light() +
  theme(
    axis.text.x = element_text(angle = 45, hjust = 1)
  ) +
  scale_fill_viridis_d() + 
  labs(
    title = "Severity by race and insurance",
    y = "Average severity at time of diagnosis",
    x = "Type of insurance"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-94-1.png)<!-- -->

We see large Hispanic/white gaps for those on Govt,Medicare FFS, Private
or with Unknown/Missing insurance information. Smaller Hispanic/white
gap for those on Medicaid or with Military insurance.

#### Severity by race and sex

``` r
# severity by race and sex

severity_race_sex <-
  timeseries_analysis %>% 
  group_by(race_ethnicity, gender) %>% 
  summarize(average = mean(severity_score), count = n()) %>% 
  arrange(desc(average))
```

    ## `summarise()` has grouped output by 'race_ethnicity'. You can override using the `.groups` argument.

``` r
severity_race_sex %>% head(5) %>% kable()
```

| race_ethnicity | gender  |  average | count |
|:---------------|:--------|---------:|------:|
| Hispanic       | Unknown | 64.34783 |    46 |
| Hispanic       | Male    | 63.22544 |  3389 |
| Hispanic       | Female  | 61.39993 |  2928 |
| White          | Unknown | 59.69298 |   114 |
| Black          | Male    | 59.69193 |  2441 |

``` r
# graphing severity by race and sex
severity_race_sex %>% 
  filter(!gender %in% c("Unknown")) %>% 
  ggplot(aes(x = race_ethnicity, y = average)) + 
  geom_point(aes(color = gender), size = 2) +
  theme_light() +
  theme(
    axis.text.x = element_text(angle = 45, hjust = 1)
  ) +
  scale_fill_viridis_d() + 
  labs(
    title = "Severity by race and sex"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-96-1.png)<!-- -->

Male patients have higher severity at the time of diagnosis than female
patients among all racial groups.

#### Severity by type of treatment

``` r
# severity by race and type of treatment

severity_race_first_treatment <-
  timeseries_analysis %>% 
  group_by(race_ethnicity, proc_group_28) %>% 
  summarize(average = mean(severity_score), count = n()) %>% 
  arrange(desc(average))
```

    ## `summarise()` has grouped output by 'race_ethnicity'. You can override using the `.groups` argument.

``` r
# severity_race_first_treatment
```

``` r
# graphing severity by race and type of treatment
severity_race_first_treatment %>% 
  ggplot(aes(x = race_ethnicity, y = average)) + 
  geom_point(aes(color = proc_group_28), size = 2) +
  theme_light() +
  theme(
    axis.text.x = element_text(angle = 45, hjust = 1)
  ) +
  scale_fill_viridis_d() + 
  labs(
    title = "Severity by race and first treatment type (28 days)"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-98-1.png)<!-- -->

Physicians are treating people with higher severity within their racial
group with antivegf or combo drugs for their first treatment, as
compared to laser or steroid.

``` r
# Likelihood of first treatment type by severity and race
treatment_sev_race <-
  timeseries_analysis %>% 
  count(severity_score, race_ethnicity, proc_group_28) %>% 
  group_by(severity_score, race_ethnicity) %>% 
  mutate(count_severity_race = sum(n)) %>% 
  mutate(prop = n / count_severity_race) %>% 
  ungroup() %>% 
  select(severity_score, race_ethnicity, proc_group_28, prop, count = n)

treatment_sev_race %>% 
  arrange(desc(severity_score, race_ethnicity))
```

    ## # A tibble: 33 × 5
    ##    severity_score race_ethnicity proc_group_28  prop count
    ##             <dbl> <ord>          <chr>         <dbl> <int>
    ##  1             85 White          antivegf          1    16
    ##  2             85 Black          antivegf          1    13
    ##  3             85 Hispanic       antivegf          1     9
    ##  4             81 White          antivegf          1    34
    ##  5             81 Black          antivegf          1    16
    ##  6             81 Hispanic       antivegf          1    11
    ##  7             78 White          antivegf          1  4075
    ##  8             78 Black          antivegf          1   863
    ##  9             78 Hispanic       antivegf          1  1276
    ## 10             73 White          antivegf          1  8274
    ## # … with 23 more rows

``` r
# Graphing likelihood of first treatment type by severity and race
treatment_sev_race %>% 
  filter(!race_ethnicity %in% c("Unknown", "Asian")) %>% 
  filter(severity_score > 50) %>% 
  ggplot(aes(x = severity_score, y = prop, color = race_ethnicity, shape = proc_group_28)) +
  geom_point() + 
  #facet_wrap("proc_group_28") +
  theme_bw() +
  #scale_y_continuous(labels = scales::percent, breaks = c(.70, .75, .8, .85, .9), limits = c(.7, .9)) +
  labs(
    title = "Likelihood of treatment at each severity score by race",
    y = "Percentage of people receiving this treatment"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-100-1.png)<!-- -->
\[New classification\] At this level, it is difficult to see how race
may be impacting treatment assigment. In the next graph, we look more
closely at the likelihood of receiving an anti-vegf treatment, based on
race.

``` r
# Graphing likelihood of antivegf as first treatment by severity and race
treatment_sev_race %>% 
  filter(!race_ethnicity %in% c("Unknown", "Asian")) %>% 
  filter(severity_score > 50) %>% 
  filter(proc_group_28 == "antivegf") %>% 
  ggplot(aes(x = severity_score, y = prop, color = race_ethnicity)) +
  geom_point() + 
  #facet_wrap("proc_group_28") +
  theme_bw() +
  scale_y_continuous(labels = scales::percent) +
  labs(
    title = "Likelihood of antivegf at each severity score by race",
    y = "Percentage of people receiving antivegf"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-101-1.png)<!-- -->

\[New classification\] Black people appear to be 2-5% points less likely
to receive antivegf treatment as compared to Caucasian people at several
severity scores (53, 58, 73, 78)

We can extend this analysis by looking at which antivegf drugs patients
of different races receive, at different severity levels.

#### Likelihood of drug received by severity score and race (given that patient recieved anti-vegf treatment)

``` r
# Calculate likelihood of drug by severity score and race (given that patient received antivegf treatment)
drug_sev_race <-
  timeseries_analysis %>% 
  filter(proc_group_365 == "antivegf") %>% 
  count(severity_score, race_ethnicity, vegf_group_365) %>% 
  group_by(severity_score, race_ethnicity) %>% 
  mutate(count_severity_race = sum(n)) %>% 
  mutate(prop = n / count_severity_race) %>% 
  ungroup() %>% 
  select(severity_score, race_ethnicity, vegf_group_365, prop, count = n)

drug_sev_race %>% 
  arrange(desc(severity_score, race_ethnicity))
```

    ## # A tibble: 129 × 5
    ##    severity_score race_ethnicity vegf_group_365   prop count
    ##             <dbl> <ord>          <chr>           <dbl> <int>
    ##  1             85 White          Avastin        0.6        9
    ##  2             85 White          combo          0.2        3
    ##  3             85 White          Eylea          0.133      2
    ##  4             85 White          Lucentis       0.0667     1
    ##  5             85 Black          Avastin        0.8        8
    ##  6             85 Black          combo          0.2        2
    ##  7             85 Hispanic       Avastin        0.333      3
    ##  8             85 Hispanic       combo          0.333      3
    ##  9             85 Hispanic       Eylea          0.333      3
    ## 10             81 White          Avastin        0.727     24
    ## # … with 119 more rows

``` r
# Graphing likelihood of first drug type by severity and race
drug_sev_race %>% 
  filter(!race_ethnicity %in% c("Unknown", "Asian", "Other")) %>% 
  ggplot(aes(x = severity_score, y = prop, color = race_ethnicity)) +
  geom_point() + 
  theme_bw() +
  facet_grid(~vegf_group_365) +
  #scale_y_continuous(labels = scales::percent, breaks = c(.70, .75, .8, .85, .9), limits = c(.7, .9)) +
  labs(
    title = "Likelihood of drug at each severity score by race",
    y = "Percentage of people receiving this drug"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-103-1.png)<!-- -->

``` r
# Graphing likelihood of Eylea as first treatment by severity and race
drug_sev_race %>% 
  filter(!race_ethnicity %in% c("Unknown", "Other")) %>% 
  filter(severity_score > 50) %>% 
  filter(vegf_group_365 == "Eylea") %>% 
  ggplot(aes(x = severity_score, y = prop, color = race_ethnicity)) +
  geom_point() + 
  theme_bw() +
  scale_y_continuous(labels = scales::percent) +
  labs(
    title = "Likelihood of receiving Eylea at higher severity score by race",
    y = "Percentage of people receiving Eylea"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-104-1.png)<!-- -->

Caucasian people are 5-10% more likely to receive Eylea treatment
(first) as compared to Hispanic people at the same level of severity,
and 2-5% more likely to receive Eylea treatment (first) as compared to
Black people at the same level of severity.

``` r
# Graphing likelihood of Bevacizumab as first treatment by severity and race
drug_sev_race %>% 
  filter(!race_ethnicity %in% c("Unknown", "Other")) %>% 
  filter(severity_score < 50) %>% 
  filter(vegf_group_365 == "Avastin") %>% 
  ggplot(aes(x = severity_score, y = prop, color = race_ethnicity)) +
  geom_point() + 
  geom_line() +
  theme_bw() +
  scale_y_continuous(labels = scales::percent) +
  labs(
    title = "Likelihood of receiving Bevacizumab at higher severity score by race",
    y = "Percentage of people receiving Bevacizumab"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-105-1.png)<!-- -->

At lower levels of severity, Hispanic people are far more likely
(15-25%) to receive Bevacizumab as compared to their Caucasian
counterparts with the same level of severity. Black and Asian people
seem somewhat more likely to receive Bevacizumab, but there is some
variation.

#### Likelihood of drug received by severity score CATEGORY and race (given that patient recieved anti-vegf treatment)

``` r
# Calculate likelihood of drug by severity score and race (given that patient received antivegf treatment)
drug_sev_cat_race <-
  timeseries_analysis %>% 
  filter(proc_group_365 == "antivegf") %>% 
  count(vision_category, race_ethnicity, vegf_group_365) %>% 
  group_by(vision_category, race_ethnicity) %>% 
  mutate(count_severity_race = sum(n)) %>% 
  mutate(prop = n / count_severity_race) %>% 
  ungroup() %>% 
  select(vision_category, race_ethnicity, vegf_group_365, prop, count = n)

drug_sev_cat_race %>% 
  arrange(desc(vision_category, race_ethnicity))
```

    ## # A tibble: 108 × 5
    ##    vision_category    race_ethnicity vegf_group_365   prop count
    ##    <chr>              <ord>          <chr>           <dbl> <int>
    ##  1 8 - PDR with edema White          Avastin        0.506   1848
    ##  2 8 - PDR with edema White          combo          0.191    699
    ##  3 8 - PDR with edema White          Eylea          0.209    762
    ##  4 8 - PDR with edema White          Lucentis       0.0939   343
    ##  5 8 - PDR with edema Black          Avastin        0.588    446
    ##  6 8 - PDR with edema Black          combo          0.182    138
    ##  7 8 - PDR with edema Black          Eylea          0.146    111
    ##  8 8 - PDR with edema Black          Lucentis       0.0831    63
    ##  9 8 - PDR with edema Hispanic       Avastin        0.678    729
    ## 10 8 - PDR with edema Hispanic       combo          0.156    168
    ## # … with 98 more rows

``` r
# Graphing likelihood of first drug type by severity and race
drug_sev_cat_race %>% 
  filter(!race_ethnicity %in% c("Unknown", "Asian", "Other"), !is.na(vision_category), !vision_category %in% c("NA")) %>% 
  mutate(prop_round = round(prop, 2)) %>% 
  ggplot(aes(x = vision_category, y = prop, fill = race_ethnicity)) +
  geom_col(position = "dodge") +
  theme_bw() +
  theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1)) +
  facet_grid(vegf_group_365~.) +
  scale_y_continuous(limits = c(0, 1)) +
  geom_text(aes(label = prop_round), vjust = -0.5, position = position_dodge(width = .9)) +
  #scale_x_continuous()
  #scale_y_continuous(labels = scales::percent, breaks = c(.70, .75, .8, .85, .9), limits = c(.7, .9)) +
  labs(
    title = "Likelihood of drug at each severity score by race",
    y = "Percentage of people receiving this drug"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-107-1.png)<!-- -->

``` r
# Graphing likelihood of Eylea as first treatment by severity and race
drug_sev_cat_race %>% 
  filter(!is.na(vision_category)) %>% 
  filter(vegf_group_365 == "Eylea") %>% 
  ggplot(aes(x = vision_category, y = prop, color = race_ethnicity)) +
  geom_point() + 
  theme_bw() +
  scale_y_continuous(labels = scales::percent) +
  labs(
    title = "Likelihood of receiving Eylea at higher vision_category by race",
    y = "Percentage of people receiving Eylea"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-108-1.png)<!-- -->

``` r
# Graphing likelihood of Bevacizumab as first treatment by severity and race
drug_sev_cat_race %>% 
  filter(!is.na(vision_category)) %>% 
  filter(vegf_group_365 == "Avastin") %>% 
  ggplot(aes(x = vision_category, y = prop, color = race_ethnicity)) +
  geom_point() + 
  geom_line() +
  theme_bw() +
  scale_y_continuous(labels = scales::percent) +
  labs(
    title = "Likelihood of receiving Bevacizumab at higher vision_category by race",
    y = "Percentage of people receiving Bevacizumab"
  )
```

    ## geom_path: Each group consists of only one observation. Do you need to
    ## adjust the group aesthetic?

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-109-1.png)<!-- -->

#### Likelihood of drug received by baseline VA and race (given that patient recieved anti-vegf treatment)

``` r
# Calculate likelihood of drug by severity score and race (given that patient received antivegf treatment)
drug_va_race <-
  timeseries_analysis %>% 
  filter(proc_group_365 == "antivegf") %>% 
  count(baseline_va_r10, race_ethnicity, vegf_group_365) %>% 
  group_by(baseline_va_r10, race_ethnicity) %>% 
  mutate(count_va_race = sum(n)) %>% 
  mutate(prop = n / count_va_race) %>% 
  ungroup() %>% 
  select(baseline_va_r10, race_ethnicity, vegf_group_365, prop, count = n)

drug_va_race %>% 
  arrange(desc(baseline_va_r10, race_ethnicity))
```

    ## # A tibble: 94 × 5
    ##    baseline_va_r10 race_ethnicity vegf_group_365   prop count
    ##              <dbl> <ord>          <chr>           <dbl> <int>
    ##  1             100 Black          Eylea          1          2
    ##  2             100 Hispanic       Avastin        1          1
    ##  3              90 White          Avastin        0.452     19
    ##  4              90 White          combo          0.0952     4
    ##  5              90 White          Eylea          0.238     10
    ##  6              90 White          Lucentis       0.214      9
    ##  7              90 Black          Avastin        0.4        2
    ##  8              90 Black          combo          0.2        1
    ##  9              90 Black          Eylea          0.2        1
    ## 10              90 Black          Lucentis       0.2        1
    ## # … with 84 more rows

``` r
# Graphing likelihood of first drug type by severity and race
drug_va_race %>% 
  filter(baseline_va_r10 <= 80) %>% 
  ggplot(aes(x = baseline_va_r10, y = prop, color = race_ethnicity)) +
  geom_point() + 
  geom_line() +
  theme_bw() +
  facet_grid(vegf_group_365~.) +
  #scale_y_continuous(labels = scales::percent, breaks = c(.70, .75, .8, .85, .9), limits = c(.7, .9)) +
  labs(
    title = "Likelihood of drug at each baseline va (rounded) by race",
    y = "Percentage of people receiving this drug"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-111-1.png)<!-- -->

#### Likelihood of drug received by severity score CATEGORY and insurance (given that patient recieved anti-vegf treatment)

``` r
# Calculate likelihood of drug by severity score and insurance (given that patient received antivegf treatment)
drug_sev_cat_insurance <-
  timeseries_analysis %>%
  filter(proc_group_365 == "antivegf", insurance %in% c("Private", "Medicare", "Medicaid")) %>% 
  count(vision_category, insurance, vegf_group_365) %>% 
  group_by(vision_category, insurance) %>% 
  mutate(count_severity_race = sum(n)) %>% 
  mutate(prop = n / count_severity_race) %>% 
  ungroup() %>% 
  select(vision_category, insurance, vegf_group_365, prop, count = n)

drug_sev_cat_insurance %>% 
  arrange(desc(vision_category, insurance))
```

    ## # A tibble: 107 × 5
    ##    vision_category    insurance vegf_group_365   prop count
    ##    <chr>              <ord>     <chr>           <dbl> <int>
    ##  1 8 - PDR with edema Private   Avastin        0.535    824
    ##  2 8 - PDR with edema Private   combo          0.192    296
    ##  3 8 - PDR with edema Private   Eylea          0.182    281
    ##  4 8 - PDR with edema Private   Lucentis       0.0909   140
    ##  5 8 - PDR with edema Medicare  Avastin        0.528   1561
    ##  6 8 - PDR with edema Medicare  combo          0.185    547
    ##  7 8 - PDR with edema Medicare  Eylea          0.197    583
    ##  8 8 - PDR with edema Medicare  Lucentis       0.0890   263
    ##  9 8 - PDR with edema Medicaid  Avastin        0.680    329
    ## 10 8 - PDR with edema Medicaid  combo          0.151     73
    ## # … with 97 more rows

``` r
# Graphing likelihood of first drug type by severity and insurance
drug_sev_cat_insurance %>% 
  filter(!is.na(vision_category), !vision_category %in% c("NA")) %>% 
  mutate(prop_round = round(prop, 2)) %>% 
  ggplot(aes(x = vision_category, y = prop, fill = insurance)) +
  geom_col(position = "dodge") +
  theme_bw() +
  theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1)) +
  facet_grid(vegf_group_365~.) +
  scale_y_continuous(limits = c(0, 1)) +
  #geom_text(aes(label = prop_round), vjust = -0.5, position = position_dodge(width = .9)) +
  #scale_x_continuous()
  #scale_y_continuous(labels = scales::percent, breaks = c(.70, .75, .8, .85, .9), limits = c(.7, .9)) +
  labs(
    title = "Likelihood of drug at each severity score by insurance",
    y = "Percentage of people receiving this drug"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-113-1.png)<!-- -->

``` r
# Graphing likelihood of Eylea as first treatment by severity and insurance
drug_sev_cat_insurance %>% 
  filter(!is.na(vision_category)) %>% 
  filter(vegf_group_365 == "Eylea") %>% 
  ggplot(aes(x = vision_category, y = prop, color = insurance)) +
  geom_point() + 
  theme_bw() +
  theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1)) +
  scale_y_continuous(labels = scales::percent) +
  labs(
    title = "Likelihood of receiving Eylea at higher vision_category by race",
    y = "Percentage of people receiving Eylea"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-114-1.png)<!-- -->

``` r
# Graphing likelihood of Bevacizumab as first treatment by severity and insurance
drug_sev_cat_insurance %>% 
  filter(!is.na(vision_category)) %>% 
  filter(vegf_group_365 == "Avastin") %>% 
  ggplot(aes(x = vision_category, y = prop, color = insurance)) +
  geom_point() + 
  geom_line() +
  theme_bw() +
  theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1)) +
  scale_y_continuous(labels = scales::percent) +
  labs(
    title = "Likelihood of receiving Bevacizumab at higher vision_category by race",
    y = "Percentage of people receiving Bevacizumab"
  )
```

    ## geom_path: Each group consists of only one observation. Do you need to
    ## adjust the group aesthetic?

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-115-1.png)<!-- -->

#### Drug received by race and insurance

Part of the reason there may be variation by race in which treatment
drug is received is due to insurance. Medical providers may only choose
drugs covered by the patients insurance (e.g., Medicare does not cover
Eylea).

``` r
# Calculate likelihood of drug by severity score and race (given that patient received antivegf treatment)
drug_sev_race_ins <-
  timeseries_analysis %>% 
  filter(proc_group_28 == "antivegf") %>% 
  count(severity_score, race_ethnicity, vegf_group_28, insurance) %>% 
  group_by(severity_score, race_ethnicity, insurance) %>% 
  mutate(count_severity_race = sum(n)) %>% 
  mutate(prop = n / count_severity_race) %>% 
  ungroup() %>% 
  select(severity_score, race_ethnicity, insurance, vegf_group_28, prop, count = n)

drug_sev_race_ins %>% 
  arrange(desc(severity_score, race_ethnicity, insurance))
```

    ## # A tibble: 563 × 6
    ##    severity_score race_ethnicity insurance       vegf_group_28  prop count
    ##             <dbl> <ord>          <ord>           <chr>         <dbl> <int>
    ##  1             85 White          Private         Avastin       1         7
    ##  2             85 White          Medicare        Avastin       0.571     4
    ##  3             85 White          Medicaid        Avastin       1         1
    ##  4             85 White          Unknown/Missing Avastin       1         1
    ##  5             85 White          Medicare        Eylea         0.286     2
    ##  6             85 White          Medicare        Lucentis      0.143     1
    ##  7             85 Black          Private         Avastin       1         6
    ##  8             85 Black          Medicare        Avastin       1         6
    ##  9             85 Black          Medicaid        Lucentis      1         1
    ## 10             85 Hispanic       Private         Avastin       0.5       1
    ## # … with 553 more rows

``` r
# Graphing likelihood of Eylea as first treatment by severity, race, and Medicare FFS  insurance
drug_sev_race_ins %>% 
  filter(!race_ethnicity %in% c("Unknown", "Other", "Asian")) %>% 
  filter(severity_score > 50) %>% 
  filter(vegf_group_28 == "Eylea") %>% 
  filter(insurance == "Medicare") %>% 
  ggplot(aes(x = severity_score, y = prop, color = race_ethnicity)) +
  geom_point() + 
  geom_line() +
  theme_bw() +
  scale_y_continuous(labels = scales::percent) +
  labs(
    title = "Likelihood of receiving Eylea at higher severity score by race, with Medicare insurance",
    y = "Percentage of people receiving Eylea"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-117-1.png)<!-- -->

``` r
# Graphing likelihood of Eylea as first treatment by severity, race, and Medicaid insurance
drug_sev_race_ins %>% 
  # Asian filtered due to too small data size
  filter(!race_ethnicity %in% c("Unknown", "Other", "Asian")) %>% 
  filter(severity_score > 50) %>% 
  filter(vegf_group_28 == "Eylea") %>% 
  filter(insurance == "Medicaid") %>% 
  ggplot(aes(x = severity_score, y = prop, color = race_ethnicity)) +
  geom_point() + 
  geom_line() +
  theme_bw() +
  scale_y_continuous(labels = scales::percent) +
  labs(
    title = "Likelihood of receiving Eylea at higher severity score by race, with Medicaid insurance",
    y = "Percentage of people receiving Eylea"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-118-1.png)<!-- -->

``` r
# Graphing likelihood of Eylea as first treatment by severity, race, and private insurance
drug_sev_race_ins %>% 
  # Asian filtered due to too small data size
  filter(!race_ethnicity %in% c("Unknown", "Other", "Asian")) %>% 
  filter(severity_score > 50) %>% 
  filter(vegf_group_28 == "Eylea") %>% 
  filter(insurance == "Private") %>% 
  ggplot(aes(x = severity_score, y = prop, color = race_ethnicity)) +
  geom_point() + 
  geom_line() +
  theme_bw() +
  scale_y_continuous(labels = scales::percent) +
  labs(
    title = "Likelihood of receiving Eylea at higher severity score by race, with private insurance",
    y = "Percentage of people receiving Eylea"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-119-1.png)<!-- -->

``` r
# Calculate likelihood of drug by severity score, race, baseline va (given that patient received antivegf treatment)
drug_sev_race_va <-
  timeseries_analysis %>% 
  filter(proc_group_28 == "antivegf") %>% 
  count(severity_score, race_ethnicity, vegf_group_28,  baseline_va_r10) %>% 
  group_by(severity_score, race_ethnicity) %>% 
  mutate(count_severity_race_ins_va = sum(n)) %>% 
  mutate(prop = n / count_severity_race_ins_va) %>% 
  ungroup() %>% 
  select(severity_score, race_ethnicity, vegf_group_28, baseline_va_r10, prop, count = n)
```

``` r
# Graphing likelihood of Eylea as first treatment by severity, race, and private insurance
drug_sev_race_va %>% 
  # Asian filtered due to too small data size
  filter(!race_ethnicity %in% c("Unknown", "Other", "Asian")) %>% 
  filter(vegf_group_28 == "Eylea") %>% 
  filter(count > 25) %>%
  filter(baseline_va_r10 == 60) %>% 
  ggplot(aes(x = severity_score, y = prop, color = race_ethnicity)) +
  geom_point() + 
  geom_line() +
  theme_bw() +
  scale_y_continuous(labels = scales::percent) +
  labs(
    title = "Likelihood of receiving Eylea at higher severity score by race",
    y = "Percentage of people receiving Eylea"
  ) 
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-121-1.png)<!-- -->

#### Severity vs. vision

Do severity scores differ by race for people with the same vision score?

``` r
#calculate severity by race and age

severity_race_vision <-
  timeseries_analysis %>% 
  group_by(race_ethnicity, baseline_va_letter) %>%
  summarize(avg_severity = mean(severity_score), count = n()) %>% 
  filter(count > 100) %>% 
  arrange(desc(avg_severity))
```

    ## `summarise()` has grouped output by 'race_ethnicity'. You can override using the `.groups` argument.

``` r
severity_race_vision
```

    ## # A tibble: 36 × 4
    ## # Groups:   race_ethnicity [3]
    ##    race_ethnicity baseline_va_letter avg_severity count
    ##    <ord>                       <dbl>        <dbl> <int>
    ##  1 Hispanic                       40         65.6   140
    ##  2 Hispanic                       35         64.1   373
    ##  3 Hispanic                       50         64.0   348
    ##  4 Hispanic                       55         63.9   355
    ##  5 Hispanic                       61         63.3   616
    ##  6 Hispanic                       85         62.5   374
    ##  7 White                          45         62.4   161
    ##  8 Hispanic                       65         62.3   847
    ##  9 Black                          35         62.2   317
    ## 10 Hispanic                       58         62.1   381
    ## # … with 26 more rows

``` r
# graph severity by race and age group

severity_race_vision %>%
  filter(race_ethnicity %in% c("Caucasian", "Black or African American", "Hispanic", "Asian")) %>% 
  ggplot(aes(x = baseline_va_letter, y = avg_severity, color = race_ethnicity)) +
  geom_line() +
  geom_point() +
  theme_light() +
  labs(
    title = "Severity by race and baseline VA", 
    y = "Average severity score", 
    x = "Baseline VA"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-123-1.png)<!-- -->

Hispanic people are more likely to have a higher starting severity score
at all levels of baseline visual acuity, as compared to Black, Asian,
and Caucasian people.

``` r
#calculate severity by race and age

severity_race_vision_flip <-
  timeseries_analysis %>% 
  group_by(race_ethnicity, severity_score) %>%
  summarize(avg_va = mean(baseline_va_letter), count = n()) %>% 
  filter(count > 100) %>% 
  arrange(desc(avg_va))
```

    ## `summarise()` has grouped output by 'race_ethnicity'. You can override using the `.groups` argument.

``` r
severity_race_vision_flip %>%
  filter(race_ethnicity %in% c("Caucasian", "Black or African American", "Hispanic")) %>% 
  ggplot(aes(x = severity_score, y = avg_va, color = race_ethnicity)) +
  geom_line() +
  geom_point() +
  theme_light() +
  labs(
    title = "Severity by race and baseline VA", 
    x = "Severity score", 
    y = "Average Baseline VA"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-124-1.png)<!-- -->

## Regression

#### Linear regression

Start with a linear regression to see what variables are initially
predictive

``` r
# basic linear regression
# one_year_va_delta_1.lm <- 
#   lm(
#     one_year_va_delta ~ relevel(race_ethnicity, ref = "Caucasian") + insurance + gender + first_dr_age + smoke_status + baseline_va_letter, 
#     data = timeseries_analysis %>% mutate(race_ethnicity = factor(race_ethnicity))
#     )
# 
# summary(one_year_va_delta_1.lm)
```

``` r
# linear regression with insurance race interaction
# one_year_va_delta_2.lm <- 
#   lm(
#     one_year_va_delta ~ relevel(race_ethnicity, ref = "Caucasian") + insurance + relevel(race_ethnicity, ref = "Caucasian") * insurance + gender + first_dr_age + smoke_status + baseline_va_letter, 
#     data = timeseries_analysis %>% 
#       filter(insurance %in% c("Medicare", "Medicaid", "Private")) %>% 
#       mutate(race_ethnicity = factor(race_ethnicity))
#   )
# 
# summary(one_year_va_delta_2.lm)
```

``` r
# linear regression with severity score
# one_year_va_delta_3.lm <- 
#   lm(one_year_va_delta ~ race_ethnicity + insurance + gender + first_dr_age + smoke_status + baseline_va_letter + severity_score, 
#      data = timeseries_analysis)
# 
# summary(one_year_va_delta_3.lm)
```

``` r
# linear regression with race insurance interaction and severity score
# one_year_va_delta_4.lm <- 
#   lm(one_year_va_delta ~ race_ethnicity + insurance + race_ethnicity * insurance + gender + first_dr_age + smoke_status + baseline_va_letter + severity_score, 
#      data = timeseries_analysis %>% 
#        filter(insurance %in% c("Medicare", "Medicaid", "Private")) %>% 
#        mutate(
#          race_ethnicity = factor(race_ethnicity),
#          race_ethnicity = relevel(race_ethnicity, ref = "Caucasian"),
#          insurance = factor(insurance), 
#          insurance = relevel(insurance, ref = "Private")
#        )
#   ) 
# 
# summary(one_year_va_delta_4.lm)
```

#### Quantile regressionn
