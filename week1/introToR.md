# Lecture 2: Working with Data in R and Rstudio

## Objectives

1. What's a data frame?
2. Get the data into R
3. Figure out what's in the dataset

Identifying the unit of analysis
differentiating between the types of variables

4. Manipulate the data frame using the R package `dplyr` main functions

- rename()
- select()
- arrange()
- filter()
- mutate()
- group_by()
- summarize()

## Data Frames, df (01:05)

- A data frame is a data set
- We read data into R from common sources like `.xls`, `.txt`, or `.csv` files
- The simplest format of data contains one row for each individual/unit of analysis in the study
- The first column of the data identifies the individual (perhaps by name or ID variable)
- subsequent columns are the other variables that have been recorded or measured

### df examples

Lake data from Baldi & Moore

`lake_data <- read_csv("mercury-lake.csv")`, where:

- `lake_data` is data frame we're creating, 
- `<-` is the assignment operator,
- `read_csv()` is a function from the `readr` library used to import csv files,
- and `"mercury-lake.csv"` is the pathway to the file we want to bring into R (this file is in the same folder/directory as the R script)

## Describing data (07:36)

Four functions to get to know a dataset

`head()`: shows the first six rows of the supplied dataset
`dim()`: provides the number of rows by the number of columns (returns number of observations by number of variables)
`names()`: lists the variable names of the columns in the dataset
`str()`: summarizes the above info and more

a # indicates a comment, code won't run following a #

### Units of Analysis (10:20)

The unit of analysis is the major entity you are working with:

- bacteria,
- lab results,
- individual people,
- groups of people,
- villages,
- countries

Which R function lets us know how many units we have? A: `dim()`

### Type of Variables (12:08)

#### Categorical variable

A variable that has grouping levels. Mathematically you can calc the proportion (%) of individuals in each level of the category

1. Nominal variables: no underlying order or rank
2. Ordinal variables: can be ordered or ranked, inherent ranking (BMI, SES)

#### Quantitative Variables

A continuous, numeric variable that you can perform math operations on (taking th median or mean of these values)

1. Discrete variables: can be counted (number of previous births)
2. Continuous variables: can be measured precisely, with a ruler or scale, divided into smaller, meaningful subunits (annual income, blood alcohol content, gestational age at birth)

### dplyr functions for data manipulation (17:00)

`library(dplyr)` would load dplyr into our script for use. Things from previous packages might be overwritten from older packages - bascially would now use the dplyr function, instead of the older function.

`names()`: prints the names of the variables in the data frame

`rename()`: changes the name of a specified variable in the data frame to a provided name
    **ex:** `lake_data_tidy <- rename(lake_data, name_of_lake = lakes)` would change **lakes** to be **name_of_lake** in the lake_data_tidy df

`select()`: subsets the data based on the provided argument (we can choose certain variables we want)
    **ex:** `smaller_data <- select(lake_data, lakes, ph, chlorophyll)` would save the lake_data, lakes, ph, and chlorophyll variables & observations into the smaller_data df
    Can also perform negative selection (omission)
    **example:** `sammler_data_2 <- select(lake_data, - age_data)`, where the `- age_data` tells us to omit the age_data variable from the new subset

(28:35 timestamp)

`arrange()`: can sort, based on the argument, in a certain order
    **example:** `lake_data %>% arrange(age_data, ph)` would sort in ascending order on first the age_data, then the ph variables

`filter()`: select which rows (observations) we want to keep in the dataset
    **example:** `lake_data_filtered <- lake_data %>% filter(ph > 7)` would have observations only where `ph` variable is more than 7
    `%in%` operator, selecting observations where variable has values in a provided list (when used with filter)
    `&` or `,` used to indicate AND operation in `filter()`
    `|` indicates OR operation in `filter()`


`mutate()`: super useful function, used to add new variables to the dataset
    **example:** `lake_data_new_fish <- lake_data %>% mutate(actual_fish_sampled = number_fish * 100)` would add a new variable, named actual_fish_sampled, to the lake_data_new_fish df

`group_by()`: groups the data by a categorical variable

`summarize()`: summarizes the data using basic stats (CLT)

`%>%` pipe operator, allows us to use multiple functions/stack function, think of it as an "and then do this" instruction
    **ex:**

    ```R
    tidy_lake_data <- lake_data %>%
        rename(name_of_lake = lakes) %>%
        mutate(actual_fish_sampled = number_fish * 100) %>%
        select(- age_data, - number_fish)
    ```

