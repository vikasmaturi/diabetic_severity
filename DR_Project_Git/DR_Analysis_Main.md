DR\_Analysis\_Main
================
Vikas Maturi
2021-07-07

## Libraries and parameters

``` r
# load libraries
library(tidyverse)
library(readxl)
library(lubridate)
library(knitr)

# Data files

# maturi universe file
universe_file <- "~/Documents/R_Projects/DR Analysis/DR_Project/maturi_universe_ultimate.xlsx"

# severity scale file
severity_file <- "~/Documents/R_Projects/DR Analysis/DR_Project/DR_severity_scale.xlsx"

# va file
va_file <- "~/Documents/R_Projects/DR Analysis/DR_Project/maturi_va_combine_20210508.csv"

# va scale file
va_scale_file <- "~/Documents/R_Projects/DR Analysis/DR_Project/visual_acuity_conversion.xlsx"

# injection file
injection_file <- "~/Documents/R_Projects/DR Analysis/DR_Project/maturi_antivegf_table_20210508.csv"

# Data output files

# path for cleaned, joined data file (all data)
all_cleaned_path <- "~/Documents/R_Projects/DR Analysis/DR_Project/maturi_all_cleaned.csv"

# path for cleaned, joined data file (top conditions)
top_conditions_cleaned_path <- "~/Documents/R_Projects/DR Analysis/DR_Project/maturi_top_cleaned.csv"

# bottom conditions - to be coded
bottom_codes_path <- "~/Documents/R_Projects/DR Analysis/DR_Project/bottom_codes.csv"

# va_basedate_by_pt 
va_basedate_path <- "~/Documents/R_Projects/DR Analysis/DR_Project/va_basedate.csv"

# universe of data with timeseries added
clean_universe_path <- "~/Documents/R_Projects/DR Analysis/DR_Project/clean_universe.csv" 
```

## Data import

``` r
# read in data

raw_universe <- read_excel(path = universe_file, sheet = 1, col_names = TRUE, .name_repair = "universal")

raw_severity <- read_excel(path = severity_file, sheet = 2, col_names = TRUE, .name_repair = "universal")

raw_va <- read_csv(file = va_file)

raw_va_scale <- read_excel(path = va_scale_file)

raw_injection <- read_csv(injection_file)
```

## Clean data

``` r
# Clean the raw data and save into new datasets
universe <-
  raw_universe %>% 
  rename(
    pt_code_name = Pt.code.name,
    eye = Eye.OD.1..OS.2,
    diagnosis_date = DR.diag.date
  ) %>% 
  mutate(
    diagnosis_date = ymd(diagnosis_date),
    index_date = ymd(index_date),
    baseline_va_letter = (1.7 - baseline_va)*50
  ) %>% 
  select(-FIND, -REPLACE, -...26, -...27, -...30, -...31)
  

severity <-
  raw_severity %>% 
  select(
    diabetes_diag = Diabetes.diagnosis,
    description = Description,
    ICD_10_equiv = ICD.10.equivalent,
    severity_score = Severity.Scale,
    new_class = New_Class,
    valid_class = Valid_Class
  ) %>% 
  mutate(
    severity_score = if_else(severity_score == "NA", as.integer(NA), as.integer(severity_score))
  )
```

    ## Warning in replace_with(out, !condition, false, fmt_args(~false),
    ## glue("length of {fmt_args(~condition)}")): NAs introduced by coercion

## Merge Data

``` r
# merge patient data with severity data
all <-
  universe %>% 
  left_join(severity, by = c("first_problem_code" = "diabetes_diag")) %>% 
  mutate(age_group = plyr::round_any(first_dr_age, 5))

# filter out data with less common conditions (~20k entries) if keeping all code types
# if fillter(code_type == New) is included, filter out data with less common conditions and classified under old system (~120k entries removed)
```

## Get VA data by time of visit for each patient/eye

