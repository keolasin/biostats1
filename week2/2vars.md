# Lecture 4: Describing data with numbers

Objectives

1. investigate measures of centrality
   1. mean, median, when they're the same vs different
2. investigate measures of spread
   1. IQR, standard dev, and variance
3. create a visualization of the five number summary
   1. boxplots using ggplot
4. calculate the variance and standard deviation

## Measures of central tendency

### Mean

average of all observations divided by the number of observations

- outliers and skewed distributions will move the mean into the tail/towards the outlier

```r
mean(rent_data[,"sym"])
```

### Median

Point at which half the measures are larger, and half are smaller

- if it's an even number, it's the average of the two central numbers
- not largely affected by outliers

```r
median(rent_data[,"sym"])
```

Where "sym" are the observations for the variable `sym` in the dataframe

#### Summarize()

```r
rent_data %>% summarize(
    mean = mean(sym),
    median = median(sym))
)
```

### When are mean/median the same?

When the data has one peak and is roughly symmetric

In skewed data:

- mean =/= median
- right-skewed data will commonly have a **larger** mean than median
- left-skewed data will commonly have a **smaller** mean than the median

Measures of central tendency are not very helpful in multi-modal distributions

## Inter-quartile range (IQR)

Q1 is the 1st quartile, 25th percentile
    25% of individuals have measurements below Q1
Q2 is the 2nd quartile, 50th percentile, the median
    50% of individuals have measurements below Q2
Q3 the 3rd quartile, 75th percentile

Q1-Q3 is called the IQR

```r
quantile(variable, 0.25)
```

or

```r
rent_data %>% summarize(
    Q1 = quantile(sym, 0.25),
    median = median(sym),
    Q3 = quantile(sym, 0.75)
)
```

### R's quantile function

`quantile(variable, 0.25)` not always the exact same answer as calculated by hand, so to compare to the calculation by hand, use

```r
quantile(data, 0.25, type=2)
```

## The Range of data

The difference between the minimum and maximum values

## Using dplyr's `summarize()`

```r
rent_data %>% summarize(
    min = min(sym),
    Q1 = quantile(sym, 0.25),
    median = median(sym),
    Q3 = quantile(sym, 0.75),
    max = max(sym)
)
```

## Sample variance and standard deviation

Let s^2 represent the variance of a sample

$s^2 = (x_1 - \bar{x})^2 + (x_2 - \bar{x})^2 + ... + (x_n - \bar{x})^2 / (n-1)$

Let s represent the standard deviation of a sample, so: 

$s = \sqrt{1/(n-1)\sum_{i=1}^{n}(x_i - \bar{x})^2}$

### Using R to calculate std dev and variance

```r
CS_dat %>% summarize(
    cs_sd = sd(cs_rate),
    cs_var = var(cs_rate)
)
```

## Box plots

also called **box and whisker** plots

the box:

- the centre line is the median
- the top of the box is the Q3
- the bottom of the box is the Q1

the whiskers (depends):

- the top of the top whisker is either the **max** value, or equal to the highest point that is below $Q_3 + 1.5*IQR$
- the bottom of the bottom whisker is either **min** value, or equal to the lowest point that is above $Q_1 - 1.5*IQR$
- in plots where the whiskers are *not* the min and max, the data points above and below the whiskers are the outliers

### Box plots in R

```r
ggplot(CS_dat, aes(y = cs_rate)) + #CS_dat is data set, aes is aesthetic where we want y variable to be the cs_rate
geom_boxplot() +
ylab("Cesarean delivery rate (%)") +
labs(title = "Box plot of the CS rates across US hospitals",
    caption = "Data from: Kozhimannil et al. 2013.") +
theme_minimal(base_size = 15) + # setting box sizes to 15
scale_x_continuo0us(labels = NULL) # removes the labels from the x-axis
```