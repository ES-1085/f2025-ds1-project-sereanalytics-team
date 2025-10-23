Project proposal
================
SerenAnalytics

``` r
library(tidyverse)
library(broom)
library(skimr)
library(janitor)
## Add any additional packages you are using here
```

## 1. Introduction

Our data comes from the UN Human Development Report. More specifically,
it is from the 2025 Global Survey on AI and Human Development. The data
set contains 21 countries that are separated in 4 categories: “High
Human Development Group”, “Low Human Development Group”, “Medium Human
Development Groups”, and “Very High Human Development Group”. The data
collected for these 21 countries is split up into 6 categories:  
- Demographic section: contains questions like age, gender, education,
and occupation  
- AI Interaction section: contains questions like AI interaction
frequency, AI use purposes, Internet access, and familiarity with AI  
- Culture and AI Perception section: contains a question about culture
and AI perception  
- Agency section: contains questions like current freedom of choice,
future freedom of choice, and AI expose and control confidence  
- Social Trust section: contains questions like trust in people, trust
in AI systems, and confidence in AI systems  
- Future Expectations section: contains questions like future AI use,
and AI impact

Our research question is focused on understanding how the use of AI
differs across different demographic groups and if that is related to
the status of the countries that these demographic groups are in. The
variables/categories we will use are Demographics, AI interactions,
Culture and AI Perception, Social Trust section, Future Expectations,
and Human Development. We might want to be looking at AI development in
the global south. More specifically, looking at the difference between
users of AI and producers (data centers) of AI.

## 2. Data

``` r
project_data <- read_csv("Data.csv")
```

    ## Rows: 1176 Columns: 7
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (6): Country ISO, Country, Section, Question, Description, Unit
    ## dbl (1): Value
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
glimpse(project_data)
```

    ## Rows: 1,176
    ## Columns: 7
    ## $ `Country ISO` <chr> "AUS", "AUS", "AUS", "AUS", "BGD", "BGD", "BGD", "BGD", …
    ## $ Country       <chr> "Australia", "Australia", "Australia", "Australia", "Ban…
    ## $ Section       <chr> "Age Group", "Age Group", "Age Group", "Age Group", "Age…
    ## $ Question      <chr> "Q1. Age 15-17", "Q1. Age 18-35", "Q1. Age 36-59", "Q1. …
    ## $ Description   <chr> "Respondents aged 15-17", "Respondents aged 18-35", "Res…
    ## $ Value         <dbl> 0.00, 31.15, 39.45, 29.40, 8.49, 43.16, 35.36, 12.99, 5.…
    ## $ Unit          <chr> "%", "%", "%", "%", "%", "%", "%", "%", "%", "%", "%", "…

``` r
project_data <- project_data %>%
  mutate(category = case_when(
    Country == "Nigeria" ~ "Low",
    Country == "Pakistan" ~ "Low",
    Country == "Bangladesh" ~ "Medium",
    Country == "Comoros" ~ "Medium",
    Country == "India" ~ "Medium",
    Country == "Kyrgyzstan" ~ "Medium",
    Country == "Brazil" ~ "High",
    Country == "China" ~ "High",
    Country == "Egypt" ~ "High",
    Country == "Fiji" ~ "High",
    Country == "Indonesia" ~ "High",
    Country == "South Africa" ~ "High",
    Country == "Tunisia" ~ "High",
    Country == "Australia" ~ "Very High",
    Country == "Germany" ~ "Very High",
    Country == "Greece" ~ "Very High",
    Country == "Japan" ~ "Very High",
    Country == "Republic of Korea" ~ "Very High",
    Country == "Russian Federation" ~ "Very High",
    Country == "Türkiye" ~ "Very High",
    Country == "United States of America" ~ "Very High"
  ))
```

``` r
project_data <- project_data %>% select(-Question)
project_data <- project_data %>% select(-Unit)
```

``` r
project_data_wide <- project_data %>%
  unite("Section_Label", Section, Description, sep = " - ") %>%
  pivot_wider(
    names_from = Section_Label,
    values_from = Value
  )
```

``` r
project_data_wide <- project_data_wide %>%
  clean_names()

project_data_clean <- project_data_wide %>%
  rename(
    iso = country_iso,
    country = country,
    category = category,
    mean_age = mean_age_mean_age_of_survey_respondents,
    age_15_17 = age_group_respondents_aged_15_17,
    age_18_35 = age_group_respondents_aged_18_35,
    age_36_59 = age_group_respondents_aged_36_59,
    age_60_plus = age_group_respondents_aged_60,
    female = gender_group_female_respondents,
    male = gender_group_male_respondents,
    other_gender = gender_group_other_respondents,
    edu_primary = education_group_completed_primary_or_less,
    edu_secondary = education_group_completed_secondary,
    edu_university = education_group_completed_university_or_more,
    occ_fulltime = occupation_group_full_time_employee,
    occ_parttime = occupation_group_part_time_employee,
    occ_self = occupation_group_self_employed,
    occ_unemployed = occupation_group_unemployed,
    occ_retired = occupation_group_retired,
    occ_student = occupation_group_student,
    occ_unpaid = occupation_group_unpaid_worker,
    trust_people = trust_in_people_most_people_can_be_trusted,
    trust_ai_confident = trust_in_ai_systems_trust_in_ai_systems_very_confident,
    ai_productivity_likely = ai_impact_ai_will_increase_productivity_likely
  )
```

## 3. Data analysis plan

The variables we will be visualizing to explore the research questions
include:  
- Country  
- Age group  
- Gender group  
- Education group  
- Occupation group  
- Disability group  
- AI Interaction Frequency  
- Familiarity with AI  
- Culture and AI Perception  
- Trust in People  
- Trust in AI Systems  
- AI impact

We will not be needing data from other sources but we will most likely
want to add a variable that separates the countries by their groups (as
mentioned above)

Types of graphs that we want to use:  
- Bar graph with “facet_wrap” based on countries or their human
development dynamics.  
- Scatter plot of trust in people vs trust in AI systems  
- Violin Plot of AI interaction frequency and age group?  
- Geom density of familiarity with AI and occupation group  
- Scatter plot of AI interaction frequency and trust in AI systems

Reflections:  
- What questions do you have about your dataset and project?  
The biggest question that we have about our dataset right now is
understanding the different categories that the questions were split
into. When we are only looking at the survey data, it is hard to know
exactly what the questions were that led to these responses. We will be
reading the report that the UN wrote following that survey to make sure
that we understand everything properly. We also as a team are still
looking at what the best option is to visualize the data and narrow our
research question even further. We feel like there are a lot of things
that can be done when looking at data that concerns AI because it’s a
subject that is at the forefront of most fields. A lot of things can be
interesting to look at so we are still in the process of narrowing down
the research questions as well as what type of visualizations we want to
use to answer that question.

- What parts of your proposal would you like the reviewers to focus on
  in particular or give advice and suggestions?  
  We would like reviewers to focus on what types of graphs we plan to
  make. We want extra feedback on if these plots with the variables
  listed would be the most effective way to communicate what we want to.

- What support do you think you will need from the CAT and Professor for
  this project?  
  I think the support that we will need from the CAT and Professor for
  this project will mostly be for handling the data. Since it is survey
  data, it is pretty hard to clean up in a way that makes sense to be
  able to run a clear data analysis plan.