``` r
va_basedate <-
  raw_va %>% 
  # add in the baseline date to the va data, by patient id and eye
  left_join(all %>% select(pt_code_name, eye, index_date), by = c("patient_guid" = "pt_code_name", "va_eye" = "eye")) %>% 
  # we find that half of the va data is not linked to the baseline patient/eye data that we have
  # filter entries where there is not linked baseline date
  filter(!is.na(index_date)) %>% 
  mutate(
    # change va to letter score
    va_letter = (1.7-va)*50,
    # determine if the va measurement is within 1 month +/- 3 weeks of the baseline date
    one_month = if_else(va_result_date > (index_date %m+% months(1) %m-% weeks(3)) & va_result_date < (index_date %m+% months(1) %m+% weeks(3)), 1, 0),
    # if the measurement is within 1 month +/- 3 weeks of the baseline date, then calculate the distance between the measurement date and 1 month past the index date
    one_mo_diff = if_else(one_month == 1, abs(index_date %m+% months(1) - va_result_date), make_difftime(NA)),
    six_month = if_else(va_result_date > (index_date %m+% months(6) %m-% weeks(8)) & va_result_date < (index_date %m+% months(6) %m+% weeks(8)), 1, 0),
    six_mo_diff = if_else(six_month == 1, abs(index_date %m+% months(6) - va_result_date), make_difftime(NA)),
    one_year = if_else(va_result_date > (index_date %m+% years(1) %m-% months(2)) & va_result_date < (index_date %m+% years(1) %m+% months(2)), 1, 0),
    one_yr_diff = if_else(one_year == 1, abs(index_date %m+% months(12) - va_result_date), make_difftime(NA)),
    two_year = if_else(va_result_date > (index_date %m+% years(2) %m-% months(4)) & va_result_date < (index_date %m+% years(2) %m+% months(4)), 1, 0),
    two_yr_diff = if_else(two_year == 1, abs(index_date %m+% months(24) - va_result_date), make_difftime(NA)),
  )

#write_csv(va_basedate, path = va_basedate_path)

index_date_results <-
  va_basedate %>% 
  summarize(
    sum_one_mo = sum(one_month),
    sum_six_mo = sum(six_month),
    sum_one_yr = sum(one_year),
    sum_two_yr = sum(two_year)
  )
```

It appears that using diagnosis date and index date result in a similar
amount of visits being identified, except for two-year later - ~100K
more visits are identified with diagnosis
data.

``` r
# create a cleaned dataset with each entry categorized as a one month, six month, one year, or two-year measurement, and the difference in days between the injection date and the "ideal' date (e.g., exactly one month after the index date for a one-month injection)

va_clean <-
  va_basedate %>% 
  mutate(
    # create consolidated column
    timeframe = case_when(
      one_month == 1 ~ "one_month",
      six_month == 1 ~ "six_month",
      one_year == 1 ~ "one_year",
      two_year == 1 ~ "two_year",
      TRUE ~ NA_character_
    )
  ) %>% 
  filter(!is.na(timeframe)) %>%
  rowwise() %>% 
  mutate(diff = sum(one_mo_diff, six_mo_diff, one_yr_diff, two_yr_diff, na.rm = TRUE)) %>% 
  ungroup()
```

``` r
# if there is more than one measurement within the timeframe, select the measurement closest to the ideal date (e.g., exactly one month, six months, etc. from the index date). If more than onne measurement at the same distance, select randomly.

va_to_join <-
  va_clean %>% 
  dplyr::select(-one_month, -one_mo_diff, -six_month, -six_mo_diff, -one_year, -one_yr_diff, -two_year, -two_yr_diff) %>% 
  group_by(patient_guid, va_eye, timeframe) %>% 
  top_n(n = -1, wt = diff) %>% 
  # for ties, select one of the rows at random
  sample_n(1) %>% 
  ungroup()

# completed va data
va_progression <- 
  va_to_join %>% 
  select(patient_guid, va_eye, index_date, va_letter, timeframe) %>% 
  spread(timeframe, va_letter) %>% 
  select(patient_guid, va_eye, index_date, one_month, six_month, one_year, two_year) 
```

``` r
va_prog_test <-
  va_progression %>%
  mutate(
    all_four = if_else((!is.na(six_month) & !is.na(one_year) & !is.na(two_year)), 1, 0),
    six_and_oneyear = if_else((!is.na(six_month) & !is.na(one_year)), 1, 0),
    one_two_year = if_else((!is.na(one_year) & !is.na(two_year)), 1, 0)
  )

va_prog_test %>% 
  count(all_four) %>% 
  mutate(prop = n / sum(n))
```

    ## # A tibble: 2 x 3
    ##   all_four      n  prop
    ##      <dbl>  <int> <dbl>
    ## 1        0 126082 0.565
    ## 2        1  96967 0.435

``` r
va_prog_test %>% 
  count(six_and_oneyear) %>% 
  mutate(prop = n / sum(n))
```

    ## # A tibble: 2 x 3
    ##   six_and_oneyear      n  prop
    ##             <dbl>  <int> <dbl>
    ## 1               0  80375 0.360
    ## 2               1 142674 0.640

