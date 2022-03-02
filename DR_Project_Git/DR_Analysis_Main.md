DR_Analysis_Main
================
Vikas Maturi
2021-07-07

-   [Setup & import](#setup--import)
    -   [Libraries and parameters](#libraries-and-parameters)
    -   [Data import](#data-import)
    -   [Join the two datasets](#join-the-two-datasets)
-   [Cleaned datasets for analysis](#cleaned-datasets-for-analysis)
-   [Table 1: Patient Demographics](#table-1-patient-demographics)
    -   [Construct dataset to generate data
        tables](#construct-dataset-to-generate-data-tables)
    -   [Table: Age by race and
        insurance](#table-age-by-race-and-insurance)
    -   [Table: Gender by race and
        insurance](#table-gender-by-race-and-insurance)
    -   [Table: Baseline VA by race and
        insurance](#table-baseline-va-by-race-and-insurance)
    -   [Table: VEGF drug by race and
        insurance](#table-vegf-drug-by-race-and-insurance)
    -   [Table: Smoke status by race and
        insurance](#table-smoke-status-by-race-and-insurance)
    -   [Table: Insurance by race](#table-insurance-by-race)
-   [Figure 1: Baseline Differences in Severity Score by
    Race](#figure-1-baseline-differences-in-severity-score-by-race)
-   [Figure 2A and 2B: Differences in Rates of Aflibercept and
    Bevacizumab Injection by Severity Score, Race and Insurance
    Status](#figure-2a-and-2b-differences-in-rates-of-aflibercept-and-bevacizumab-injection-by-severity-score-race-and-insurance-status)
-   [Regression X: Variables that influence likelihood of receiving
    Avastin
    treatment](#regression-x-variables-that-influence-likelihood-of-receiving-avastin-treatment)
-   [Figure 3A and 3B: Visual Acuity Change from Baseline at One and Two
    Years, by
    Race](#figure-3a-and-3b-visual-acuity-change-from-baseline-at-one-and-two-years-by-race)
-   [Figure 4A, 4B, 4C: Visual Acuity Change from Baseline at One and
    Two Years, by Insurance and
    Race](#figure-4a-4b-4c-visual-acuity-change-from-baseline-at-one-and-two-years-by-insurance-and-race)
-   [Regression X: Variables that influence change in vision after one
    year](#regression-x-variables-that-influence-change-in-vision-after-one-year)
-   [Figure 5: Average severity score at each baseline VA (rounded to
    nearest 5) by
    race](#figure-5-average-severity-score-at-each-baseline-va-rounded-to-nearest-5-by-race)
-   [Figure 6: Average severity score at each baseline VA (rounded to
    nearest 5) by
    insurance](#figure-6-average-severity-score-at-each-baseline-va-rounded-to-nearest-5-by-insurance)
-   [Timeseries analysis of visual
    acuity](#timeseries-analysis-of-visual-acuity)
    -   [Distribution of VA by race](#distribution-of-va-by-race)
    -   [Timeseries VA by race and
        severity](#timeseries-va-by-race-and-severity)
    -   [Timeseries VA by race](#timeseries-va-by-race)
    -   [Timeseries VA by insurance](#timeseries-va-by-insurance)
    -   [Percentage with 15 letter loss by
        race](#percentage-with-15-letter-loss-by-race)
    -   [Percentage with 15 letter loss by
        insurance](#percentage-with-15-letter-loss-by-insurance)
    -   [Timeseries VA by insurance and
        race](#timeseries-va-by-insurance-and-race)
    -   [Timeseries VA by smoking and
        race](#timeseries-va-by-smoking-and-race)
    -   [Timeseries VA by gender and
        race](#timeseries-va-by-gender-and-race)
    -   [Race VA timeseries](#race-va-timeseries)
    -   [Race VA timeseries bucketed by
        10](#race-va-timeseries-bucketed-by-10)
    -   [Severity race VA timeseries](#severity-race-va-timeseries)
    -   [Severity insurance race VA
        timeseries](#severity-insurance-race-va-timeseries)
-   [Injections by VA analysis](#injections-by-va-analysis)
-   [Baseline analysis](#baseline-analysis)
    -   [Severity by race](#severity-by-race)
    -   [Severity by race and region](#severity-by-race-and-region)
    -   [Severity by race and age](#severity-by-race-and-age)
    -   [Severity by region](#severity-by-region)
    -   [Severity by race and
        insurance](#severity-by-race-and-insurance)
    -   [Severity by race and sex](#severity-by-race-and-sex)
    -   [Severity by type of treatment](#severity-by-type-of-treatment)
    -   [Likelihood of drug received by severity score and race (given
        that patient recieved anti-vegf
        treatment)](#likelihood-of-drug-received-by-severity-score-and-race-given-that-patient-recieved-anti-vegf-treatment)
    -   [Likelihood of drug received by severity score CATEGORY and race
        (given that patient recieved anti-vegf
        treatment)](#likelihood-of-drug-received-by-severity-score-category-and-race-given-that-patient-recieved-anti-vegf-treatment)
    -   [Likelihood of drug received by baseline VA and race (given that
        patient recieved anti-vegf
        treatment)](#likelihood-of-drug-received-by-baseline-va-and-race-given-that-patient-recieved-anti-vegf-treatment)
    -   [Likelihood of drug received by severity score CATEGORY and
        insurance (given that patient recieved anti-vegf
        treatment)](#likelihood-of-drug-received-by-severity-score-category-and-insurance-given-that-patient-recieved-anti-vegf-treatment)
    -   [Drug received by race and
        insurance](#drug-received-by-race-and-insurance)
    -   [Severity vs. vision](#severity-vs-vision)
-   [Regression](#regression)
    -   [Linear regression](#linear-regression)
    -   [Quantile regressionn](#quantile-regressionn)

## Setup & import

#### Libraries and parameters

``` r
# load libraries
library(tidyverse)
library(readxl)
library(lubridate)
library(knitr)
library(googlesheets4)
```

#### Data import

``` r
# read in cleaned, combined data on visits and severity
visits_severity <- read_csv(file = "data_clean/maturi_all_cleaned.csv")

# read in cleaned data on timeservies visual acuity
va_progression <- read_csv(file = "data_clean/va_progression.csv")
```

#### Join the two datasets

``` r
joined_data <- 
  visits_severity %>% 
  # select the data
  dplyr::select(pt_code_name, eye, first_problem_code, severity_score, gender, race_ethnicity, first_dr_age, age_group, insurance, region, smoke_status, new_class, valid_class, include_given_code, vision_category, vision_threatening, pt_code_name, eye, index_date, baseline_va_letter, proc_group_28, proc_group_365, proc_group_any, vegf_group_28, vegf_group_365, vegf_group_any, retina_speciality, baseline_iop, pdr_group, cat_eyes) %>% 
  left_join(va_progression, by = c("pt_code_name" = "patient_guid", "eye" = "va_eye", "index_date"))
```

## Cleaned datasets for analysis

The ‘timeseries_analysis’ dataset will be the basis of future analyses

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
    two_year_va_delta = two_year - baseline_va_letter
  ) 
```

## Table 1: Patient Demographics

#### Construct dataset to generate data tables

``` r
# Create dataset for basic paper tables
tbl_data <-
  timeseries_analysis %>% 
  mutate(
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
    ##    <chr>        <dbl> <chr>                     <dbl> <chr>  <chr>         
    ##  1 00022808cb4…     1 362.02                       73 Male   Caucasian     
    ##  2 0002d4cc5f7…     1 362.02                       73 Male   Hispanic      
    ##  3 000340244d4…     1 E11.3411                     58 Male   Caucasian     
    ##  4 0003859ca8d…     1 E11.351                      78 Female Caucasian     
    ##  5 000406a5939…     1 362.05                       44 Male   Caucasian     
    ##  6 0007e418646…     1 362.02                       73 Female Black or Afri…
    ##  7 000944c41d8…     1 362.05                       44 Male   Caucasian     
    ##  8 00096924cfb…     1 E11.3211                     40 Male   Caucasian     
    ##  9 000aadb4949…     2 362.02                       73 Female Caucasian     
    ## 10 000b59b304e…     2 362.04                       35 Male   Caucasian     
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

| age_tbl | Black or African American | Caucasian    | Hispanic     |
|:--------|:--------------------------|:-------------|:-------------|
| \< 53   | 1258 (21.243              | 5856 (18.897 | 1705 (26.796 |
| \> 70   | 1148 (19.385              | 6508 (21.001 | 911 (14.317  |
| 53 - 58 | 996 (16.819               | 4986 (16.09  | 1191 (18.718 |
| 59 - 64 | 1260 (21.277              | 6591 (21.269 | 1381 (21.704 |
| 65 - 70 | 1260 (21.277              | 7048 (22.744 | 1175 (18.466 |

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

| gender  | Black or African American | Caucasian     | Hispanic     |
|:--------|:--------------------------|:--------------|:-------------|
| Female  | 3455 (58.342              | 13715 (44.258 | 2928 (46.016 |
| Male    | 2441 (41.219              | 17160 (55.374 | 3389 (53.261 |
| Unknown | 26 (0.439                 | 114 (0.368    | 46 (0.723    |

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

| va_tbl          | Black or African American | Caucasian     | Hispanic     |
|:----------------|:--------------------------|:--------------|:-------------|
| 20/201 or worse | 53 (0.895                 | 221 (0.713    | 64 (1.006    |
| 20/40 or better | 3164 (53.428              | 17545 (56.617 | 3128 (49.159 |
| 20/41 - 20/70   | 1650 (27.862              | 8471 (27.336  | 1899 (29.844 |
| 20/71 - 20/200  | 1055 (17.815              | 4752 (15.334  | 1272 (19.991 |

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

| vegf_tbl    | Black or African American | Caucasian     | Hispanic     |
|:------------|:--------------------------|:--------------|:-------------|
| aflibercept | 984 (16.616               | 6241 (20.139  | 673 (10.577  |
| bevacizumab | 3078 (51.976              | 14036 (45.293 | 4175 (65.614 |
| combo       | 1157 (19.537              | 6408 (20.678  | 1051 (16.517 |
| ranibizumab | 703 (11.871               | 4304 (13.889  | 464 (7.292   |

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

| smoke_status                                    | Black or African American | Caucasian     | Hispanic     |
|:------------------------------------------------|:--------------------------|:--------------|:-------------|
| Former / No longer active / Past History / Quit | 1434 (24.215              | 8753 (28.246  | 1386 (21.782 |
| No / Never                                      | 3846 (64.944              | 18345 (59.198 | 4341 (68.223 |
| Unknown / Unclassified                          | 60 (1.013                 | 352 (1.136    | 67 (1.053    |
| Yes / Active                                    | 582 (9.828                | 3539 (11.42   | 569 (8.942   |

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

| insurance_tbl | race_ethnicity            |     n | pct_insurance_race |
|:--------------|:--------------------------|------:|-------------------:|
| Medicaid      | Black or African American |   371 |           6.264775 |
| Medicaid      | Caucasian                 |  1392 |           4.491917 |
| Medicaid      | Hispanic                  |   712 |          11.189690 |
| Medicare      | Black or African American |  3803 |          64.218170 |
| Medicare      | Caucasian                 | 19551 |          63.090129 |
| Medicare      | Hispanic                  |  3305 |          51.940908 |
| Other         | Black or African American |   482 |           8.139142 |
| Other         | Caucasian                 |  2091 |           6.747556 |
| Other         | Hispanic                  |   793 |          12.462675 |
| Private       | Black or African American |  1266 |          21.377913 |
| Private       | Caucasian                 |  7955 |          25.670399 |
| Private       | Hispanic                  |  1553 |          24.406726 |

## Figure 1: Baseline Differences in Severity Score by Race

To consider here: how do we want to respond to the NAs in this graph?
One option I could see is creating four new categories: (mild NPDR
unknown edema, moderate NPDR unknown edema, severe NPDR unknown edema,
PDR unknown edema)

``` r
timeseries_analysis %>% 
  dplyr::count(vision_category, race_ethnicity) %>%
  dplyr::ungroup() %>% 
  dplyr::group_by(race_ethnicity) %>% 
  dplyr::mutate(prop_category = n / sum(n)) %>% 
  ggplot() + 
  geom_col(aes(x = vision_category, y = prop_category, group = race_ethnicity, fill = race_ethnicity), position = "dodge", alpha = .2) +
  theme_light() +
  theme(
    axis.text.x = element_text(angle = 45, hjust = 1)
  ) +
  scale_y_continuous(labels = scales::label_percent(accuracy = 10)) +
  labs(
    x = "Severity Category",
    y = "Percentage ",
    fill = "Race"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

## Figure 2A and 2B: Differences in Rates of Aflibercept and Bevacizumab Injection by Severity Score, Race and Insurance Status

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
    ##    vision_category    race_ethnicity            vegf_group_365   prop count
    ##    <chr>              <chr>                     <chr>           <dbl> <int>
    ##  1 8 - PDR with edema Black or African American Avastin        0.588    446
    ##  2 8 - PDR with edema Black or African American combo          0.182    138
    ##  3 8 - PDR with edema Black or African American Eylea          0.146    111
    ##  4 8 - PDR with edema Black or African American Lucentis       0.0831    63
    ##  5 8 - PDR with edema Caucasian                 Avastin        0.506   1848
    ##  6 8 - PDR with edema Caucasian                 combo          0.191    699
    ##  7 8 - PDR with edema Caucasian                 Eylea          0.209    762
    ##  8 8 - PDR with edema Caucasian                 Lucentis       0.0939   343
    ##  9 8 - PDR with edema Hispanic                  Avastin        0.678    729
    ## 10 8 - PDR with edema Hispanic                  combo          0.156    168
    ## # … with 98 more rows

``` r
# Graphing likelihood of first drug type by severity and race
drug_sev_cat_race %>% 
  # remove individuals with no vision category
  filter(!is.na(vision_category), !vision_category %in% c("NA")) %>% 
  # remove combo, Eylea, and Lucentis for this graph
  filter(vegf_group_365 == "Avastin") %>% 
  # round proportion to nearest 100th
  mutate(pct_round = round(prop, 2)) %>% 
  ggplot(aes(x = vision_category, y = pct_round, fill = race_ethnicity)) +
  geom_col(position = "dodge") +
  theme_bw() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1)) +
  facet_grid(vegf_group_365 ~ .) +
  #geom_text(aes(label = paste0(pct_round, "%")), vjust = -0.5, position = position_dodge(width = 1)) +
  #scale_x_continuous()
  scale_y_continuous(labels = scales::label_percent(accuracy = 1), limits = c(0, 1)) +
  labs(
    x = "Severity category",
    y = "Percentage of people receiving Avastin",
    fill = "Race"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

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
    ##    <chr>              <chr>     <chr>           <dbl> <int>
    ##  1 8 - PDR with edema Medicaid  Avastin        0.680    329
    ##  2 8 - PDR with edema Medicaid  combo          0.151     73
    ##  3 8 - PDR with edema Medicaid  Eylea          0.118     57
    ##  4 8 - PDR with edema Medicaid  Lucentis       0.0517    25
    ##  5 8 - PDR with edema Medicare  Avastin        0.528   1561
    ##  6 8 - PDR with edema Medicare  combo          0.185    547
    ##  7 8 - PDR with edema Medicare  Eylea          0.197    583
    ##  8 8 - PDR with edema Medicare  Lucentis       0.0890   263
    ##  9 8 - PDR with edema Private   Avastin        0.535    824
    ## 10 8 - PDR with edema Private   combo          0.192    296
    ## # … with 97 more rows

``` r
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
  theme_bw() +
  theme(axis.text.x = element_text(angle = 45, hjust=1)) +
  facet_grid(vegf_group_365~.) +
  scale_y_continuous(labels = scales::label_percent(accuracy = 1), limits = c(0, 1)) +
  # different colors for insurance
  scale_fill_viridis_d() +
  labs(
    x = "Severity category",
    y = "Percentage of people receiving Avastin",
    fill = "Insurance"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->

## Regression X: Variables that influence likelihood of receiving Avastin treatment

``` r
avastin_1.lm <- 
  lm(avastin ~ race_ethnicity + insurance + race_ethnicity * insurance + gender + first_dr_age + smoke_status + baseline_va_letter +  severity_score, 
     data = timeseries_analysis %>% 
       filter(
         insurance %in% c("Medicare", "Medicaid", "Private")
       ) %>% 
       mutate(
         race_ethnicity = factor(race_ethnicity),
         race_ethnicity = relevel(race_ethnicity, ref = "Caucasian"),
         insurance = factor(insurance), 
         insurance = relevel(insurance, ref = "Private"),
         smoke_status = factor(smoke_status),
         smoke_status = relevel(smoke_status, ref = "No / Never"),
         avastin = if_else(vegf_group_365 == "Avastin", 1, 0)
       ) 
  ) 

summary(avastin_1.lm)
```

    ## 
    ## Call:
    ## lm(formula = avastin ~ race_ethnicity + insurance + race_ethnicity * 
    ##     insurance + gender + first_dr_age + smoke_status + baseline_va_letter + 
    ##     severity_score, data = timeseries_analysis %>% filter(insurance %in% 
    ##     c("Medicare", "Medicaid", "Private")) %>% mutate(race_ethnicity = factor(race_ethnicity), 
    ##     race_ethnicity = relevel(race_ethnicity, ref = "Caucasian"), 
    ##     insurance = factor(insurance), insurance = relevel(insurance, 
    ##         ref = "Private"), smoke_status = factor(smoke_status), 
    ##     smoke_status = relevel(smoke_status, ref = "No / Never"), 
    ##     avastin = if_else(vegf_group_365 == "Avastin", 1, 0)))
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -0.8783 -0.4540 -0.3376  0.4985  0.7298 
    ## 
    ## Coefficients:
    ##                                                               Estimate
    ## (Intercept)                                                  0.2975053
    ## race_ethnicityBlack or African American                      0.0287778
    ## race_ethnicityHispanic                                       0.1489780
    ## insuranceMedicaid                                            0.1058042
    ## insuranceMedicare                                            0.0233615
    ## genderMale                                                  -0.0226201
    ## genderUnknown                                               -0.0239314
    ## first_dr_age                                                -0.0023127
    ## smoke_statusFormer / No longer active / Past History / Quit  0.0048371
    ## smoke_statusUnknown / Unclassified                           0.0246780
    ## smoke_statusYes / Active                                     0.0329755
    ## baseline_va_letter                                           0.0012265
    ## severity_score                                               0.0034629
    ## race_ethnicityBlack or African American:insuranceMedicaid    0.0425301
    ## race_ethnicityHispanic:insuranceMedicaid                     0.0210238
    ## race_ethnicityBlack or African American:insuranceMedicare    0.0437979
    ## race_ethnicityHispanic:insuranceMedicare                     0.0293485
    ##                                                             Std. Error
    ## (Intercept)                                                  0.0252844
    ## race_ethnicityBlack or African American                      0.0148308
    ## race_ethnicityHispanic                                       0.0136050
    ## insuranceMedicaid                                            0.0143058
    ## insuranceMedicare                                            0.0072044
    ## genderMale                                                   0.0050211
    ## genderUnknown                                                0.0374106
    ## first_dr_age                                                 0.0002556
    ## smoke_statusFormer / No longer active / Past History / Quit  0.0057660
    ## smoke_statusUnknown / Unclassified                           0.0242382
    ## smoke_statusYes / Active                                     0.0081407
    ## baseline_va_letter                                           0.0001950
    ## severity_score                                               0.0001672
    ## race_ethnicityBlack or African American:insuranceMedicaid    0.0322196
    ## race_ethnicityHispanic:insuranceMedicaid                     0.0263531
    ## race_ethnicityBlack or African American:insuranceMedicare    0.0171868
    ## race_ethnicityHispanic:insuranceMedicare                     0.0164391
    ##                                                             t value
    ## (Intercept)                                                  11.766
    ## race_ethnicityBlack or African American                       1.940
    ## race_ethnicityHispanic                                       10.950
    ## insuranceMedicaid                                             7.396
    ## insuranceMedicare                                             3.243
    ## genderMale                                                   -4.505
    ## genderUnknown                                                -0.640
    ## first_dr_age                                                 -9.049
    ## smoke_statusFormer / No longer active / Past History / Quit   0.839
    ## smoke_statusUnknown / Unclassified                            1.018
    ## smoke_statusYes / Active                                      4.051
    ## baseline_va_letter                                            6.288
    ## severity_score                                               20.711
    ## race_ethnicityBlack or African American:insuranceMedicaid     1.320
    ## race_ethnicityHispanic:insuranceMedicaid                      0.798
    ## race_ethnicityBlack or African American:insuranceMedicare     2.548
    ## race_ethnicityHispanic:insuranceMedicare                      1.785
    ##                                                             Pr(>|t|)    
    ## (Intercept)                                                  < 2e-16 ***
    ## race_ethnicityBlack or African American                      0.05234 .  
    ## race_ethnicityHispanic                                       < 2e-16 ***
    ## insuranceMedicaid                                           1.43e-13 ***
    ## insuranceMedicare                                            0.00119 ** 
    ## genderMale                                                  6.65e-06 ***
    ## genderUnknown                                                0.52237    
    ## first_dr_age                                                 < 2e-16 ***
    ## smoke_statusFormer / No longer active / Past History / Quit  0.40152    
    ## smoke_statusUnknown / Unclassified                           0.30862    
    ## smoke_statusYes / Active                                    5.12e-05 ***
    ## baseline_va_letter                                          3.24e-10 ***
    ## severity_score                                               < 2e-16 ***
    ## race_ethnicityBlack or African American:insuranceMedicaid    0.18684    
    ## race_ethnicityHispanic:insuranceMedicaid                     0.42501    
    ## race_ethnicityBlack or African American:insuranceMedicare    0.01083 *  
    ## race_ethnicityHispanic:insuranceMedicare                     0.07422 .  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.4896 on 39891 degrees of freedom
    ## Multiple R-squared:  0.04078,    Adjusted R-squared:  0.0404 
    ## F-statistic:   106 on 16 and 39891 DF,  p-value: < 2.2e-16

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
  )
  
loss_15_race %>%
  ggplot(aes(x = time_diff, y = pct_15pt_loss, fill = race_ethnicity)) +
  geom_col(position = "dodge") +
  geom_errorbar(aes(ymin = lower_bound, ymax = upper_bound), position = position_dodge(width = .9), width = .6) +
  theme_light() +
  scale_y_continuous(labels = scales::label_percent(accuracy = 1)) +
  labs(
    x = "Time after baseline VA measurement",
    y = "Percentage of eyes that lost 15 or more pts of VA",
    fill = "Race"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->

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
  # ggrepel::geom_text_repel(
  #   data = va_race_diff %>% filter(time_diff == "two_year_change"),
  #   aes(label = count)
  # ) +
  theme_light() + 
  labs(
    x = "Time point",
    y = "Change in visual acuity",
    color = "Race"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->

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
  )
  
loss_15_insurance %>% 
  ggplot(aes(x = time_diff, y = pct_15pt_loss, fill = insurance)) +
  geom_col(position = "dodge") +
  geom_errorbar(aes(ymin = lower_bound, ymax = upper_bound), position = position_dodge(width = .9), width = .6) +
  theme_light() +
  scale_y_continuous(labels = scales::label_percent(accuracy = 1)) +
  scale_fill_viridis_d() +
  labs(
    x = "Time after baseline VA measurement",
    y = "Percentage of eyes that lost 15 or more points of VA",
    fill = "Insurance"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-21-1.png)<!-- -->

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
    ##    race_ethnicity            baseline_va_letter avg_severity count
    ##    <chr>                                  <dbl>        <dbl> <int>
    ##  1 Hispanic                                  40         65.6   140
    ##  2 Hispanic                                  35         64.1   373
    ##  3 Hispanic                                  50         64.0   348
    ##  4 Hispanic                                  55         63.9   355
    ##  5 Hispanic                                  61         63.3   616
    ##  6 Hispanic                                  85         62.5   374
    ##  7 Caucasian                                 45         62.4   161
    ##  8 Hispanic                                  65         62.3   847
    ##  9 Black or African American                 35         62.2   317
    ## 10 Hispanic                                  58         62.1   381
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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-23-1.png)<!-- -->

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
  ungroup()

va_insurance_diff %>% 
  ggplot(aes(x = time_diff, y = va_change, color = insurance)) +
  geom_point() +
  geom_line(aes(group = insurance)) +
  # ggrepel::geom_text_repel(
  #   data = va_insurance_diff %>% filter(time_diff == "two_year_change"),
  #   aes(label = count)
  # ) +
  theme_light() + 
  scale_color_viridis_d() +
  labs(
    x = "Time point",
    y = "Change in visual acuity",
    color = "Insurance"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-24-1.png)<!-- -->

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
         race_ethnicity = factor(race_ethnicity),
         race_ethnicity = relevel(race_ethnicity, ref = "Caucasian"),
         insurance = factor(insurance), 
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
    ##     mutate(race_ethnicity = factor(race_ethnicity), race_ethnicity = relevel(race_ethnicity, 
    ##         ref = "Caucasian"), insurance = factor(insurance), insurance = relevel(insurance, 
    ##         ref = "Private"), smoke_status = factor(smoke_status), 
    ##         smoke_status = relevel(smoke_status, ref = "No / Never")))
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -96.894  -4.372   1.666   6.483  42.577 
    ## 
    ## Coefficients:
    ##                                                              Estimate
    ## (Intercept)                                                 36.411416
    ## race_ethnicityBlack or African American                     -1.456907
    ## race_ethnicityHispanic                                      -1.316521
    ## insuranceMedicaid                                           -2.128057
    ## insuranceMedicare                                           -1.780485
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
    ## race_ethnicityBlack or African American:insuranceMedicaid   -0.067253
    ## race_ethnicityHispanic:insuranceMedicaid                     0.602269
    ## race_ethnicityBlack or African American:insuranceMedicare    1.348333
    ## race_ethnicityHispanic:insuranceMedicare                    -0.078540
    ##                                                             Std. Error
    ## (Intercept)                                                   0.634959
    ## race_ethnicityBlack or African American                       0.368723
    ## race_ethnicityHispanic                                        0.338773
    ## insuranceMedicaid                                             0.355968
    ## insuranceMedicare                                             0.179176
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
    ## race_ethnicityBlack or African American:insuranceMedicaid     0.801029
    ## race_ethnicityHispanic:insuranceMedicaid                      0.655178
    ## race_ethnicityBlack or African American:insuranceMedicare     0.427327
    ## race_ethnicityHispanic:insuranceMedicare                      0.408718
    ##                                                             t value
    ## (Intercept)                                                  57.345
    ## race_ethnicityBlack or African American                      -3.951
    ## race_ethnicityHispanic                                       -3.886
    ## insuranceMedicaid                                            -5.978
    ## insuranceMedicare                                            -9.937
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
    ## race_ethnicityBlack or African American:insuranceMedicaid    -0.084
    ## race_ethnicityHispanic:insuranceMedicaid                      0.919
    ## race_ethnicityBlack or African American:insuranceMedicare     3.155
    ## race_ethnicityHispanic:insuranceMedicare                     -0.192
    ##                                                             Pr(>|t|)    
    ## (Intercept)                                                  < 2e-16 ***
    ## race_ethnicityBlack or African American                     7.79e-05 ***
    ## race_ethnicityHispanic                                      0.000102 ***
    ## insuranceMedicaid                                           2.27e-09 ***
    ## insuranceMedicare                                            < 2e-16 ***
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
    ## race_ethnicityBlack or African American:insuranceMedicaid   0.933090    
    ## race_ethnicityHispanic:insuranceMedicaid                    0.357973    
    ## race_ethnicityBlack or African American:insuranceMedicare   0.001605 ** 
    ## race_ethnicityHispanic:insuranceMedicare                    0.847617    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 12.17 on 39888 degrees of freedom
    ## Multiple R-squared:  0.1459, Adjusted R-squared:  0.1455 
    ## F-statistic: 358.6 on 19 and 39888 DF,  p-value: < 2.2e-16

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
    ##    race_ethnicity            baseline_va_r5 avg_severity count
    ##    <chr>                              <dbl>        <dbl> <int>
    ##  1 Hispanic                              40         65.9   144
    ##  2 Hispanic                              35         64.1   373
    ##  3 Hispanic                              50         64.0   359
    ##  4 Hispanic                              55         63.8   361
    ##  5 Hispanic                              60         62.9  1024
    ##  6 Caucasian                             25         62.6   147
    ##  7 Hispanic                              85         62.5   374
    ##  8 Caucasian                             45         62.3   175
    ##  9 Black or African American             35         62.2   317
    ## 10 Hispanic                              65         62.2   866
    ## # … with 21 more rows

``` r
# graph severity by race and age group

severity_race_vision %>%
  filter(race_ethnicity %in% c("Caucasian", "Black or African American", "Hispanic")) %>% 
  ggplot(aes(x = baseline_va_r5, y = avg_severity, color = race_ethnicity)) +
  geom_line() +
  geom_point() +
  theme_light() +
  labs(
    title = "Severity by race and baseline VA", 
    y = "Average severity score", 
    x = "Baseline VA", 
    color = "Race"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-27-1.png)<!-- -->

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
    ##    <chr>                    <dbl>        <dbl> <int>
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

severity_insurance_vision %>%
  filter(insurance %in% c("Medicaid", "Medicare", "Private")) %>% 
  ggplot(aes(x = baseline_va_r5, y = avg_severity, color = insurance)) +
  geom_line() +
  geom_point() +
  theme_light() +
  labs(
    title = "Severity by insurance and baseline VA", 
    y = "Average severity score", 
    x = "Baseline VA", 
    color = "Insurance"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-29-1.png)<!-- -->

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
    ##   race_ethnicity            total_race
    ##   <chr>                          <int>
    ## 1 Black or African American       5922
    ## 2 Caucasian                      30989
    ## 3 Hispanic                        6363

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-31-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-32-1.png)<!-- -->

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
    ##    severity_score race_ethnicity                n   prop
    ##             <dbl> <chr>                     <int>  <dbl>
    ##  1             35 Black or African American   596 0.101 
    ##  2             35 Caucasian                  3349 0.108 
    ##  3             35 Hispanic                    442 0.0695
    ##  4             40 Black or African American   323 0.0545
    ##  5             40 Caucasian                  1887 0.0609
    ##  6             40 Hispanic                    226 0.0355
    ##  7             43 Black or African American   148 0.0250
    ##  8             43 Caucasian                   840 0.0271
    ##  9             43 Hispanic                    141 0.0222
    ## 10             44 Black or African American   817 0.138 
    ## # … with 23 more rows

``` r
## density chart of distribution of severity scores, by race
timeseries_analysis %>% 
  ggplot() +
  geom_density(aes(x = severity_score, group = race_ethnicity, color = race_ethnicity), alpha = .2) +
  theme_light()
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-35-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-36-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-37-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-38-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-39-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-40-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-41-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-42-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-43-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-43-2.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-44-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-44-2.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-48-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-50-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-53-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-54-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-55-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-56-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-57-1.png)<!-- -->

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
    ##              <dbl>          <dbl> <chr>          <int> <chr>         <dbl>
    ##  1              80             73 Caucasian       2863 baseline_va    79.3
    ##  2              80             73 Caucasian       2863 one_year_va    76.2
    ##  3              80             73 Caucasian       2863 two_year_va    74.8
    ##  4              60             73 Caucasian       2648 baseline_va    61.1
    ##  5              60             73 Caucasian       2648 one_year_va    64.0
    ##  6              60             73 Caucasian       2648 two_year_va    64.1
    ##  7              80             44 Caucasian       1729 baseline_va    78.9
    ##  8              80             44 Caucasian       1729 one_year_va    76.9
    ##  9              80             44 Caucasian       1729 two_year_va    75.4
    ## 10              70             73 Caucasian       1532 baseline_va    70.0
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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-59-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-61-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-62-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-63-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-65-1.png)<!-- -->

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

| race_ethnicity            |  average | count |
|:--------------------------|---------:|------:|
| Hispanic                  | 62.39353 |  6363 |
| Black or African American | 58.56974 |  5922 |
| Caucasian                 | 57.03453 | 30989 |

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
    ##    race_ethnicity            region    average count
    ##    <chr>                     <chr>       <dbl> <int>
    ##  1 Hispanic                  <NA>         68.3    98
    ##  2 Hispanic                  Midwest      63.7   491
    ##  3 Hispanic                  West         63.1  2269
    ##  4 Hispanic                  South        62.3  2633
    ##  5 Hispanic                  Northeast    59.6   872
    ##  6 Black or African American South        58.8  3409
    ##  7 Black or African American Northeast    58.4  1125
    ##  8 Black or African American Midwest      58.1   981
    ##  9 Black or African American West         58.0   339
    ## 10 Caucasian                 South        57.8 10792
    ## 11 Caucasian                 West         57.3  5058
    ## 12 Black or African American <NA>         56.8    68
    ## 13 Caucasian                 Midwest      56.5  8652
    ## 14 Caucasian                 <NA>         56.4   334
    ## 15 Caucasian                 Northeast    56.3  6153

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-72-1.png)<!-- -->

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

| race_ethnicity            | age_group | avg_severity | count |
|:--------------------------|----------:|-------------:|------:|
| Black or African American |        25 |     72.25926 |    27 |
| Hispanic                  |        35 |     68.89744 |   156 |
| Caucasian                 |        35 |     68.50963 |   571 |
| Caucasian                 |        20 |     68.33333 |    33 |
| Hispanic                  |        45 |     68.32414 |   435 |
| Hispanic                  |        25 |     68.27273 |    33 |
| Hispanic                  |        40 |     68.19355 |   279 |
| Black or African American |        20 |     68.00000 |     3 |
| Hispanic                  |        20 |     68.00000 |     7 |
| Caucasian                 |        30 |     67.95183 |   436 |

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-74-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-75-1.png)<!-- -->

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
    ##    race_ethnicity            insurance       average count
    ##    <chr>                     <chr>             <dbl> <int>
    ##  1 Hispanic                  Govt               65.2   123
    ##  2 Hispanic                  Unknown/Missing    63.3   640
    ##  3 Hispanic                  Medicaid           63.2   712
    ##  4 Hispanic                  Private            62.5  1553
    ##  5 Caucasian                 Medicaid           61.9  1392
    ##  6 Hispanic                  Medicare           61.9  3305
    ##  7 Black or African American Medicaid           61.6   371
    ##  8 Caucasian                 Unknown/Missing    60.5   794
    ##  9 Black or African American Unknown/Missing    60.1   229
    ## 10 Hispanic                  Military           59.3    30
    ## 11 Caucasian                 Govt               59.2   862
    ## 12 Black or African American Private            59.0  1266
    ## 13 Caucasian                 Private            58.9  7955
    ## 14 Black or African American Govt               58.7   180
    ## 15 Black or African American Medicare           58.0  3803
    ## 16 Black or African American Military           57.3    73
    ## 17 Caucasian                 Military           55.9   435
    ## 18 Caucasian                 Medicare           55.7 19551

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-78-1.png)<!-- -->

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

| race_ethnicity            | gender  |  average | count |
|:--------------------------|:--------|---------:|------:|
| Hispanic                  | Unknown | 64.34783 |    46 |
| Hispanic                  | Male    | 63.22544 |  3389 |
| Hispanic                  | Female  | 61.39993 |  2928 |
| Caucasian                 | Unknown | 59.69298 |   114 |
| Black or African American | Male    | 59.69193 |  2441 |

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-80-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-82-1.png)<!-- -->

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
    ##    severity_score race_ethnicity            proc_group_28  prop count
    ##             <dbl> <chr>                     <chr>         <dbl> <int>
    ##  1             85 Black or African American antivegf          1    13
    ##  2             85 Caucasian                 antivegf          1    16
    ##  3             85 Hispanic                  antivegf          1     9
    ##  4             81 Black or African American antivegf          1    16
    ##  5             81 Caucasian                 antivegf          1    34
    ##  6             81 Hispanic                  antivegf          1    11
    ##  7             78 Black or African American antivegf          1   863
    ##  8             78 Caucasian                 antivegf          1  4075
    ##  9             78 Hispanic                  antivegf          1  1276
    ## 10             73 Black or African American antivegf          1  1795
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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-84-1.png)<!-- -->
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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-85-1.png)<!-- -->

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
    ##    severity_score race_ethnicity            vegf_group_365   prop count
    ##             <dbl> <chr>                     <chr>           <dbl> <int>
    ##  1             85 Black or African American Avastin        0.8        8
    ##  2             85 Black or African American combo          0.2        2
    ##  3             85 Caucasian                 Avastin        0.6        9
    ##  4             85 Caucasian                 combo          0.2        3
    ##  5             85 Caucasian                 Eylea          0.133      2
    ##  6             85 Caucasian                 Lucentis       0.0667     1
    ##  7             85 Hispanic                  Avastin        0.333      3
    ##  8             85 Hispanic                  combo          0.333      3
    ##  9             85 Hispanic                  Eylea          0.333      3
    ## 10             81 Black or African American Avastin        0.533      8
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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-87-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-88-1.png)<!-- -->

Caucasian people are 5-10% more likely to receive Eylea treatment
(first) as compared to Hispanic people at the same level of severity,
and 2-5% more likely to receive Eylea treatment (first) as compared to
Black people at the same level of severity.

``` r
# Graphing likelihood of Avastin as first treatment by severity and race
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
    title = "Likelihood of receiving Avastin at higher severity score by race",
    y = "Percentage of people receiving Avastin"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-89-1.png)<!-- -->

At lower levels of severity, Hispanic people are far more likely
(15-25%) to receive Avastin as compared to their Caucasian counterparts
with the same level of severity. Black and Asian people seem somewhat
more likely to receive Avastin, but there is some variation.

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
    ##    vision_category    race_ethnicity            vegf_group_365   prop count
    ##    <chr>              <chr>                     <chr>           <dbl> <int>
    ##  1 8 - PDR with edema Black or African American Avastin        0.588    446
    ##  2 8 - PDR with edema Black or African American combo          0.182    138
    ##  3 8 - PDR with edema Black or African American Eylea          0.146    111
    ##  4 8 - PDR with edema Black or African American Lucentis       0.0831    63
    ##  5 8 - PDR with edema Caucasian                 Avastin        0.506   1848
    ##  6 8 - PDR with edema Caucasian                 combo          0.191    699
    ##  7 8 - PDR with edema Caucasian                 Eylea          0.209    762
    ##  8 8 - PDR with edema Caucasian                 Lucentis       0.0939   343
    ##  9 8 - PDR with edema Hispanic                  Avastin        0.678    729
    ## 10 8 - PDR with edema Hispanic                  combo          0.156    168
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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-91-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-92-1.png)<!-- -->

``` r
# Graphing likelihood of Avastin as first treatment by severity and race
drug_sev_cat_race %>% 
  filter(!is.na(vision_category)) %>% 
  filter(vegf_group_365 == "Avastin") %>% 
  ggplot(aes(x = vision_category, y = prop, color = race_ethnicity)) +
  geom_point() + 
  geom_line() +
  theme_bw() +
  scale_y_continuous(labels = scales::percent) +
  labs(
    title = "Likelihood of receiving Avastin at higher vision_category by race",
    y = "Percentage of people receiving Avastin"
  )
```

    ## geom_path: Each group consists of only one observation. Do you need to
    ## adjust the group aesthetic?

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-93-1.png)<!-- -->

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
    ##    baseline_va_r10 race_ethnicity            vegf_group_365   prop count
    ##              <dbl> <chr>                     <chr>           <dbl> <int>
    ##  1             100 Black or African American Eylea          1          2
    ##  2             100 Hispanic                  Avastin        1          1
    ##  3              90 Black or African American Avastin        0.4        2
    ##  4              90 Black or African American combo          0.2        1
    ##  5              90 Black or African American Eylea          0.2        1
    ##  6              90 Black or African American Lucentis       0.2        1
    ##  7              90 Caucasian                 Avastin        0.452     19
    ##  8              90 Caucasian                 combo          0.0952     4
    ##  9              90 Caucasian                 Eylea          0.238     10
    ## 10              90 Caucasian                 Lucentis       0.214      9
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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-95-1.png)<!-- -->

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
    ##    <chr>              <chr>     <chr>           <dbl> <int>
    ##  1 8 - PDR with edema Medicaid  Avastin        0.680    329
    ##  2 8 - PDR with edema Medicaid  combo          0.151     73
    ##  3 8 - PDR with edema Medicaid  Eylea          0.118     57
    ##  4 8 - PDR with edema Medicaid  Lucentis       0.0517    25
    ##  5 8 - PDR with edema Medicare  Avastin        0.528   1561
    ##  6 8 - PDR with edema Medicare  combo          0.185    547
    ##  7 8 - PDR with edema Medicare  Eylea          0.197    583
    ##  8 8 - PDR with edema Medicare  Lucentis       0.0890   263
    ##  9 8 - PDR with edema Private   Avastin        0.535    824
    ## 10 8 - PDR with edema Private   combo          0.192    296
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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-97-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-98-1.png)<!-- -->

``` r
# Graphing likelihood of Avastin as first treatment by severity and insurance
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
    title = "Likelihood of receiving Avastin at higher vision_category by race",
    y = "Percentage of people receiving Avastin"
  )
```

    ## geom_path: Each group consists of only one observation. Do you need to
    ## adjust the group aesthetic?

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-99-1.png)<!-- -->

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
    ##    severity_score race_ethnicity      insurance   vegf_group_28  prop count
    ##             <dbl> <chr>               <chr>       <chr>         <dbl> <int>
    ##  1             85 Black or African A… Medicare    Avastin       1         6
    ##  2             85 Black or African A… Private     Avastin       1         6
    ##  3             85 Black or African A… Medicaid    Lucentis      1         1
    ##  4             85 Caucasian           Medicaid    Avastin       1         1
    ##  5             85 Caucasian           Medicare    Avastin       0.571     4
    ##  6             85 Caucasian           Private     Avastin       1         7
    ##  7             85 Caucasian           Unknown/Mi… Avastin       1         1
    ##  8             85 Caucasian           Medicare    Eylea         0.286     2
    ##  9             85 Caucasian           Medicare    Lucentis      0.143     1
    ## 10             85 Hispanic            Medicare    Avastin       0.6       3
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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-101-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-102-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-103-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-105-1.png)<!-- -->

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
    ##    race_ethnicity            baseline_va_letter avg_severity count
    ##    <chr>                                  <dbl>        <dbl> <int>
    ##  1 Hispanic                                  40         65.6   140
    ##  2 Hispanic                                  35         64.1   373
    ##  3 Hispanic                                  50         64.0   348
    ##  4 Hispanic                                  55         63.9   355
    ##  5 Hispanic                                  61         63.3   616
    ##  6 Hispanic                                  85         62.5   374
    ##  7 Caucasian                                 45         62.4   161
    ##  8 Hispanic                                  65         62.3   847
    ##  9 Black or African American                 35         62.2   317
    ## 10 Hispanic                                  58         62.1   381
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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-107-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-108-1.png)<!-- -->

## Regression

#### Linear regression

Start with a linear regression to see what variables are initially
predictive

``` r
# basic linear regression
one_year_va_delta_1.lm <- 
  lm(
    one_year_va_delta ~ relevel(race_ethnicity, ref = "Caucasian") + insurance + gender + first_dr_age + smoke_status + baseline_va_letter, 
    data = timeseries_analysis %>% mutate(race_ethnicity = factor(race_ethnicity))
    )

summary(one_year_va_delta_1.lm)
```

    ## 
    ## Call:
    ## lm(formula = one_year_va_delta ~ relevel(race_ethnicity, ref = "Caucasian") + 
    ##     insurance + gender + first_dr_age + smoke_status + baseline_va_letter, 
    ##     data = timeseries_analysis %>% mutate(race_ethnicity = factor(race_ethnicity)))
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -98.372  -4.407   1.660   6.413  43.423 
    ## 
    ## Coefficients:
    ##                                                                      Estimate
    ## (Intercept)                                                         30.978618
    ## relevel(race_ethnicity, ref = "Caucasian")Black or African American -0.720649
    ## relevel(race_ethnicity, ref = "Caucasian")Hispanic                  -1.520637
    ## insuranceMedicaid                                                   -2.000904
    ## insuranceMedicare                                                   -1.593007
    ## insuranceMilitary                                                   -1.292300
    ## insurancePrivate                                                     0.137909
    ## insuranceUnknown/Missing                                            -0.488881
    ## genderMale                                                           0.358964
    ## genderUnknown                                                       -1.832205
    ## first_dr_age                                                        -0.031519
    ## smoke_statusNo / Never                                              -0.185306
    ## smoke_statusUnknown / Unclassified                                   0.111446
    ## smoke_statusYes / Active                                            -0.825105
    ## baseline_va_letter                                                  -0.389425
    ##                                                                     Std. Error
    ## (Intercept)                                                           0.601645
    ## relevel(race_ethnicity, ref = "Caucasian")Black or African American   0.173870
    ## relevel(race_ethnicity, ref = "Caucasian")Hispanic                    0.171204
    ## insuranceMedicaid                                                     0.434615
    ## insuranceMedicare                                                     0.371103
    ## insuranceMilitary                                                     0.637384
    ## insurancePrivate                                                      0.375842
    ## insuranceUnknown/Missing                                              0.468436
    ## genderMale                                                            0.120059
    ## genderUnknown                                                         0.897739
    ## first_dr_age                                                          0.005833
    ## smoke_statusNo / Never                                                0.138248
    ## smoke_statusUnknown / Unclassified                                    0.568850
    ## smoke_statusYes / Active                                              0.213192
    ## baseline_va_letter                                                    0.004617
    ##                                                                     t value
    ## (Intercept)                                                          51.490
    ## relevel(race_ethnicity, ref = "Caucasian")Black or African American  -4.145
    ## relevel(race_ethnicity, ref = "Caucasian")Hispanic                   -8.882
    ## insuranceMedicaid                                                    -4.604
    ## insuranceMedicare                                                    -4.293
    ## insuranceMilitary                                                    -2.028
    ## insurancePrivate                                                      0.367
    ## insuranceUnknown/Missing                                             -1.044
    ## genderMale                                                            2.990
    ## genderUnknown                                                        -2.041
    ## first_dr_age                                                         -5.404
    ## smoke_statusNo / Never                                               -1.340
    ## smoke_statusUnknown / Unclassified                                    0.196
    ## smoke_statusYes / Active                                             -3.870
    ## baseline_va_letter                                                  -84.339
    ##                                                                     Pr(>|t|)
    ## (Intercept)                                                          < 2e-16
    ## relevel(race_ethnicity, ref = "Caucasian")Black or African American 3.41e-05
    ## relevel(race_ethnicity, ref = "Caucasian")Hispanic                   < 2e-16
    ## insuranceMedicaid                                                   4.16e-06
    ## insuranceMedicare                                                   1.77e-05
    ## insuranceMilitary                                                   0.042617
    ## insurancePrivate                                                    0.713670
    ## insuranceUnknown/Missing                                            0.296655
    ## genderMale                                                          0.002792
    ## genderUnknown                                                       0.041266
    ## first_dr_age                                                        6.56e-08
    ## smoke_statusNo / Never                                              0.180124
    ## smoke_statusUnknown / Unclassified                                  0.844678
    ## smoke_statusYes / Active                                            0.000109
    ## baseline_va_letter                                                   < 2e-16
    ##                                                                        
    ## (Intercept)                                                         ***
    ## relevel(race_ethnicity, ref = "Caucasian")Black or African American ***
    ## relevel(race_ethnicity, ref = "Caucasian")Hispanic                  ***
    ## insuranceMedicaid                                                   ***
    ## insuranceMedicare                                                   ***
    ## insuranceMilitary                                                   *  
    ## insurancePrivate                                                       
    ## insuranceUnknown/Missing                                               
    ## genderMale                                                          ** 
    ## genderUnknown                                                       *  
    ## first_dr_age                                                        ***
    ## smoke_statusNo / Never                                                 
    ## smoke_statusUnknown / Unclassified                                     
    ## smoke_statusYes / Active                                            ***
    ## baseline_va_letter                                                  ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 12.18 on 43259 degrees of freedom
    ## Multiple R-squared:  0.1428, Adjusted R-squared:  0.1425 
    ## F-statistic: 514.5 on 14 and 43259 DF,  p-value: < 2.2e-16

``` r
# linear regression with insurance race interaction
one_year_va_delta_2.lm <- 
  lm(
    one_year_va_delta ~ relevel(race_ethnicity, ref = "Caucasian") + insurance + relevel(race_ethnicity, ref = "Caucasian") * insurance + gender + first_dr_age + smoke_status + baseline_va_letter, 
    data = timeseries_analysis %>% 
      filter(insurance %in% c("Medicare", "Medicaid", "Private")) %>% 
      mutate(race_ethnicity = factor(race_ethnicity))
  )

summary(one_year_va_delta_2.lm)
```

    ## 
    ## Call:
    ## lm(formula = one_year_va_delta ~ relevel(race_ethnicity, ref = "Caucasian") + 
    ##     insurance + relevel(race_ethnicity, ref = "Caucasian") * 
    ##     insurance + gender + first_dr_age + smoke_status + baseline_va_letter, 
    ##     data = timeseries_analysis %>% filter(insurance %in% c("Medicare", 
    ##         "Medicaid", "Private")) %>% mutate(race_ethnicity = factor(race_ethnicity)))
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -98.378  -4.366   1.701   6.384  43.829 
    ## 
    ## Coefficients:
    ##                                                                                        Estimate
    ## (Intercept)                                                                           28.663963
    ## relevel(race_ethnicity, ref = "Caucasian")Black or African American                   -1.589816
    ## relevel(race_ethnicity, ref = "Caucasian")Hispanic                                    -0.947951
    ## insuranceMedicare                                                                      0.386078
    ## insurancePrivate                                                                       2.265171
    ## genderMale                                                                             0.354700
    ## genderUnknown                                                                         -1.359469
    ## first_dr_age                                                                          -0.030101
    ## smoke_statusNo / Never                                                                -0.160607
    ## smoke_statusUnknown / Unclassified                                                     0.326253
    ## smoke_statusYes / Active                                                              -0.864808
    ## baseline_va_letter                                                                    -0.386455
    ## relevel(race_ethnicity, ref = "Caucasian")Black or African American:insuranceMedicare  1.336430
    ## relevel(race_ethnicity, ref = "Caucasian")Hispanic:insuranceMedicare                  -0.833595
    ## relevel(race_ethnicity, ref = "Caucasian")Black or African American:insurancePrivate   0.076244
    ## relevel(race_ethnicity, ref = "Caucasian")Hispanic:insurancePrivate                   -0.679177
    ##                                                                                       Std. Error
    ## (Intercept)                                                                             0.581578
    ## relevel(race_ethnicity, ref = "Caucasian")Black or African American                     0.714091
    ## relevel(race_ethnicity, ref = "Caucasian")Hispanic                                      0.563650
    ## insuranceMedicare                                                                       0.353305
    ## insurancePrivate                                                                        0.356792
    ## genderMale                                                                              0.125189
    ## genderUnknown                                                                           0.933057
    ## first_dr_age                                                                            0.006139
    ## smoke_statusNo / Never                                                                  0.143800
    ## smoke_statusUnknown / Unclassified                                                      0.610762
    ## smoke_statusYes / Active                                                                0.221782
    ## baseline_va_letter                                                                      0.004828
    ## relevel(race_ethnicity, ref = "Caucasian")Black or African American:insuranceMedicare   0.746228
    ## relevel(race_ethnicity, ref = "Caucasian")Hispanic:insuranceMedicare                    0.609335
    ## relevel(race_ethnicity, ref = "Caucasian")Black or African American:insurancePrivate    0.803628
    ## relevel(race_ethnicity, ref = "Caucasian")Hispanic:insurancePrivate                     0.657290
    ##                                                                                       t value
    ## (Intercept)                                                                            49.287
    ## relevel(race_ethnicity, ref = "Caucasian")Black or African American                    -2.226
    ## relevel(race_ethnicity, ref = "Caucasian")Hispanic                                     -1.682
    ## insuranceMedicare                                                                       1.093
    ## insurancePrivate                                                                        6.349
    ## genderMale                                                                              2.833
    ## genderUnknown                                                                          -1.457
    ## first_dr_age                                                                           -4.903
    ## smoke_statusNo / Never                                                                 -1.117
    ## smoke_statusUnknown / Unclassified                                                      0.534
    ## smoke_statusYes / Active                                                               -3.899
    ## baseline_va_letter                                                                    -80.046
    ## relevel(race_ethnicity, ref = "Caucasian")Black or African American:insuranceMedicare   1.791
    ## relevel(race_ethnicity, ref = "Caucasian")Hispanic:insuranceMedicare                   -1.368
    ## relevel(race_ethnicity, ref = "Caucasian")Black or African American:insurancePrivate    0.095
    ## relevel(race_ethnicity, ref = "Caucasian")Hispanic:insurancePrivate                    -1.033
    ##                                                                                       Pr(>|t|)
    ## (Intercept)                                                                            < 2e-16
    ## relevel(race_ethnicity, ref = "Caucasian")Black or African American                    0.02600
    ## relevel(race_ethnicity, ref = "Caucasian")Hispanic                                     0.09261
    ## insuranceMedicare                                                                      0.27451
    ## insurancePrivate                                                                      2.19e-10
    ## genderMale                                                                             0.00461
    ## genderUnknown                                                                          0.14512
    ## first_dr_age                                                                          9.46e-07
    ## smoke_statusNo / Never                                                                 0.26405
    ## smoke_statusUnknown / Unclassified                                                     0.59322
    ## smoke_statusYes / Active                                                              9.66e-05
    ## baseline_va_letter                                                                     < 2e-16
    ## relevel(race_ethnicity, ref = "Caucasian")Black or African American:insuranceMedicare  0.07331
    ## relevel(race_ethnicity, ref = "Caucasian")Hispanic:insuranceMedicare                   0.17131
    ## relevel(race_ethnicity, ref = "Caucasian")Black or African American:insurancePrivate   0.92441
    ## relevel(race_ethnicity, ref = "Caucasian")Hispanic:insurancePrivate                    0.30147
    ##                                                                                          
    ## (Intercept)                                                                           ***
    ## relevel(race_ethnicity, ref = "Caucasian")Black or African American                   *  
    ## relevel(race_ethnicity, ref = "Caucasian")Hispanic                                    .  
    ## insuranceMedicare                                                                        
    ## insurancePrivate                                                                      ***
    ## genderMale                                                                            ** 
    ## genderUnknown                                                                            
    ## first_dr_age                                                                          ***
    ## smoke_statusNo / Never                                                                   
    ## smoke_statusUnknown / Unclassified                                                       
    ## smoke_statusYes / Active                                                              ***
    ## baseline_va_letter                                                                    ***
    ## relevel(race_ethnicity, ref = "Caucasian")Black or African American:insuranceMedicare .  
    ## relevel(race_ethnicity, ref = "Caucasian")Hispanic:insuranceMedicare                     
    ## relevel(race_ethnicity, ref = "Caucasian")Black or African American:insurancePrivate     
    ## relevel(race_ethnicity, ref = "Caucasian")Hispanic:insurancePrivate                      
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 12.21 on 39892 degrees of freedom
    ## Multiple R-squared:  0.1402, Adjusted R-squared:  0.1399 
    ## F-statistic: 433.6 on 15 and 39892 DF,  p-value: < 2.2e-16

``` r
# linear regression with severity score
one_year_va_delta_3.lm <- 
  lm(one_year_va_delta ~ race_ethnicity + insurance + gender + first_dr_age + smoke_status + baseline_va_letter + severity_score, 
     data = timeseries_analysis)

summary(one_year_va_delta_3.lm)
```

    ## 
    ## Call:
    ## lm(formula = one_year_va_delta ~ race_ethnicity + insurance + 
    ##     gender + first_dr_age + smoke_status + baseline_va_letter + 
    ##     severity_score, data = timeseries_analysis)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -97.096  -4.373   1.688   6.458  42.065 
    ## 
    ## Coefficients:
    ##                                     Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)                        36.153163   0.712027  50.775  < 2e-16
    ## race_ethnicityCaucasian             0.640607   0.173422   3.694 0.000221
    ## race_ethnicityHispanic             -0.626518   0.221681  -2.826 0.004712
    ## insuranceMedicaid                  -1.977758   0.433320  -4.564 5.03e-06
    ## insuranceMedicare                  -1.497899   0.370042  -4.048 5.18e-05
    ## insuranceMilitary                  -1.310894   0.635482  -2.063 0.039134
    ## insurancePrivate                    0.115201   0.374722   0.307 0.758517
    ## insuranceUnknown/Missing           -0.386531   0.467080  -0.828 0.407932
    ## genderMale                          0.406688   0.119737   3.396 0.000683
    ## genderUnknown                      -1.695073   0.895100  -1.894 0.058268
    ## first_dr_age                       -0.058077   0.006044  -9.609  < 2e-16
    ## smoke_statusNo / Never             -0.153169   0.137849  -1.111 0.266517
    ## smoke_statusUnknown / Unclassified  0.219453   0.567191   0.387 0.698823
    ## smoke_statusYes / Active           -0.763172   0.212590  -3.590 0.000331
    ## baseline_va_letter                 -0.398650   0.004639 -85.935  < 2e-16
    ## severity_score                     -0.064347   0.003987 -16.138  < 2e-16
    ##                                       
    ## (Intercept)                        ***
    ## race_ethnicityCaucasian            ***
    ## race_ethnicityHispanic             ** 
    ## insuranceMedicaid                  ***
    ## insuranceMedicare                  ***
    ## insuranceMilitary                  *  
    ## insurancePrivate                      
    ## insuranceUnknown/Missing              
    ## genderMale                         ***
    ## genderUnknown                      .  
    ## first_dr_age                       ***
    ## smoke_statusNo / Never                
    ## smoke_statusUnknown / Unclassified    
    ## smoke_statusYes / Active           ***
    ## baseline_va_letter                 ***
    ## severity_score                     ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 12.15 on 43258 degrees of freedom
    ## Multiple R-squared:  0.1479, Adjusted R-squared:  0.1476 
    ## F-statistic: 500.5 on 15 and 43258 DF,  p-value: < 2.2e-16

``` r
# linear regression with race insurance interaction and severity score
one_year_va_delta_4.lm <- 
  lm(one_year_va_delta ~ race_ethnicity + insurance + race_ethnicity * insurance + gender + first_dr_age + smoke_status + baseline_va_letter + severity_score, 
     data = timeseries_analysis %>% 
       filter(insurance %in% c("Medicare", "Medicaid", "Private")) %>% 
       mutate(
         race_ethnicity = factor(race_ethnicity),
         race_ethnicity = relevel(race_ethnicity, ref = "Caucasian"),
         insurance = factor(insurance), 
         insurance = relevel(insurance, ref = "Private")
       )
  ) 

summary(one_year_va_delta_4.lm)
```

    ## 
    ## Call:
    ## lm(formula = one_year_va_delta ~ race_ethnicity + insurance + 
    ##     race_ethnicity * insurance + gender + first_dr_age + smoke_status + 
    ##     baseline_va_letter + severity_score, data = timeseries_analysis %>% 
    ##     filter(insurance %in% c("Medicare", "Medicaid", "Private")) %>% 
    ##     mutate(race_ethnicity = factor(race_ethnicity), race_ethnicity = relevel(race_ethnicity, 
    ##         ref = "Caucasian"), insurance = factor(insurance), insurance = relevel(insurance, 
    ##         ref = "Private")))
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -97.086  -4.384   1.686   6.462  42.440 
    ## 
    ## Coefficients:
    ##                                                            Estimate
    ## (Intercept)                                               36.732480
    ## race_ethnicityBlack or African American                   -1.468444
    ## race_ethnicityHispanic                                    -1.390592
    ## insuranceMedicaid                                         -2.196106
    ## insuranceMedicare                                         -1.775398
    ## genderMale                                                 0.408028
    ## genderUnknown                                             -1.213175
    ## first_dr_age                                              -0.056661
    ## smoke_statusNo / Never                                    -0.127497
    ## smoke_statusUnknown / Unclassified                         0.472311
    ## smoke_statusYes / Active                                  -0.799874
    ## baseline_va_letter                                        -0.395687
    ## severity_score                                            -0.064491
    ## race_ethnicityBlack or African American:insuranceMedicaid -0.070026
    ## race_ethnicityHispanic:insuranceMedicaid                   0.611111
    ## race_ethnicityBlack or African American:insuranceMedicare  1.318884
    ## race_ethnicityHispanic:insuranceMedicare                  -0.101821
    ##                                                           Std. Error
    ## (Intercept)                                                 0.643648
    ## race_ethnicityBlack or African American                     0.368807
    ## race_ethnicityHispanic                                      0.338326
    ## insuranceMedicaid                                           0.355753
    ## insuranceMedicare                                           0.179157
    ## genderMale                                                  0.124862
    ## genderUnknown                                               0.930315
    ## first_dr_age                                                0.006356
    ## smoke_statusNo / Never                                      0.143386
    ## smoke_statusUnknown / Unclassified                          0.609009
    ## smoke_statusYes / Active                                    0.221158
    ## baseline_va_letter                                          0.004850
    ## severity_score                                              0.004158
    ## race_ethnicityBlack or African American:insuranceMedicaid   0.801226
    ## race_ethnicityHispanic:insuranceMedicaid                    0.655340
    ## race_ethnicityBlack or African American:insuranceMedicare   0.427397
    ## race_ethnicityHispanic:insuranceMedicare                    0.408802
    ##                                                           t value Pr(>|t|)
    ## (Intercept)                                                57.069  < 2e-16
    ## race_ethnicityBlack or African American                    -3.982 6.86e-05
    ## race_ethnicityHispanic                                     -4.110 3.96e-05
    ## insuranceMedicaid                                          -6.173 6.76e-10
    ## insuranceMedicare                                          -9.910  < 2e-16
    ## genderMale                                                  3.268 0.001085
    ## genderUnknown                                              -1.304 0.192225
    ## first_dr_age                                               -8.915  < 2e-16
    ## smoke_statusNo / Never                                     -0.889 0.373910
    ## smoke_statusUnknown / Unclassified                          0.776 0.438025
    ## smoke_statusYes / Active                                   -3.617 0.000299
    ## baseline_va_letter                                        -81.583  < 2e-16
    ## severity_score                                            -15.511  < 2e-16
    ## race_ethnicityBlack or African American:insuranceMedicaid  -0.087 0.930356
    ## race_ethnicityHispanic:insuranceMedicaid                    0.933 0.351079
    ## race_ethnicityBlack or African American:insuranceMedicare   3.086 0.002031
    ## race_ethnicityHispanic:insuranceMedicare                   -0.249 0.803307
    ##                                                              
    ## (Intercept)                                               ***
    ## race_ethnicityBlack or African American                   ***
    ## race_ethnicityHispanic                                    ***
    ## insuranceMedicaid                                         ***
    ## insuranceMedicare                                         ***
    ## genderMale                                                ** 
    ## genderUnknown                                                
    ## first_dr_age                                              ***
    ## smoke_statusNo / Never                                       
    ## smoke_statusUnknown / Unclassified                           
    ## smoke_statusYes / Active                                  ***
    ## baseline_va_letter                                        ***
    ## severity_score                                            ***
    ## race_ethnicityBlack or African American:insuranceMedicaid    
    ## race_ethnicityHispanic:insuranceMedicaid                     
    ## race_ethnicityBlack or African American:insuranceMedicare ** 
    ## race_ethnicityHispanic:insuranceMedicare                     
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 12.17 on 39891 degrees of freedom
    ## Multiple R-squared:  0.1453, Adjusted R-squared:  0.145 
    ## F-statistic:   424 on 16 and 39891 DF,  p-value: < 2.2e-16

#### Quantile regressionn
