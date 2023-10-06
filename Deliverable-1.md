Mini Data-Analysis Deliverable 1
================

# Welcome to your (maybe) first-ever data analysis project!

And hopefully the first of many. Let’s get started:

1.  Install the [`datateachr`](https://github.com/UBC-MDS/datateachr)
    package by typing the following into your **R terminal**:

<!-- -->

    install.packages("devtools")
    devtools::install_github("UBC-MDS/datateachr")

2.  Load the packages below.

``` r
library(datateachr)
library(tidyverse)
```

3.  Make a repository in the <https://github.com/stat545ubc-2023>
    Organization. You can do this by following the steps found on canvas
    in the entry called [MDA: Create a
    repository](https://canvas.ubc.ca/courses/126199/pages/mda-create-a-repository).
    One completed, your repository should automatically be listed as
    part of the stat545ubc-2023 Organization.

# Instructions

## For Both Milestones

- Each milestone has explicit tasks. Tasks that are more challenging
  will often be allocated more points.

- Each milestone will be also graded for reproducibility, cleanliness,
  and coherence of the overall Github submission.

- While the two milestones will be submitted as independent
  deliverables, the analysis itself is a continuum - think of it as two
  chapters to a story. Each chapter, or in this case, portion of your
  analysis, should be easily followed through by someone unfamiliar with
  the content.
  [Here](https://swcarpentry.github.io/r-novice-inflammation/06-best-practices-R/)
  is a good resource for what constitutes “good code”. Learning good
  coding practices early in your career will save you hassle later on!

- The milestones will be equally weighted.

## For Milestone 1

**To complete this milestone**, edit [this very `.Rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-1.Rmd)
directly. Fill in the sections that are tagged with
`<!--- start your work below --->`.

**To submit this milestone**, make sure to knit this `.Rmd` file to an
`.md` file by changing the YAML output settings from
`output: html_document` to `output: github_document`. Commit and push
all of your work to the mini-analysis GitHub repository you made
earlier, and tag a release on GitHub. Then, submit a link to your tagged
release on canvas.

**Points**: This milestone is worth 36 points: 30 for your analysis, and
6 for overall reproducibility, cleanliness, and coherence of the Github
submission.

# Learning Objectives

By the end of this milestone, you should:

- Become familiar with your dataset of choosing
- Select 4 questions that you would like to answer with your data
- Generate a reproducible and clear report using R Markdown
- Become familiar with manipulating and summarizing your data in tibbles
  using `dplyr`, with a research question in mind.

# Task 1: Choose your favorite dataset

The `datateachr` package by Hayley Boyce and Jordan Bourak currently
composed of 7 semi-tidy datasets for educational purposes. Here is a
brief description of each dataset:

- *apt_buildings*: Acquired courtesy of The City of Toronto’s Open Data
  Portal. It currently has 3455 rows and 37 columns.

- *building_permits*: Acquired courtesy of The City of Vancouver’s Open
  Data Portal. It currently has 20680 rows and 14 columns.

- *cancer_sample*: Acquired courtesy of UCI Machine Learning Repository.
  It currently has 569 rows and 32 columns.

- *flow_sample*: Acquired courtesy of The Government of Canada’s
  Historical Hydrometric Database. It currently has 218 rows and 7
  columns.

- *parking_meters*: Acquired courtesy of The City of Vancouver’s Open
  Data Portal. It currently has 10032 rows and 22 columns.

- *steam_games*: Acquired courtesy of Kaggle. It currently has 40833
  rows and 21 columns.

- *vancouver_trees*: Acquired courtesy of The City of Vancouver’s Open
  Data Portal. It currently has 146611 rows and 20 columns.

**Things to keep in mind**

- We hope that this project will serve as practice for carrying our your
  own *independent* data analysis. Remember to comment your code, be
  explicit about what you are doing, and write notes in this markdown
  document when you feel that context is required. As you advance in the
  project, prompts and hints to do this will be diminished - it’ll be up
  to you!

- Before choosing a dataset, you should always keep in mind **your
  goal**, or in other ways, *what you wish to achieve with this data*.
  This mini data-analysis project focuses on *data wrangling*,
  *tidying*, and *visualization*. In short, it’s a way for you to get
  your feet wet with exploring data on your own.

And that is exactly the first thing that you will do!

1.1 **(1 point)** Out of the 7 datasets available in the `datateachr`
package, choose **4** that appeal to you based on their description.
Write your choices below:

**Note**: We encourage you to use the ones in the `datateachr` package,
but if you have a dataset that you’d really like to use, you can include
it here. But, please check with a member of the teaching team to see
whether the dataset is of appropriate complexity. Also, include a
**brief** description of the dataset here to help the teaching team
understand your data.

<!-------------------------- Start your work below ---------------------------->

1: `apt_buildings`  
2: `cancer_sample`  
3: `parking_meters`  
4: `steam_games`  

<!----------------------------------------------------------------------------->

1.2 **(6 points)** One way to narrowing down your selection is to
*explore* the datasets. Use your knowledge of dplyr to find out at least
*3* attributes about each of these datasets (an attribute is something
such as number of rows, variables, class type…). The goal here is to
have an idea of *what the data looks like*.

*Hint:* This is one of those times when you should think about the
cleanliness of your analysis. I added a single code chunk for you below,
but do you want to use more than one? Would you like to write more
comments outside of the code chunk?

<!-------------------------- Start your work below ---------------------------->

``` r
# Save each of the four datasets as local variables.
# This will cut down on runtime because otherwise we would constantly be loading a 'fresh' copy of the same dataset.
apts <- apt_buildings
cancer <- cancer_sample
parking <- parking_meters
steam <- steam_games
```

First, we will count the number of `numeric` columns and `character`
columns in each of the four datasets.

A `numeric` column in a dataset indicates that the data corresponding to
that variable can be represented by a numerical value, which typically
means that it is a quantitative variable. On the other hand, a
`character` column has values stored as text, which typically represents
that the variable is qualitative or categorical in nature. By finding
these counts of columns in our data, we can get a better understanding
of the data types which we are likely to encounter in each of the four
selected datasets.

``` r
# Count number of numeric columns per dataset
apts_numeric_cols = ncol(select_if(apts, is.numeric))
cancer_numeric_cols = ncol(select_if(cancer, is.numeric))
parking_numeric_cols = ncol(select_if(parking, is.numeric))
steam_numeric_cols = ncol(select_if(steam, is.numeric))

# Count number of character columns per dataset
apts_character_cols = ncol(select_if(apts, is.character))
cancer_character_cols = ncol(select_if(cancer, is.character))
parking_character_cols = ncol(select_if(parking, is.character))
steam_character_cols = ncol(select_if(steam, is.character))

# Print each of these values as a sentence (this is mostly for the sake of readability)
cat(paste("The `apt_buildings` dataset has", apts_numeric_cols, "numeric variables and", 
          apts_character_cols, "text-based (character) variables.\n"))
cat(paste("The `cancer_sample` dataset has", cancer_numeric_cols, "numeric variables and", 
          cancer_character_cols, "text-based (character) variables.\n"))
cat(paste("The `parking_meters` dataset has", parking_numeric_cols, "numeric variables and", 
          parking_character_cols, "text-based (character) variables.\n"))
cat(paste("The `steam_games` dataset has", steam_numeric_cols, "numeric variables and", 
          steam_character_cols, "text-based (character) variables.\n"))
```

    The `apt_buildings` dataset has 9 numeric variables and 28 text-based (character) variables.
    The `cancer_sample` dataset has 31 numeric variables and 1 text-based (character) variables.
    The `parking_meters` dataset has 2 numeric variables and 20 text-based (character) variables.
    The `steam_games` dataset has 4 numeric variables and 17 text-based (character) variables.

Next, for each of the four datasets, we will use the `glimpse` function
from `dplyr` to look at some of the values stored in each column of the
dataset. In addition to showing a snippet of the values for each column
in the dataset, this function will also display the names and data types
of each column, as well as the dimensions of the dataset. The `glimpse`
function allows us to get a better feel for the data as a whole, and it
can help us evaluate the ‘tidiness’ of each dataset.

``` r
# View a glimpse of each of the four datasets
glimpse(apts)
```

    Rows: 3,455
    Columns: 37
    $ id                               <dbl> 10359, 10360, 10361, 10362, 10363, 10…
    $ air_conditioning                 <chr> "NONE", "NONE", "NONE", "NONE", "NONE…
    $ amenities                        <chr> "Outdoor rec facilities", "Outdoor po…
    $ balconies                        <chr> "YES", "YES", "YES", "YES", "NO", "NO…
    $ barrier_free_accessibilty_entr   <chr> "YES", "NO", "NO", "YES", "NO", "NO",…
    $ bike_parking                     <chr> "0 indoor parking spots and 10 outdoo…
    $ exterior_fire_escape             <chr> "NO", "NO", "NO", "YES", "NO", NA, "N…
    $ fire_alarm                       <chr> "YES", "YES", "YES", "YES", "YES", "Y…
    $ garbage_chutes                   <chr> "YES", "YES", "NO", "NO", "NO", "NO",…
    $ heating_type                     <chr> "HOT WATER", "HOT WATER", "HOT WATER"…
    $ intercom                         <chr> "YES", "YES", "YES", "YES", "YES", "Y…
    $ laundry_room                     <chr> "YES", "YES", "YES", "YES", "YES", "Y…
    $ locker_or_storage_room           <chr> "NO", "YES", "YES", "YES", "NO", "YES…
    $ no_of_elevators                  <dbl> 3, 3, 0, 1, 0, 0, 0, 2, 4, 2, 0, 2, 2…
    $ parking_type                     <chr> "Underground Garage , Garage accessib…
    $ pets_allowed                     <chr> "YES", "YES", "YES", "YES", "YES", "Y…
    $ prop_management_company_name     <chr> NA, "SCHICKEDANZ BROS. PROPERTIES", N…
    $ property_type                    <chr> "PRIVATE", "PRIVATE", "PRIVATE", "PRI…
    $ rsn                              <dbl> 4154812, 4154815, 4155295, 4155309, 4…
    $ separate_gas_meters              <chr> "NO", "NO", "NO", "NO", "NO", "NO", "…
    $ separate_hydro_meters            <chr> "YES", "YES", "YES", "YES", "YES", "Y…
    $ separate_water_meters            <chr> "NO", "NO", "NO", "NO", "NO", "NO", "…
    $ site_address                     <chr> "65  FOREST MANOR RD", "70  CLIPPER R…
    $ sprinkler_system                 <chr> "YES", "YES", "NO", "YES", "NO", "NO"…
    $ visitor_parking                  <chr> "PAID", "FREE", "UNAVAILABLE", "UNAVA…
    $ ward                             <chr> "17", "17", "03", "03", "02", "02", "…
    $ window_type                      <chr> "DOUBLE PANE", "DOUBLE PANE", "DOUBLE…
    $ year_built                       <dbl> 1967, 1970, 1927, 1959, 1943, 1952, 1…
    $ year_registered                  <dbl> 2017, 2017, 2017, 2017, 2017, NA, 201…
    $ no_of_storeys                    <dbl> 17, 14, 4, 5, 4, 4, 4, 7, 32, 4, 4, 7…
    $ emergency_power                  <chr> "NO", "YES", "NO", "NO", "NO", "NO", …
    $ `non-smoking_building`           <chr> "YES", "NO", "YES", "YES", "YES", "NO…
    $ no_of_units                      <dbl> 218, 206, 34, 42, 25, 34, 14, 105, 57…
    $ no_of_accessible_parking_spaces  <dbl> 8, 10, 20, 42, 12, 0, 5, 1, 1, 6, 12,…
    $ facilities_available             <chr> "Recycling bins", "Green Bin / Organi…
    $ cooling_room                     <chr> "NO", "NO", "NO", "NO", "NO", "NO", "…
    $ no_barrier_free_accessible_units <dbl> 2, 0, 0, 42, 0, NA, 14, 0, 0, 1, 25, …

``` r
glimpse(cancer)
```

    Rows: 569
    Columns: 32
    $ ID                      <dbl> 842302, 842517, 84300903, 84348301, 84358402, …
    $ diagnosis               <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M", "…
    $ radius_mean             <dbl> 17.990, 20.570, 19.690, 11.420, 20.290, 12.450…
    $ texture_mean            <dbl> 10.38, 17.77, 21.25, 20.38, 14.34, 15.70, 19.9…
    $ perimeter_mean          <dbl> 122.80, 132.90, 130.00, 77.58, 135.10, 82.57, …
    $ area_mean               <dbl> 1001.0, 1326.0, 1203.0, 386.1, 1297.0, 477.1, …
    $ smoothness_mean         <dbl> 0.11840, 0.08474, 0.10960, 0.14250, 0.10030, 0…
    $ compactness_mean        <dbl> 0.27760, 0.07864, 0.15990, 0.28390, 0.13280, 0…
    $ concavity_mean          <dbl> 0.30010, 0.08690, 0.19740, 0.24140, 0.19800, 0…
    $ concave_points_mean     <dbl> 0.14710, 0.07017, 0.12790, 0.10520, 0.10430, 0…
    $ symmetry_mean           <dbl> 0.2419, 0.1812, 0.2069, 0.2597, 0.1809, 0.2087…
    $ fractal_dimension_mean  <dbl> 0.07871, 0.05667, 0.05999, 0.09744, 0.05883, 0…
    $ radius_se               <dbl> 1.0950, 0.5435, 0.7456, 0.4956, 0.7572, 0.3345…
    $ texture_se              <dbl> 0.9053, 0.7339, 0.7869, 1.1560, 0.7813, 0.8902…
    $ perimeter_se            <dbl> 8.589, 3.398, 4.585, 3.445, 5.438, 2.217, 3.18…
    $ area_se                 <dbl> 153.40, 74.08, 94.03, 27.23, 94.44, 27.19, 53.…
    $ smoothness_se           <dbl> 0.006399, 0.005225, 0.006150, 0.009110, 0.0114…
    $ compactness_se          <dbl> 0.049040, 0.013080, 0.040060, 0.074580, 0.0246…
    $ concavity_se            <dbl> 0.05373, 0.01860, 0.03832, 0.05661, 0.05688, 0…
    $ concave_points_se       <dbl> 0.015870, 0.013400, 0.020580, 0.018670, 0.0188…
    $ symmetry_se             <dbl> 0.03003, 0.01389, 0.02250, 0.05963, 0.01756, 0…
    $ fractal_dimension_se    <dbl> 0.006193, 0.003532, 0.004571, 0.009208, 0.0051…
    $ radius_worst            <dbl> 25.38, 24.99, 23.57, 14.91, 22.54, 15.47, 22.8…
    $ texture_worst           <dbl> 17.33, 23.41, 25.53, 26.50, 16.67, 23.75, 27.6…
    $ perimeter_worst         <dbl> 184.60, 158.80, 152.50, 98.87, 152.20, 103.40,…
    $ area_worst              <dbl> 2019.0, 1956.0, 1709.0, 567.7, 1575.0, 741.6, …
    $ smoothness_worst        <dbl> 0.1622, 0.1238, 0.1444, 0.2098, 0.1374, 0.1791…
    $ compactness_worst       <dbl> 0.6656, 0.1866, 0.4245, 0.8663, 0.2050, 0.5249…
    $ concavity_worst         <dbl> 0.71190, 0.24160, 0.45040, 0.68690, 0.40000, 0…
    $ concave_points_worst    <dbl> 0.26540, 0.18600, 0.24300, 0.25750, 0.16250, 0…
    $ symmetry_worst          <dbl> 0.4601, 0.2750, 0.3613, 0.6638, 0.2364, 0.3985…
    $ fractal_dimension_worst <dbl> 0.11890, 0.08902, 0.08758, 0.17300, 0.07678, 0…

``` r
glimpse(parking)
```

    Rows: 10,032
    Columns: 22
    $ meter_head     <chr> "Twin", "Pay Station", "Twin", "Single", "Twin", "Twin"…
    $ r_mf_9a_6p     <chr> "$2.00", "$1.00", "$1.00", "$1.00", "$2.00", "$2.00", "…
    $ r_mf_6p_10     <chr> "$4.00", "$1.00", "$1.00", "$1.00", "$1.00", "$1.00", "…
    $ r_sa_9a_6p     <chr> "$2.00", "$1.00", "$1.00", "$1.00", "$2.00", "$2.00", "…
    $ r_sa_6p_10     <chr> "$4.00", "$1.00", "$1.00", "$1.00", "$1.00", "$1.00", "…
    $ r_su_9a_6p     <chr> "$2.00", "$1.00", "$1.00", "$1.00", "$2.00", "$2.00", "…
    $ r_su_6p_10     <chr> "$4.00", "$1.00", "$1.00", "$1.00", "$1.00", "$1.00", "…
    $ rate_misc      <chr> NA, "$ .50", NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
    $ time_in_effect <chr> "METER IN EFFECT: 9:00 AM TO 10:00 PM", "METER IN EFFEC…
    $ t_mf_9a_6p     <chr> "2 Hr", "10 Hrs", "2 Hr", "2 Hr", "2 Hr", "3 Hr", "2 Hr…
    $ t_mf_6p_10     <chr> "4 Hr", "10 Hrs", "4 Hr", "4 Hr", "4 Hr", "4 Hr", "4 Hr…
    $ t_sa_9a_6p     <chr> "2 Hr", "10 Hrs", "2 Hr", "2 Hr", "2 Hr", "3 Hr", "2 Hr…
    $ t_sa_6p_10     <chr> "4 Hr", "10 Hrs", "4 Hr", "4 Hr", "4 Hr", "4 Hr", "4 Hr…
    $ t_su_9a_6p     <chr> "2 Hr", "10 Hrs", "2 Hr", "2 Hr", "2 Hr", "3 Hr", "2 Hr…
    $ t_su_6p_10     <chr> "4 Hr", "10 Hrs", "4 Hr", "4 Hr", "4 Hr", "4 Hr", "4 Hr…
    $ time_misc      <chr> NA, "No Time Limit", NA, NA, NA, NA, NA, NA, NA, NA, NA…
    $ credit_card    <chr> "No", "Yes", "No", "No", "No", "No", "No", "No", "No", …
    $ pay_phone      <chr> "66890", "59916", "57042", "57159", "51104", "60868", "…
    $ longitude      <dbl> -123.1289, -123.0982, -123.1013, -123.1862, -123.1278, …
    $ latitude       <dbl> 49.28690, 49.27215, 49.25468, 49.26341, 49.26354, 49.27…
    $ geo_local_area <chr> "West End", "Strathcona", "Riley Park", "West Point Gre…
    $ meter_id       <chr> "670805", "471405", "C80145", "D03704", "301023", "5913…

``` r
glimpse(steam)
```

    Rows: 40,833
    Columns: 21
    $ id                       <dbl> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14…
    $ url                      <chr> "https://store.steampowered.com/app/379720/DO…
    $ types                    <chr> "app", "app", "app", "app", "app", "bundle", …
    $ name                     <chr> "DOOM", "PLAYERUNKNOWN'S BATTLEGROUNDS", "BAT…
    $ desc_snippet             <chr> "Now includes all three premium DLC packs (Un…
    $ recent_reviews           <chr> "Very Positive,(554),- 89% of the 554 user re…
    $ all_reviews              <chr> "Very Positive,(42,550),- 92% of the 42,550 u…
    $ release_date             <chr> "May 12, 2016", "Dec 21, 2017", "Apr 24, 2018…
    $ developer                <chr> "id Software", "PUBG Corporation", "Harebrain…
    $ publisher                <chr> "Bethesda Softworks,Bethesda Softworks", "PUB…
    $ popular_tags             <chr> "FPS,Gore,Action,Demons,Shooter,First-Person,…
    $ game_details             <chr> "Single-player,Multi-player,Co-op,Steam Achie…
    $ languages                <chr> "English,French,Italian,German,Spanish - Spai…
    $ achievements             <dbl> 54, 37, 128, NA, NA, NA, 51, 55, 34, 43, 72, …
    $ genre                    <chr> "Action", "Action,Adventure,Massively Multipl…
    $ game_description         <chr> "About This Game Developed by id software, th…
    $ mature_content           <chr> NA, "Mature Content Description  The develope…
    $ minimum_requirements     <chr> "Minimum:,OS:,Windows 7/8.1/10 (64-bit versio…
    $ recommended_requirements <chr> "Recommended:,OS:,Windows 7/8.1/10 (64-bit ve…
    $ original_price           <dbl> 19.99, 29.99, 39.99, 44.99, 0.00, NA, 59.99, …
    $ discount_price           <dbl> 14.99, NA, NA, NA, NA, 35.18, 70.42, 17.58, N…

Lastly, we will calculate the proportion of missing values in each of
the four datasets. This step can be very helpful in choosing a dataset
to analyze further, as it is often easier to work with a dataset with
few or no missing entries.

``` r
# Compute proportion of NA values in each dataset
apts_missing_prop = sum(is.na(apts)) / (nrow(apts) * ncol(apts))
cancer_missing_prop = sum(is.na(cancer)) / (nrow(cancer) * ncol(cancer))
parking_missing_prop = sum(is.na(parking)) / (nrow(parking) * ncol(parking))
steam_missing_prop = sum(is.na(steam)) / (nrow(steam) * ncol(steam))

# Round each proportion to 5 decimal places and multiply by 100 (to write it as a percentage)
apts_missing_prop = round(apts_missing_prop, 5) * 100
cancer_missing_prop = round(cancer_missing_prop, 5) * 100
parking_missing_prop = round(parking_missing_prop, 5) * 100
steam_missing_prop = round(steam_missing_prop, 5) * 100

# Print each of these values as a sentence (this is mostly for the sake of readability)
cat(paste0("Proportion of missing values in the `apt_buildings` dataset: ", apts_missing_prop, "%\n"))
cat(paste0("Proportion of missing values in the `cancer_sample` dataset: ", cancer_missing_prop, "%\n"))
cat(paste0("Proportion of missing values in the `parking_meters` dataset: ", parking_missing_prop, "%\n"))
cat(paste0("Proportion of missing values in the `steam_games` dataset: ", steam_missing_prop, "%\n"))
```

    Proportion of missing values in the `apt_buildings` dataset: 4.917%
    Proportion of missing values in the `cancer_sample` dataset: 0%
    Proportion of missing values in the `parking_meters` dataset: 8.652%
    Proportion of missing values in the `steam_games` dataset: 21.096%

<!----------------------------------------------------------------------------->

1.3 **(1 point)** Now that you’ve explored the 4 datasets that you were
initially most interested in, let’s narrow it down to 1. What lead you
to choose this one? Briefly explain your choice below.

<!-------------------------- Start your work below ---------------------------->

Of the four datasets which were examined above, the one which I would
like to examine in further detail for the Mini Data Analysis project is
the `apt_buildings` dataset.

I chose to work with this data because I was born and raised in Toronto,
and I lived there for the first 22 years of my life before moving to
Vancouver to attend UBC. Because I have spent so much of my life in
Toronto, I believe that I have a lot of background knowledge about the
city of Toronto as a whole which can help me to develop a stronger
research question and a more thorough analysis of the apartment building
data.

Additionally, I wanted to work with the `apt_buildings` dataset because
the data is both metaphorically and literally “close to home”.

<!----------------------------------------------------------------------------->

1.4 **(2 points)** Time for a final decision! Going back to the
beginning, it’s important to have an *end goal* in mind. For example, if
I had chosen the `titanic` dataset for my project, I might’ve wanted to
explore the relationship between survival and other variables. Try to
think of 1 research question that you would want to answer with your
dataset. Note it down below.

<!-------------------------- Start your work below ---------------------------->

The main research question which I would like to tackle in this data
analysis project is:

**How have the characteristics of apartment buildings in the City of
Toronto changed over time?**

In particular, I would like to examine the relationships between the
year that an apartment building was built (`build_year`) and qualities
of the apartment building such as the number of stories and units in the
building (`no_of_storeys` and `no_of_units`), and the building’s
amenities (`amenities`) to analyze how newly-built apartments differ
from older apartment buildings due to factors such as modern
construction techniques and an increased need for medium-density and
high-density housing in the city.

<!----------------------------------------------------------------------------->

# Important note

Read Tasks 2 and 3 *fully* before starting to complete either of them.
Probably also a good point to grab a coffee to get ready for the fun
part!

This project is semi-guided, but meant to be *independent*. For this
reason, you will complete tasks 2 and 3 below (under the **START HERE**
mark) as if you were writing your own exploratory data analysis report,
and this guidance never existed! Feel free to add a brief introduction
section to your project, format the document with markdown syntax as you
deem appropriate, and structure the analysis as you deem appropriate. If
you feel lost, you can find a sample data analysis
[here](https://www.kaggle.com/headsortails/tidy-titarnic) to have a
better idea. However, bear in mind that it is **just an example** and
you will not be required to have that level of complexity in your
project.

# Task 2: Exploring your dataset

If we rewind and go back to the learning objectives, you’ll see that by
the end of this deliverable, you should have formulated *4* research
questions about your data that you may want to answer during your
project. However, it may be handy to do some more exploration on your
dataset of choice before creating these questions - by looking at the
data, you may get more ideas. **Before you start this task, read all
instructions carefully until you reach START HERE under Task 3**.

2.1 **(12 points)** Complete *4 out of the following 8 exercises* to
dive deeper into your data. All datasets are different and therefore,
not all of these tasks may make sense for your data - which is why you
should only answer *4*.

Make sure that you’re using dplyr and ggplot2 rather than base R for
this task. Outside of this project, you may find that you prefer using
base R functions for certain tasks, and that’s just fine! But part of
this project is for you to practice the tools we learned in class, which
is dplyr and ggplot2.

1.  Plot the distribution of a numeric variable.
2.  Create a new variable based on other variables in your data (only if
    it makes sense)
3.  Investigate how many missing values there are per variable. Can you
    find a way to plot this?
4.  Explore the relationship between 2 variables in a plot.
5.  Filter observations in your data according to your own criteria.
    Think of what you’d like to explore - again, if this was the
    `titanic` dataset, I may want to narrow my search down to passengers
    born in a particular year…
6.  Use a boxplot to look at the frequency of different observations
    within a single variable. You can do this for more than one variable
    if you wish!
7.  Make a new tibble with a subset of your data, with variables and
    observations that you are interested in exploring.
8.  Use a density plot to explore any of your variables (that are
    suitable for this type of plot).

2.2 **(4 points)** For each of the 4 exercises that you complete,
provide a *brief explanation* of why you chose that exercise in relation
to your data (in other words, why does it make sense to do that?), and
sufficient comments for a reader to understand your reasoning and code.

<!-------------------------- Start your work below ---------------------------->

### Exercise 3:

In order to get a better understanding of the `apt_buildings` dataset,
it would be helpful to know how many of the observations in the dataset
are missing and how many of them have recorded values.

To do this, we can calculate the missing and recorded values for each
variable (column) of the dataset, and create a bar plot in order to get
a grasp on the distribution of missing values in the data.

``` r
# Count the number of missing and recorded values per column and create a tibble with this data
apts_missing <- tibble(variable = colnames(apts), 
                       missing = colSums(is.na(apts)), 
                       recorded = colSums(!is.na(apts)))

# Use pivot longer to put all counts in one column (this is required for the plot)
apts_missing <- apts_missing %>%
  pivot_longer(cols = !variable, names_to = "type", values_to = "count")

# Create a column plot to visualize counts of missing/recorded observations for each variable
apts_missing %>% ggplot(aes(x = forcats::fct_rev(variable), y = count, fill = type)) +
  geom_col() +
  theme_bw() +
  labs(x = "Variable Name", y = "Count", fill = "Type of Observation", 
       title = "Missing and Recorded Observations in the Apartment Buildings Dataset per Column") +
  theme(plot.title = element_text(hjust = 0.5)) +
  scale_fill_manual(values = c("#BA2F2A", "#088158")) +
  coord_flip()
```

![](Deliverable-1_files/figure-gfm/exercise-3-1.png)<!-- -->

Based on the plot above, we see that there are only two variables with
high proportions of unrecorded values (relative to the rest of the
dataset), which are the `amenities` and `prop_management_company_name`
variables.

It is likely that a missing value in the `amenities` category is meant
to represent that the apartment building does not have any additional
amenities, as opposed to simply representing a missing observation for
that apartment building.

Similarly, a missing value in the `prop_management_company_name`
variable may mean that the building is not managed by a property
management company, as opposed to implying that the property management
company is simply unknown.

### Exercise 2:

As we saw above, the `amenities` variable has a very high proportion of
missing values compared to the rest of the `apt_buildings` dataset, and
we hypothesize that these unrecorded values are meaningful, as they
could represent that there are no amenities in the building, as opposed
to the data simply being missing.

To remedy this, we will create a new numeric variable called
`no_of_amenities` (following the dataset’s naming convention), which
represents the number of amenities in the building. An `NA` value
represents that the building has zero amenities, and if a building has
multiple amenities, they are listed as one string of text in which each
amenity is separated by a comma. Thus, we can count the number of
amenities in the building by adding 1 to the number of commas in the
`amenities` column.

Additionally, we will make a variable called `decade_built`, which
represents the decade when an apartment building was built. This
variable will allow us to group apartment buildings together based on a
larger range of time than just one year, which will be convenient for
plotting our data later.

``` r
# Create a new variable called "no_of_amenities" representing the number of amenities that the apartment building has.
apts <- apts %>%
  mutate(no_of_amenities = if_else(is.na(amenities), 0, 
                                   stringr::str_count(amenities, ",") + 1))

# Create a new variable called "decade_built" representing the decade when a building was built
apts <- apts %>%
  # Divide by 10 to get the decade + a decimal (for example, 196.4)
  mutate(decade_built = year_built/10) %>% 
  # Use floor to just get the first three digits of the year (for example, 196)
  mutate(decade_built = floor(decade_built)) %>%
  # Multiply by 10 to get the first year of the decade (for example, 1960)
  mutate(decade_built = 10 * decade_built) %>%
  # Convert to a string and add an 's' on the end (for example, '1960s')
  mutate(decade_built = paste0(as.character(decade_built), "s"))
```

### Exercise 4:

We would like to examine the relationship between the number of stories
in an apartment building, the number of units in the building, and the
number of amenities which the building has.

To accomplish this goal in one visualization, we can use a scatter plot
to visualize the relationship between two of these variables by putting
them on the axes, and we can also include the third variable by having
the size of the points in the scatter plot be proportional to the third
variable.

``` r
apts %>% 
  # There was one massive outlier with over 4000 units, which seems to have been a data entry error
  # as it was listed at 5x the size of the second-largest building, and seems impossibly large. 
  filter(no_of_units < 4000) %>% 
  # Create a scatter plot of stories and units, with point size proportional to amenities
  ggplot(aes(x = no_of_storeys, y = no_of_units, size = no_of_amenities)) +
    # Color each point by property type and lower alpha (opacity)
    geom_point(aes(color = property_type), alpha = 0.15) +
    scale_color_manual(values = c("#FF9933", "#015E05", "#0047AB")) +
    theme_bw() +
    labs(x = "Number of Stories", y = "Number of Units", color = "Property Type", size = "Number of Amenities",
         title = "Number of stories, units, and amenities in each apartment building in the City of Toronto",
         subtitle = "Based on a dataset of 3455 apartment buildings from the OpenData Toronto portal") +
    theme(plot.title = element_text(hjust = 0.5), plot.subtitle = element_text(hjust = 0.5))
```

![](Deliverable-1_files/figure-gfm/exercise-4-1.png)<!-- -->

In the plot above, we see that most of the largest apartment buildings
(in terms of both units and stories) in the City of Toronto are
privately-owned buildings, with a few TCHC (Toronto Community Housing
Corporation) buildings among the largest in the city. As we may have
expected, there is a very strong linear correlation between the number
of stories in a building and the number of units in the building.

It also appears that a majority of the larger buildings in the City of
Toronto have some amenities, with a small amount of these buildings not
offering any amenities to their residents.

### Exercise 8:

To visually inspect how the sizes of apartment buildings have changed
over time, we can use the `ggridges` package to create ridgeplots for
each decade between 1880 and 2020 which display the density of units in
apartment buildings which were built in that decade. By putting these
ridgeplots together on one large plot, we will be able to see the
differences in apartment building sizes in Toronto by decade.

``` r
apts %>%
  # Remove any missing observations
  drop_na(year_built, air_conditioning) %>% 
  # There was one massive outlier with over 4000 units, which seems to have been a data entry error
  # as it was listed at 5x the size of the second-largest building, and seems impossibly large.
  filter(no_of_units < 4000) %>% 
  # Drop years before 1880 as there is not enough data to make the density ridges 
  filter(year_built >= 1880) %>%
  # Create the plot and make it look nice
  ggplot(aes(x = no_of_units, y = as.factor(decade_built))) +
    ggridges::geom_density_ridges(alpha = 1/3, fill = "#443399") +
    theme_bw() +
    labs(x = "Number of Units", y = "Decade of Construction",
         title = "Number of units per apartment building in Toronto by decade of building construction from 1880-2020",
         subtitle = "Based on a dataset of 3455 apartment buildings from the OpenData Toronto portal") +
    theme(plot.title = element_text(hjust = 0.5), plot.subtitle = element_text(hjust = 0.5))
```

![](Deliverable-1_files/figure-gfm/exercise-8-1.png)<!-- -->

Based on the plot above, we see that the distribution of apartment
building sizes in Toronto remain quite steady (and slowly grow over
time) from the 1880’s to the 1950’s. Starting in the 1960’s, we see a
noticeable uptick in the number of newly-constructed apartment buildings
with more than 100 units, and this pattern of constructing larger
apartment buildings has remained into the 2010’s, while the number of
units in these these “larger” apartment buildings continues to grow each
decade.

<!----------------------------------------------------------------------------->

# Task 3: Choose research questions

**(4 points)** So far, you have chosen a dataset and gotten familiar
with it through exploring the data. You have also brainstormed one
research question that interested you (Task 1.4). Now it’s time to pick
4 research questions that you would like to explore in Milestone 2!
Write the 4 questions and any additional comments below.

<!--- *****START HERE***** --->

The four research questions which I would like to explore in more detail
in Milestone 2 are: 1. **As the City of Toronto grows, are new apartment
buildings more commonly built in areas which are further from the
downtown core? Where are new apartment buildings being built in the City
of Toronto?** 2. **Which features (e.g. accessible entrances, balconies,
laundry rooms, underground parking) are more common in recently-built
apartment buildings than older buildings? Which features are more common
in older buildings?** 3. **Is there a correlation between the size of a
building any the number of amenities offered in the building? Are these
values correlated with newer buildings in general?“** 4. **How do
privately-owned apartment buildings differ from the buildings which are
owned by the City of Toronto? Are certain features more common in
privately-owned buildings than in Social Housing and TCHC buildings?**

<!----------------------------->

# Overall reproducibility/Cleanliness/Coherence Checklist

## Coherence (0.5 points)

The document should read sensibly from top to bottom, with no major
continuity errors. An example of a major continuity error is having a
data set listed for Task 3 that is not part of one of the data sets
listed in Task 1.

## Error-free code (3 points)

For full marks, all code in the document should run without error. 1
point deduction if most code runs without error, and 2 points deduction
if more than 50% of the code throws an error.

## Main README (1 point)

There should be a file named `README.md` at the top level of your
repository. Its contents should automatically appear when you visit the
repository on GitHub.

Minimum contents of the README file:

- In a sentence or two, explains what this repository is, so that
  future-you or someone else stumbling on your repository can be
  oriented to the repository.
- In a sentence or two (or more??), briefly explains how to engage with
  the repository. You can assume the person reading knows the material
  from STAT 545A. Basically, if a visitor to your repository wants to
  explore your project, what should they know?

Once you get in the habit of making README files, and seeing more README
files in other projects, you’ll wonder how you ever got by without them!
They are tremendously helpful.

## Output (1 point)

All output is readable, recent and relevant:

- All Rmd files have been `knit`ted to their output md files.
- All knitted md files are viewable without errors on Github. Examples
  of errors: Missing plots, “Sorry about that, but we can’t show files
  that are this big right now” messages, error messages from broken R
  code
- All of these output files are up-to-date – that is, they haven’t
  fallen behind after the source (Rmd) files have been updated.
- There should be no relic output files. For example, if you were
  knitting an Rmd to html, but then changed the output to be only a
  markdown file, then the html file is a relic and should be deleted.

(0.5 point deduction if any of the above criteria are not met. 1 point
deduction if most or all of the above criteria are not met.)

Our recommendation: right before submission, delete all output files,
and re-knit each milestone’s Rmd file, so that everything is up to date
and relevant. Then, after your final commit and push to Github, CHECK on
Github to make sure that everything looks the way you intended!

## Tagged release (0.5 points)

You’ve tagged a release for Milestone 1.

### Attribution

Thanks to Icíar Fernández Boyano for mostly putting this together, and
Vincenzo Coia for launching.