``` r
va_prog_test %>% 
  count(one_two_year) %>% 
  mutate(prop = n / sum(n))
```

    ## # A tibble: 2 x 3
    ##   one_two_year      n  prop
    ##          <dbl>  <int> <dbl>
    ## 1            0 112360 0.504
    ## 2            1 110689 0.496

#### Bring the data back together

``` r
all_va_prog <- 
  va_progression %>% 
  left_join(all %>% select(first_problem_code, severity_score, gender, race_ethnicity, first_dr_age, age_group, insurance, region, smoke_status, new_class, valid_class, pt_code_name, eye, index_date, baseline_va_letter, proc_group_28, proc_group_365, proc_group_any, vegf_group_28, vegf_group_365, vegf_group_any, retina_speciality, baseline_iop, pdr_group, cat_eyes), by = c("patient_guid" = "pt_code_name", "va_eye" = "eye", "index_date"))
```

## Timeseries Analysis

``` r
# filter out the necesssary data to conduct time series analysis
timeseries_analysis <-
  all_va_prog %>% 
  filter(
    # only data classified under new ICD codes or codes starting of the format 36x.xxx
    valid_class == 1,
    #race is White, Black, or Hispanic, to ensure adequate numbers
    race_ethnicity %in% c("Caucasian", "Black or African American", "Hispanic"),
    #race and gender data available for the patient
    !is.na(race_ethnicity), 
    !is.na(gender),
    !is.na(severity_score),
    # only patients with one year and two year data available
    !is.na(one_year), 
    !is.na(two_year),
    # only patients getting antivegf treatment
    proc_group_28 == "antivegf",
    # no cat eyes
    !cat_eyes == 1
  ) %>% 
  # select only one eye per patient
  group_by(patient_guid) %>% 
  sample_n(1) %>% 
  ungroup() %>% 
  # adjust insurance data to combine medicare FFS and medicare managed
  mutate(
    insurance = if_else(insurance %in% c("Medicare FFS", "Medicare Managed"), "Medicare", insurance)
  ) 

# timeseries_plotting <-
#   timeseries_analysis %>% 
#   gather(key = "timepoint", value = "va_plot", one_year, two_year)

# clean_universe <-
#   write_csv(timeseries_analysis, path = clean_universe_path)
```

#### Timeseries VA by race and severity

``` r
severity_race_va <-
  timeseries_analysis %>% 
  group_by(race_ethnicity, severity_score) %>% 
  summarize(baseline_va = mean(baseline_va_letter), one_year_va = mean(one_year), two_year_va = mean(two_year), count = n()) %>% 
  gather(key = "timepoint", value = "va_plot", baseline_va, one_year_va, two_year_va)
```

``` r
# average two year visual acuity by starting severity and race

severity_race_va %>% 
  filter(timepoint == "two_year_va") %>% 
  filter(severity_score < 80) %>% 
  ggplot(aes(x = severity_score, y = va_plot, color = race_ethnicity)) +
  geom_point() +
  geom_line(aes(group = race_ethnicity)) +
  theme_light() + 
  labs(
    title = "VA at two year by race and severity score", 
    x = "Severity score", 
    y = "Two-year visual acuity"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

``` r
## density chart of distribution of severity scores, by race
timeseries_analysis %>% 
  ggplot() +
  geom_density(aes(x = severity_score, group = race_ethnicity, color = race_ethnicity), alpha = .2) +
  theme_light()
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

``` r
severity_race_va %>% 
  filter(severity_score == 73) %>% 
  ggplot(aes(x = timepoint, y = va_plot, color = race_ethnicity)) +
  geom_point() +
  geom_line(aes(group = race_ethnicity)) +
  theme_light() + 
  labs(
    title = "VA at baseline, one, and two years by race at severity score 73"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

``` r
severity_race_va %>% 
  filter(severity_score < 80) %>% 
  ggplot(aes(x = timepoint, y = va_plot, color = severity_score, linetype = race_ethnicity)) +
  geom_point() +
  geom_line(aes(group = interaction(severity_score, race_ethnicity))) +
  theme_light() + 
  scale_color_viridis_c() +
  labs(
    title = "VA at baseline, one, and two years by race and severity score"
  ) 
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->

#### Timeseries VA by insurance and race

