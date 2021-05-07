--- 
title: "CPS in Japan"
author: "Keisuke Kawata"
date: "2021-05-08"
site: bookdown::bookdown_site
documentclass: book
bibliography: [book.bib, packages.bib]
biblio-style: apalike
link-citations: yes
description: "TBA"
---




# Summary

- Describe Japanese Labor market from 1986-2021.

- Use the [Labor force survey](https://www.stat.go.jp/english/data/roudou/index.html), which is open-access and includes similar variables as the current population survey in U.S.


<!--chapter:end:index.Rmd-->

# Simple description

## Environment


```r
library(data.table)
library(tidytable)
library(tidyverse)
library(lubridate)
```

## Data


```r
raw <- 
  fread("data/aggregate_time_series.csv") %>%
  mutate(date = ymd(date),
         employment = as.numeric(就業者)/as.numeric(`15歳以上人口`),
         sex = if_else(性別 == "男", "male", "female"))
```

## Employment rate

- Report $e_{g,m,y} = \frac{Employment_{g,m,y}}{Population_{g,m,y}}$, where $Employment_{g,m,y}$ and $Population_{g,m,y}$ are numbers of employment and population over 15 years old in month $m$, year $y$ and gender group $g$, respectively.


```r
raw %>%
  ggplot(aes(x = date,
             y = employment,
             color = sex)
         ) +
  geom_line() +
  theme_bw() +
  ylab("employment rate")
```

![](CPS_Japan_files/figure-epub3/unnamed-chunk-3-1.png)<!-- -->

## Year-to-year difference of employment rate

- Report change of employment rate $\tilde e_{g,m,y}=e_{g,m,y}-e_{g,m,y-1}$


```r
raw %>%
  mutate(year = year(date),
         month = month(date)
         ) %>%
  arrange(sex,
          month,
          year) %>%
  group_by(sex,
           month) %>%
  mutate(employment = employment - lag(employment)) %>%
  ungroup %>%
  filter(year >= 1969) %>%
  ggplot(aes(x = date,
             y = employment,
             color = sex)
         ) +
  geom_line() +
  geom_hline(yintercept = 0) +
  ylab("") +
  xlab("") +
  theme_bw() +
  theme(legend.position = "bottom")
```

![](CPS_Japan_files/figure-epub3/unnamed-chunk-4-1.png)<!-- -->


## Gender gap

- Report change of employment rate $\tilde e_{male,m,y} - \tilde e_{female,m,y}$


```r
raw %>%
  mutate(year = year(date),
         month = month(date)
         ) %>%
  arrange(sex,
          month,
          year) %>%
  group_by(sex,
           month) %>%
  mutate(employment = employment - lag(employment)) %>%
  ungroup %>%
  filter(year >= 1969) %>%
  arrange(date,
          sex) %>%
  group_by(date) %>%
  mutate(employment = employment - lag(employment)) %>%
  ungroup %>%
  filter(sex == "male") %>%
  ggplot(aes(x = date,
             y = employment)
         ) +
  geom_line() +
  geom_hline(yintercept = 0) +
  ylab("") +
  xlab("") +
  theme_bw()
```

![](CPS_Japan_files/figure-epub3/unnamed-chunk-5-1.png)<!-- -->


<!--chapter:end:01-intro.Rmd-->

# Simple description

## Environment


```r
library(data.table)
library(tidytable)
library(tidyverse)
library(lubridate)
```

## Data


```r
raw <- 
  fread("data/aggregate_time_series.csv") %>%
  mutate(date = ymd(date),
         employment = as.numeric(就業者)/as.numeric(`15歳以上人口`),
         sex = if_else(性別 == "男", "male", "female"))
```

## Employment rate

- Report $e_{g,m,y} = \frac{Employment_{g,m,y}}{Population_{g,m,y}}$, where $Employment_{g,m,y}$ and $Population_{g,m,y}$ are numbers of employment and population over 15 years old in month $m$, year $y$ and gender group $g$, respectively.


```r
raw %>%
  filter(date >= "2019-01-01") %>%
  ggplot(aes(x = date,
             y = employment,
             color = sex)
         ) +
  geom_line() +
  theme_bw() +
  ylab("employment rate")
```

![](CPS_Japan_files/figure-epub3/unnamed-chunk-8-1.png)<!-- -->

## Year-to-year difference of employment rate

- Report change of employment rate $\tilde e_{g,m,y}=e_{g,m,y}-e_{g,m,y-1}$


```r
raw %>%
  mutate(year = year(date),
         month = month(date)
         ) %>%
  arrange(sex,
          month,
          year) %>%
  group_by(sex,
           month) %>%
  mutate(employment = employment - lag(employment)) %>%
  ungroup %>%
  filter(year >= 2019) %>%
  ggplot(aes(x = date,
             y = employment,
             color = sex)
         ) +
  geom_line() +
  geom_point() +
  geom_hline(yintercept = 0) +
  ylab("") +
  xlab("") +
  theme_bw() +
  theme(legend.position = "bottom")
```

![](CPS_Japan_files/figure-epub3/unnamed-chunk-9-1.png)<!-- -->


## Gender gap

- Report change of employment rate $\tilde e_{male,m,y} - \tilde e_{female,m,y}$


```r
raw %>%
  mutate(year = year(date),
         month = month(date)
         ) %>%
  arrange(sex,
          month,
          year) %>%
  group_by(sex,
           month) %>%
  mutate(employment = employment - lag(employment)) %>%
  ungroup %>%
  filter(year >= 2019) %>%
  arrange(date,
          sex) %>%
  group_by(date) %>%
  mutate(employment = employment - lag(employment)) %>%
  ungroup %>%
  filter(sex == "male") %>%
  ggplot(aes(x = date,
             y = employment)
         ) +
  geom_line() +
  geom_point() +
  geom_hline(yintercept = 0) +
  ylab("") +
  xlab("") +
  theme_bw()
```

![](CPS_Japan_files/figure-epub3/unnamed-chunk-10-1.png)<!-- -->


<!--chapter:end:02-aftercovid.Rmd-->


# References {-}


<!--chapter:end:03-references.Rmd-->

