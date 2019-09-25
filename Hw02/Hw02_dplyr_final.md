---
title: "Assignment 2"
author: "Linda Dumalo"
date: "24/09/2019"
output:
  pdf_document: default
  html_document:
    keep_md: yes
--- 

```{r setup, include=FALSE, message=F}

library(gapminder)
library(tidyverse)
library(ggplot2)
library(knitr)
knitr::kable 
``` 

## **Exercise 1: Basic dplyr**

#### Exercise 1.2 

Use the pipe operator %>% to select “country” and “gdpPercap” from your filtered dataset in 1.1 

```{r}
gapminder %>% 
  filter(country == "Australia" | country == "Canada"| country == "Germany") %>%  
  filter(between(year, 1970, 1980))
``` 
  
#### Exercise 1.3 

Filter gapminder to all entries that have experienced a drop in life expectancy. 

delta_lifeExp is the change in life exptancy from 3 years prior to the the value in the year column. 

``` {r} 

gapminder %>% 
  mutate(lifeExp, lag_lifeExp = lag(lifeExp)) %>% 
  mutate(lifeExp, delta_lifeExp = lifeExp - lag_lifeExp) %>%
  select(country, year, delta_lifeExp) %>%
  drop_na(delta_lifeExp)
  
```

The data was then sorted to display the countries that experienced the largest drop in life expectancy from 2002 to 2007 at the top of the table. 

``` {r}
gapminder %>% 
  mutate(lifeExp, lag_lifeExp = lag(lifeExp)) %>% 
  mutate(lifeExp, delta_lifeExp = lifeExp - lag_lifeExp) %>%
  filter(delta_lifeExp < 0) %>%
  arrange(-desc(delta_lifeExp)) %>% 
  select(country, year, delta_lifeExp) %>%
  drop_na(delta_lifeExp)
```

``` {r}
gapminder %>% 
  mutate(lifeExp, lag_lifeExp = lag(lifeExp)) %>% 
  mutate(lifeExp, delta_lifeExp = lifeExp - lag_lifeExp) %>%
   filter(year == 2007) %>%
  arrange(-desc(delta_lifeExp)) %>% 
  select(country,delta_lifeExp) %>%
  drop_na(delta_lifeExp) %>%
  rename(delta_lifeExp_2002_2007 = delta_lifeExp)
```

### Exercise 1.4 

Filter gapminder so that it shows the max GDP per capita experienced by each country. 

``` {r}
gapminder %>% 
  group_by(country) %>%
  filter(gdpPercap == max(gdpPercap)) %>% 
  select(country, year, gdpPercap) %>%
  rename(Max_GDPperCap = gdpPercap)

```

### Exercise 1.5 

Produce a scatterplot of Canada’s life expectancy vs. GDP per capita using ggplot()

```{r}
gapminder %>%
  filter(country == "Canada") %>%
  ggplot(aes(gdpPercap, lifeExp)) + 
  geom_point() + 
  scale_x_log10() + 
  xlab("GDP per Capita") + 
  ylab("Life Exptectancy")  
  # theme_bw was not working!
  # wanted to add labels corresonding to the year for each point
``` 

## **Exercise 2**

### Categorical Variable Exploration 

Exploring the GDP per capita (gdpPercap) for the countries in Asia for the year 2007. The GDP per capita is stated in USD. 

**Maximum GDP Per Capita**

``` {r}
Maxgdp <- gapminder %>%  # was, trying to show the higest and the lowest gdppercap in 1 table, realize that this likely will not work using summarize...  
  filter(continent == 'Asia') %>%
  filter(year == 2007) %>%
  summarize(max_gdpPercap = max(gdpPercap), country = country[gdpPercap == max_gdpPercap]) %>%
  select(country, max_gdpPercap) %>%
  top_n(1) 
```

**Minimum GDP Per Capita**

``` {r}
Mingdp <- gapminder %>%  # was, trying to show the higest and the lowest gdppercap 
  select(country, continent, year, gdpPercap) %>%
  filter(continent == 'Asia') %>%
  filter(year == 2007) %>%
  summarize(min_gdpPercap = min(gdpPercap), country = country[gdpPercap == min_gdpPercap]) %>%
  select(country, min_gdpPercap) %>%
  top_n(-1)  
```

**Minimum and Maximum GDP per capita in 2007 in Asia**  

Minimum GDP per capitat in 2007 was **Myanmar** at $ 944  USD and the maximum GDP per capita was **Kuwait** with $ 47307. 

Ideally, would have plotted these into a bar graph or something 

## **Exercise 3: Explore various plot type**

Instead of focusing on the year 2007, the GDP per capita in the countries of Asia will be analyzed from 1987 to 2007 For the countries with the top 10 GDP 

```{r}
gapminder %>% 
  filter(continent == 'Asia') %>%
  filter(between(year,1987,2007)) %>% 
  group_by(country) %>% 
  group_by(gdpPercap) %>%
  arrange(desc(gdpPercap)) %>%
  ungroup() %>% 
  slice(1:40) %>% # trying to narrow down to the countries with the top 10 highest GDP per Capita  
  ggplot(aes(country, gdpPercap)) +
  geom_boxplot() +
  xlab("Country") +
  theme(axis.text.x = element_text(angle = 45, hjust = 1)) +
  # theme(axis.text.x = element_text(angle = 45, hjust = 1))
  ylab("GDP per Capita") 
  # not pretty... 
```

## Exploring a Dfferent dataset 

Source for description of data:
https://stat.ethz.ch/R-manual/R-devel/library/datasets/html/beavers.html 

#### Description of Dataframe 

Reynolds (1994) describes a small part of a study of the long-term temperature dynamics of beaver *Castor canadensis* in north-central Wisconsin. Body temperature was measured by telemetry every 10 minutes for females. The data used corresponds to 1 day in December, starting at 8:40 am (840) to 23:50 (2350). Temperatures are in degree celsius.

``` {r} 
beaverday <- data.frame(beaver1) %>% 
  slice(1:91)
```

``` {r}
ggplot(beaverday, aes(time, temp)) + 
  geom_point(aes(time, temp)) + 
  geom_point(colour = "blue", alpha = 0.8) +
  theme_bw()
```

Scatter plot to show the relationship between inactivity (0) and activity (1). 

``` {r}
ggplot(beaverday, aes(activ, temp)) + 
  geom_point(aes(activ, temp)) + 
  geom_point(colour = "blue", alpha = 0.8) +
  theme_bw()
```

### **Bonus** 

Original code:

```{r}
filter(gapminder, country == c("Rwanda", "Afghanistan"))
```

Presumably the analyst’s intent was to get the data for Rwanda and Afghanistan however it appears that there are some rows missing. 
 
```{r}
gapminder %>%
    filter(country == "Afghanistan" | country == "Rwanda") 
```