``` r
insurance_race_va <-
  timeseries_analysis %>% 
  group_by(race_ethnicity, insurance) %>% 
  summarize(baseline_va = mean(baseline_va_letter), one_year_va = mean(one_year), two_year_va = mean(two_year), count = n()) %>% 
  gather(key = "timepoint", value = "va_plot", baseline_va, one_year_va, two_year_va)
```

``` r
## NOTE - be sure to combine the two medicare datasets rather than eliminating one

insurance_race_va %>% 
  filter(!insurance %in% c("Unknown/Missing", "Military", "Govt")) %>% 
  ggplot(aes(x = timepoint, y = va_plot, color = race_ethnicity, linetype = insurance)) +
  geom_point() +
  geom_line(aes(group = interaction(insurance, race_ethnicity))) +
  ggrepel::geom_text_repel(
    data = insurance_race_va %>% filter(timepoint == "two_year_va", insurance %in% c("Private", "Medicaid", "Medicare")), 
    aes(label = count, hjust = -1)
  ) +
  theme_light() + 
  labs(
    title = "VA at baseline, one, and two years by race and insurance"
  ) 
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->

There are disparities in baseline VA that persist over time for Black
and Hispanic patients as compared to White patients. The type of
insurance appears to have strong correlation to the baseline VA, but
does not fully explain the difference in race.

#### Timeseries VA by gender and race

``` r
gender_race_va <-
  timeseries_analysis %>% 
  group_by(race_ethnicity, gender) %>% 
  summarize(baseline_va = mean(baseline_va_letter), one_year_va = mean(one_year), two_year_va = mean(two_year), count = n()) %>% 
  gather(key = "timepoint", value = "va_plot", baseline_va, one_year_va, two_year_va)
```

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->
Female patients appear to hvae lower baseline VA as compared to their
male counterparts of the same race. The raw gain or loss in VA over time
appears to be similar between genders within racial groups.

## Injection data

``` r
# only for patients where raw injection data is 1 or 2
injection_count <-
  raw_injection %>%
  left_join(all %>% select(pt_code_name, eye, index_date), by = c("patient_guid" = "pt_code_name", "eye" =  "eye")) %>% 
  filter(index_date %m+% years(1) > injection_date) %>%
  select(patient_guid, eye, injection_date) %>% 
  distinct() 

raw_injection %>% 
  filter(patient_guid == "d402e848a6d5442387a0030ae910aeb3") %>% 
  arrange(desc(injection_date))
```

``` r
injection_count %>% 
  count(patient_guid, eye) %>% 
  count(n) %>% 
  arrange(desc(nn))
```

``` r
# raw_injection %>% 
#   count(patient_guid, eye, injection_date) %>% 
#   arrange(desc(n))
# 
# raw_injection %>% 
#   filter(patient_guid == "000340244d4948c1bf7d05483c11671c") %>% 
#   arrange(injection_date)
```

``` r
# injection_count %>% 
#   filter(n < 13) %>% 
#   ggplot(aes(x = n)) +
#   geom_histogram(binwidth = 1)
```

``` r
in_study <-
  all %>% 
  mutate(
    eye1 = if_else(eye == 1, 1, 0),
    eye2 = if_else(eye == 2, 1, 0)
  )  %>%
  group_by(pt_code_name) %>% 
  summarize(eye1 = sum(eye1), eye2 = sum(eye2))

eye_match <-
  raw_injection %>% 
  group_by(patient_guid, injection_date) %>% 
  add_count(eye) %>% 
  ungroup() %>% 
  arrange(desc(n)) %>% 
  spread(eye, n) 

eye_match <-
  eye_match %>% 
  rename(
    "one" = `1`, 
    "two" = `2`,
    "four" = `4`
  )


eye_match %>% filter(patient_guid == "d402e848a6d5442387a0030ae910aeb3")


