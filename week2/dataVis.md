# Lecture 3: Visualizing Data

## Objectives

Using `ggplot` in R

## Don't use pie charts

generally speaking, not a good idea to use pie charts. why?

- hard to judge the size of the slices relative to one another
  - bar chart better for the display of categorical data
- bar charts display the number or percent of data

### Categorical variables: Infectious disease data example

want to make a bar chart of the prcent of cases on id for each category of disease

load data: `id_data <- read_csv("id_data.csv")`

loading ggplot: `library(ggplot2)`
setting up plot: `ggplot(id_data, aes=(x = disease, y = percent_cases)) +`, aes is aesthestic. This line sets up the canvas
make a bar chart: `geom_bar(stat = "identity")` identity tells us the quantity (percent) of cases
styling: `base_size` controls the font size

visually informative: 
```r
id_data <- id_data %>%
  mutate(disease_ordered = fct_reorder(disease, percent_cases, .desc)) # now the chart is ordered descending
```

coloring in geom_bar: `geom_bar(stat= "identity", aes(fill=type))` to link the bar's fill to the disease type

## visualizing quantitative variables using histograms

histograms because the underlying scale is continuous and the order of the bars matters
In order to make a histogram, the data needs to be binned into categories and the number or percent

### Example: opioid state Rx rates

`opi_data <- read.csv("data.csv")`
`head(opi_data)` inspecting the data
mean provides the mean prescribing rate per 100 individuals.

Creating the histogram:
x variable are the bins of rates

`ggplot(data=opi_data, aes(x= Mean)) +
geom_histogram(col= "white", binwidth = 5) +
labs( x = "Mean opioid rx rate per 100 individuals", y = "number of states") +
theme_minimal(base_size = 15)`

## Describing distributions

- when we examine histograms, we can make comments on a distributions:
  - shape: is the dist symmetric or skewed (left, right)? skewed right if the tail is loooong to the right
  - Center: does the histogram have one peak, or multiple (unimodal vs bimodal)?
  - spread: how spread out are the values, what is the range of the data?
  - outliers: do any of the measurements fall outside of the range of most of the data points?

## Time plots

Used for continuous data
specific subset of plots where the x variable is time
unlike the previous plots, the time plot shows a relationship between two variables:
1. a quant variable
2. time
Often, these plots are used to observe cycles/trends

### Example: average rainfall by month in a year, or life expectancy in CA

if we're looking just for white men in california, we want to use the `filter()` function to select certain criteria

```r
wm_cali <- le_data %>% 
  filter(state == "California",
  sex == "Male",
  race == "White")
```
`wm_cali` is now a dataset with just white men in california

## Line and point graphs

```r
ggplot(data = wm_cali, aes(x = year, y = LE)) +
  geom_point() + 
  labs(title = "Life expect in white men in Cali, 1969-2013)",
    y = "Life expectancy",
    x = "Year",
    caption = "Data from Riddell et al. (2018)")
```

### geom_line()

```r
ggplot(data = wm_cali, aes(x = year, y = LE)) +
  geom_line(col = "blue") + 
  labs(title = "Life expect in white men in Cali, 1969-2013)",
    y = "Life expectancy",
    x = "Year",
    caption = "Data from Riddell et al. (2018)")
```

## Review

`ggplot` to set up canvas for graphics
`geom_bar(stat = "identity")` to make a bar chart when you specify the y variable
`geom_histogram()` to make a histogram for which ggplot needs to calculate the count
`fct_reorder(var1,var2)` to reorder the categorical variable (var1) by a numeric variable (var2)
`geom_point()` makes dot plots
`geom_line()` make line plots