injection_clean <-
  eye_match %>% 
  left_join(in_study, by = c("patient_guid" = "pt_code_name")) %>% 
  mutate(
    new_eye = case_when(
      (one == 1 & is.na(two) & is.na(four)) ~ 1, # keep entry
      (is.na(one) & two == 1 & is.na(four)) ~ 2,  # keep entry
      (one == 1 & two == 1 & is.na(four)) ~ 3, # 3 means entries should be made for both eyes 
      (one > 1 & is.na(two) & is.na(four)) ~ 1, # keep only one entry from this date
      (is.na(one) & two > 1 & is.na(four)) ~ 2, # keep only one entry from this date
      (one > 1 & two > 1 & is.na(four)) ~ 3, # 3 means keep oen entry for both eyes
      (is.na(one) & is.na(two) & four > 0 & eye1 == 1 & is.na(eye2)) ~ 1,
      (is.na(one) & is.na(two) & four > 0 & is.na(eye1) & eye2 == 1) ~ 2,
      (is.na(one) & is.na(two) & four > 0 & eye1 == 1 & eye2 == 1) ~ 5, # 5 means to remove all data for this person from the dataset
      (one > 0 & is.na(two) & four > 0 & eye1 == 1 & eye2 == 0) ~ 6, # throw out four, keep eye one
      (is.na(one) & two > 0 & four > 0 & eye1 == 0 & eye2 == 1) ~ 7, # throw out four, keep eye two
      (one > 0 & is.na(two) & four > 0 & eye1 == 1 & eye2 == 1) ~ 8, # throw out four and all eye two data
      (is.na(one) & two > 0 & four > 0 & eye1 == 1 & eye2 == 1) ~ 9, # throw out four and all eye one data
      
      (is.na()),
      TRUE ~ NA_real_
    )
  )
  

  left_join(all %>% select(pt_code_name, eye1, eye2, index_date), by = c("patient_guid" = "pt_code_name", "eye" =  "eye")) 
  select(pt_code_name, eye, eye1, eye2, code_type, index_date, first_problem_code, severity_score, race_ethnicity, first_dr_age, everything())
```

## Analysis

#### Dataset for baseline analysis

``` r
# ensure that the same filters are applied to the top conditions dataset as in the timeseries analysis
# NOTE TO SELF - use the updated timeseries analysis here (save top_conditions as time_series analysis to avoid confusion, or go through and replace)
top_conditions <-
  all %>% 
  filter(
    # only data classified under new ICD codes or codes starting of the format 36x.xxx
    valid_class == 1,
    #race and gender data available for the patient
    !is.na(race_ethnicity), 
    !is.na(gender),
    !is.na(severity_score),
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
  ) 
```

#### Severity by race

``` r
# Calculate severity by race
severity_race <-
  top_conditions %>% 
  group_by(race_ethnicity) %>% 
  summarize(average = mean(severity_score), count = n()) %>% 
  arrange(desc(average))

# Output severity by race to a cleaner table
severity_race %>% kable()
```

| race\_ethnicity           |  average | count |
| :------------------------ | -------: | ----: |
| Hispanic                  | 60.73258 | 20537 |
| Other                     | 58.98754 |  3210 |
| Unknown                   | 58.10541 | 14021 |
| Black or African American | 57.32034 | 18218 |
| Asian                     | 56.93627 |  3860 |
| Caucasian                 | 55.97076 | 74042 |

Hispanic people have the highest severity of all racial groups, nearaly
5 points higher than White people. Black, Asian, and Caucasian people
have similar severity at timme of diagnosis.

#### Severity by race and region

``` r
# Calculate severity by race and region
severity_race_reg <-
  top_conditions %>% 
  group_by(race_ethnicity, region) %>% 
  summarize(average = mean(severity_score), count = n()) %>% 
  arrange(desc(average))

# Print severity by race and region table
severity_race_reg
```

    ## # A tibble: 30 x 4
    ## # Groups:   race_ethnicity [6]
    ##    race_ethnicity            region    average count
    ##    <chr>                     <chr>       <dbl> <int>
    ##  1 Hispanic                  West         62.2  7051
    ##  2 Hispanic                  Midwest      61.2  1458
    ##  3 Hispanic                  South        60.6  8703
    ##  4 Other                     West         60.5  1197
    ##  5 Unknown                   West         59.9  4700
    ##  6 Other                     Midwest      58.5   447
    ##  7 Other                     South        58.3  1093
    ##  8 Unknown                   South        58.3  4725
    ##  9 Hispanic                  Northeast    58.2  2626
    ## 10 Black or African American South        57.8 10623
    ## # … with 20 more rows

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-29-1.png)<!-- -->

While there is some regional variation, Hispanic people are most likely
among all racial groups to have higher severity at time of diagnosis as
compared to other racial groups.

#### Severity by race and age

``` r
#calculate severity by race and age

severity_race_age <-
  top_conditions %>% 
  group_by(race_ethnicity, age_group) %>% 
  summarize(avg_severity = mean(severity_score), count = n()) %>% 
  arrange(desc(avg_severity))

severity_race_age %>% head(10) %>% kable()
```

| race\_ethnicity           | age\_group | avg\_severity | count |
| :------------------------ | ---------: | ------------: | ----: |
| Other                     |         35 |      70.62687 |    67 |
| Asian                     |         25 |      70.54545 |    22 |
| Other                     |         25 |      69.47368 |    19 |
| Hispanic                  |         35 |      68.59471 |   454 |
| Hispanic                  |         30 |      68.26766 |   269 |
| Hispanic                  |         25 |      68.23423 |   111 |
| Unknown                   |         30 |      67.96916 |   227 |
| Other                     |         30 |      67.95652 |    46 |
| Black or African American |         25 |      67.43836 |    73 |
| Asian                     |         30 |      67.18182 |    44 |

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-31-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-32-1.png)<!-- -->

\[New classification\] Average severity at time of diagnosis is ~1.5-2
pts higher for Black people vs. Caucasian people, and ~3-4 pts higher
for Hispanic poeple as compared to Caucasian people

#### Severity by region

``` r
# severity by region

severity_reg <-
  top_conditions %>% 
  group_by(region) %>% 
  summarize(average = mean(severity_score), n = n()) %>% 
  arrange(desc(average))

severity_reg %>% kable()
```

| region    |  average |     n |
| :-------- | -------: | ----: |
| West      | 58.86380 | 27165 |
| South     | 57.65373 | 53086 |
| Midwest   | 56.08193 | 27464 |
| Northeast | 55.82708 | 23675 |
| NA        | 55.22538 |  2498 |

Severity in the West does appear to be a few points higher than in other
regions - perhaps due to a higher number of Hispanic people being from
the West.

#### Severity by race and insurance

``` r
# severity by race and insurace

severity_race_ins <-
  top_conditions %>% 
  group_by(race_ethnicity, insurance) %>% 
  summarize(average = mean(severity_score), count = n()) %>% 
  arrange(desc(average))

severity_race_ins
```

    ## # A tibble: 36 x 4
    ## # Groups:   race_ethnicity [6]
    ##    race_ethnicity insurance       average count
    ##    <chr>          <chr>             <dbl> <int>
    ##  1 Other          Medicaid           62.9   351
    ##  2 Hispanic       Govt               62.7   379
    ##  3 Other          Govt               62.4   104
    ##  4 Hispanic       Medicaid           61.8  2447
    ##  5 Hispanic       Unknown/Missing    61.6  2490
    ##  6 Unknown        Medicaid           60.8  1425
    ##  7 Hispanic       Private            60.7  4905
    ##  8 Caucasian      Medicaid           60.5  4016
    ##  9 Hispanic       Medicare           60.3 10200
    ## 10 Unknown        Govt               60.1   298
    ## # … with 26 more rows

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-35-1.png)<!-- -->

We see large Hispanic/white gaps for those on Govt,Medicare FFS, Private
or with Unknown/Missing insurance information. Smaller Hispanic/white
gap for those on Medicaid or with Military insurance.

#### Severity by race and sex

``` r
# severity by race and sex

severity_race_sex <-
  top_conditions %>% 
  group_by(race_ethnicity, gender) %>% 
  summarize(average = mean(severity_score), count = n()) %>% 
  arrange(desc(average))

severity_race_sex %>% head(5) %>% kable()
```

| race\_ethnicity | gender  |  average | count |
| :-------------- | :------ | -------: | ----: |
| Hispanic        | Male    | 61.84665 | 10766 |
| Hispanic        | Female  | 59.52116 |  9640 |
| Other           | Male    | 59.46301 |  1568 |
| Unknown         | Unknown | 59.21154 |    52 |
| Other           | Female  | 58.58075 |  1610 |

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-37-1.png)<!-- -->

Male patients have higher severity at the time of diagnosis than female
patients among all racial groups.

#### Severity by type of treatment

``` r
# severity by race and type of treatment

severity_race_first_treatment <-
  top_conditions %>% 
  group_by(race_ethnicity, proc_group_28) %>% 
  summarize(average = mean(severity_score), count = n()) %>% 
  arrange(desc(average))

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-39-1.png)<!-- -->

Physicians are treating people with higher severity within their racial
group with antivegf or combo drugs for their first treatment, as
compared to laser or steroid.

``` r
# Likelihood of first treatment type by severity and race
treatment_sev_race <-
  top_conditions %>% 
  count(severity_score, race_ethnicity, proc_group_28) %>% 
  group_by(severity_score, race_ethnicity) %>% 
  mutate(count_severity_race = sum(n)) %>% 
  mutate(prop = n / count_severity_race) %>% 
  ungroup() %>% 
  select(severity_score, race_ethnicity, proc_group_28, prop, count = n)

treatment_sev_race %>% 
  arrange(desc(severity_score, race_ethnicity))
```

    ## # A tibble: 269 x 5
    ##    severity_score race_ethnicity            proc_group_28   prop count
    ##             <int> <chr>                     <chr>          <dbl> <int>
    ##  1             85 Asian                     antivegf      0.8        4
    ##  2             85 Asian                     laser         0.2        1
    ##  3             85 Black or African American antivegf      0.848     39
    ##  4             85 Black or African American combo         0.0217     1
    ##  5             85 Black or African American laser         0.130      6
    ##  6             85 Caucasian                 antivegf      0.897     52
    ##  7             85 Caucasian                 laser         0.103      6
    ##  8             85 Hispanic                  antivegf      0.923     24
    ##  9             85 Hispanic                  combo         0.0385     1
    ## 10             85 Hispanic                  laser         0.0385     1
    ## # … with 259 more rows

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-41-1.png)<!-- -->
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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-42-1.png)<!-- -->

\[New classification\] Black people appear to be 2-5% points less likely
to receive antivegf treatment as compared to Caucasian people at several
severity scores (53, 58, 73, 78)

We can extend this analysis by looking at which antivegf drugs patients
of different races receive, at different severity
levels.

#### Likelihood of drug received by severity score and race (given that patient recieved anti-vegf treatment)

``` r
# Calculate likelihood of drug by severity score and race (given that patient received antivegf treatment)
drug_sev_race <-
  top_conditions %>% 
  filter(proc_group_28 == "antivegf") %>% 
  count(severity_score, race_ethnicity, vegf_group_28) %>% 
  group_by(severity_score, race_ethnicity) %>% 
  mutate(count_severity_race = sum(n)) %>% 
  mutate(prop = n / count_severity_race) %>% 
  ungroup() %>% 
  select(severity_score, race_ethnicity, vegf_group_28, prop, count = n)

drug_sev_race %>% 
  arrange(desc(severity_score, race_ethnicity))
```

    ## # A tibble: 263 x 5
    ##    severity_score race_ethnicity            vegf_group_28   prop count
    ##             <int> <chr>                     <chr>          <dbl> <int>
    ##  1             85 Asian                     Avastin       1          4
    ##  2             85 Black or African American Avastin       0.949     37
    ##  3             85 Black or African American Eylea         0.0513     2
    ##  4             85 Caucasian                 Avastin       0.788     41
    ##  5             85 Caucasian                 Eylea         0.135      7
    ##  6             85 Caucasian                 Lucentis      0.0769     4
    ##  7             85 Hispanic                  Avastin       0.833     20
    ##  8             85 Hispanic                  Eylea         0.125      3
    ##  9             85 Hispanic                  Lucentis      0.0417     1
    ## 10             85 Other                     Avastin       1          7
    ## # … with 253 more rows

``` r
# Graphing likelihood of first drug type by severity and race
drug_sev_race %>% 
  filter(!race_ethnicity %in% c("Unknown")) %>% 
  ggplot(aes(x = severity_score, y = prop, color = race_ethnicity, shape = vegf_group_28)) +
  geom_point() + 
  theme_bw() +
  #scale_y_continuous(labels = scales::percent, breaks = c(.70, .75, .8, .85, .9), limits = c(.7, .9)) +
  labs(
    title = "Likelihood of drug at each severity score by race",
    y = "Percentage of people receiving this drug"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-44-1.png)<!-- -->

``` r
# Graphing likelihood of Eylea as first treatment by severity and race
drug_sev_race %>% 
  filter(!race_ethnicity %in% c("Unknown", "Other")) %>% 
  filter(severity_score > 50) %>% 
  filter(vegf_group_28 == "Eylea") %>% 
  ggplot(aes(x = severity_score, y = prop, color = race_ethnicity)) +
  geom_point() + 
  theme_bw() +
  scale_y_continuous(labels = scales::percent) +
  labs(
    title = "Likelihood of receiving Eylea at higher severity score by race",
    y = "Percentage of people receiving Eylea"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-45-1.png)<!-- -->

Caucasian people are 5-10% more likely to receive Eylea treatment
(first) as compared to Hispanic people at the same level of severity,
and 2-5% more likely to receive Eylea treatment (first) as compared to
Black people at the same level of severity.

``` r
# Graphing likelihood of Avastin as first treatment by severity and race
drug_sev_race %>% 
  filter(!race_ethnicity %in% c("Unknown", "Other")) %>% 
  filter(severity_score < 50) %>% 
  filter(vegf_group_28 == "Avastin") %>% 
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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-46-1.png)<!-- -->

At lower levels of severity, Hispanic people are far more likely
(15-25%) to receive Avastin as compared to their Caucasian counterparts
with the same level of severity. Black and Asian people seem somewhat
more likely to receive Avastin, but there is some variation.

#### Drug received by race and insurance

Part of the reason there may be variation by race in which treatment
drug is received is due to insurance. Medical providers may only choose
drugs covered by the patients insurance (e.g., Medicare does not cover
Eylea).

``` r
# Calculate likelihood of drug by severity score and race (given that patient received antivegf treatment)
drug_sev_race_ins <-
  top_conditions %>% 
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

    ## # A tibble: 1,212 x 6
    ##    severity_score race_ethnicity      insurance   vegf_group_28  prop count
    ##             <int> <chr>               <chr>       <chr>         <dbl> <int>
    ##  1             85 Asian               Medicaid    Avastin       1         1
    ##  2             85 Asian               Medicare    Avastin       1         1
    ##  3             85 Asian               Private     Avastin       1         1
    ##  4             85 Asian               Unknown/Mi… Avastin       1         1
    ##  5             85 Black or African A… Medicaid    Avastin       1         3
    ##  6             85 Black or African A… Medicare    Avastin       0.895    17
    ##  7             85 Black or African A… Private     Avastin       1        16
    ##  8             85 Black or African A… Unknown/Mi… Avastin       1         1
    ##  9             85 Black or African A… Medicare    Eylea         0.105     2
    ## 10             85 Caucasian           Govt        Avastin       1         1
    ## # … with 1,202 more rows

``` r
# Graphing likelihood of Eylea as first treatment by severity, race, and Medicare FFS  insurance
drug_sev_race_ins %>% 
  filter(!race_ethnicity %in% c("Unknown", "Other")) %>% 
  filter(severity_score > 50) %>% 
  filter(vegf_group_28 == "Eylea") %>% 
  filter(insurance == "Medicare") %>% 
  ggplot(aes(x = severity_score, y = prop, color = race_ethnicity)) +
  geom_point() + 
  geom_line() +
  theme_bw() +
  scale_y_continuous(labels = scales::percent) +
  labs(
    title = "Likelihood of receiving Eylea at higher severity score by race, with Medicare FFS insurance",
    y = "Percentage of people receiving Eylea"
  )
```

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-48-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-49-1.png)<!-- -->

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-50-1.png)<!-- -->

#### Severity vs. vision

Do severity scores differ by race for people with the same vision score?

``` r
#calculate severity by race and age

severity_race_vision <-
  top_conditions %>% 
  group_by(race_ethnicity, baseline_va_letter) %>%
  summarize(avg_severity = mean(severity_score), count = n()) %>% 
  filter(count > 100) %>% 
  arrange(desc(avg_severity))

severity_race_vision
```

    ## # A tibble: 72 x 4
    ## # Groups:   race_ethnicity [6]
    ##    race_ethnicity            baseline_va_letter avg_severity count
    ##    <chr>                                  <dbl>        <dbl> <int>
    ##  1 Hispanic                                26           65.7   110
    ##  2 Hispanic                                35           65.2  1160
    ##  3 Hispanic                                40           65.1   454
    ##  4 Hispanic                                50           63.6  1040
    ##  5 Unknown                                 35           63.5   609
    ##  6 Hispanic                                55.          63.1   966
    ##  7 Unknown                                 40           63.0   245
    ##  8 Caucasian                               26           63.0   312
    ##  9 Caucasian                               45.0         62.8   351
    ## 10 Black or African American               35           62.8   885
    ## # … with 62 more rows

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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-52-1.png)<!-- -->

Hispanic people are more likely to have a higher starting severity score
at all levels of baseline visual acuity, as compared to Black, Asian,
and Caucasian people.

``` r
#calculate severity by race and age

severity_race_vision_flip <-
  top_conditions %>% 
  group_by(race_ethnicity, severity_score) %>%
  summarize(avg_va = mean(baseline_va_letter), count = n()) %>% 
  filter(count > 100) %>% 
  arrange(desc(avg_va))


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

![](DR_Analysis_Main_files/figure-gfm/unnamed-chunk-53-1.png)<!-- -->
