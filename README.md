# UNICEF_Data---
title: "The Correlation Between Stunting and Life Expectancy: An Economical Perspective"
author: "Rushikesh Bodkhe"
format:
  html:
    embed-resources: true
    code-fold: true
    theme: cosmo
    toc: true
editor: visual
---

## BAA1030 Data Analytics & Story Telling

**Student Name:** Rushikesh Bodkhe  
**Student Number:** 47897  
**Program:** MSc Business Management

## Executive Summary

This report explores the global patterns of childhood stunting and its correlation with life expectancy, offering an economical perspective. Childhood stunting, a significant public health concern, is not only a reflection of chronic malnutrition but also strongly associated with reduced life expectancy. Through data analysis and visualizations, this report identifies countries with high stunting rates and investigates how these rates may economically impact a nation's longevity and development.

## Introduction

Stunting—defined as children having a height-for-age more than two standard deviations below the WHO standard—remains a critical challenge for both public health and economic development. It reflects inadequate early life nutrition and care, and contributes to reduced cognitive development, poorer health outcomes, and shorter life expectancy. This report uses UNICEF's data to explore the relationship between stunting prevalence and life expectancy across countries, offering insights into how tackling stunting could enhance human capital and socio-economic growth.

## Data Preparation

```{python}
import pandas as pd
import plotnine as p9

# Load datasets
indicator = pd.read_csv("unicef_indicator_2.csv")
metadata = pd.read_csv("unicef_metadata.csv")

# Filter the required indicator
df = indicator[indicator["indicator"] == "Height-for-age <-2 SD (stunting), Modeled Estimates"]

# Merge with metadata
df = df.merge(metadata, on="country", how="left")
```

## Basic Exploratory Data Analysis (EDA)

```{python}
# Check basic information
print(df.describe())

# Top 10 countries with highest stunting
top10 = df.groupby('country')['obs_value'].mean().sort_values(ascending=False).head(10)
print(top10)

# Check correlation between stunting and life expectancy
correlation = df[['obs_value', 'life_expectancy']].dropna().corr()
print(correlation)
```

*The correlation table above explores how child stunting relates to life expectancy.*

## Visualization 1: World Map Chart

```{python}
# Plot a world map of average stunting rates
world_avg = df.groupby('country')['obs_value'].mean().reset_index()

(p9.ggplot(world_avg, p9.aes(x='country', y='obs_value')) +
 p9.geom_col(fill='skyblue') +
 p9.coord_flip() +
 p9.theme(axis_text_x=p9.element_text(rotation=90)) +
 p9.labs(title='Average Stunting Rate by Country', x='Country', y='Stunting Rate (%)'))
```

*This chart shows the average stunting rates per country, highlighting regions most impacted.*

## Visualization 2: Bar Chart - Top 10 Countries

```{python}
# Top 10 stunting rates
(p9.ggplot(top10.reset_index(), p9.aes(x='country', y='obs_value')) +
 p9.geom_bar(stat='identity', fill='salmon') +
 p9.coord_flip() +
 p9.labs(title='Top 10 Countries by Average Stunting Rate', x='Country', y='Stunting Rate (%)'))
```

*This bar chart highlights the countries with the highest levels of child stunting.*

## Visualization 3: Scatterplot with Linear Regression (Year vs Stunting)

```{python}
# Scatterplot of year vs stunting rate
(p9.ggplot(df, p9.aes(x='year', y='obs_value')) +
 p9.geom_point(alpha=0.3) +
 p9.geom_smooth(method='lm', color='blue') +
 p9.labs(title='Trend of Stunting Rates Over Years', x='Year', y='Stunting Rate (%)'))
```

*The scatterplot with regression line reveals the general trend in stunting rates over time.*

## Visualization 4: Time Series Chart

```{python}
# Average stunting rate per year
yearly = df.groupby('year')['obs_value'].mean().reset_index()

(p9.ggplot(yearly, p9.aes(x='year', y='obs_value')) +
 p9.geom_line(color='green') +
 p9.geom_point(color='red') +
 p9.labs(title='Global Average Stunting Rate Over Time', x='Year', y='Stunting Rate (%)'))
```

*This line chart shows how the global average of child stunting rates has changed year by year.*

## Visualization 5: Stunting vs Life Expectancy Scatterplot

```{python}
# Scatterplot stunting vs life expectancy
(p9.ggplot(df.dropna(), p9.aes(x='obs_value', y='life_expectancy')) +
 p9.geom_point(color='purple', alpha=0.6) +
 p9.geom_smooth(method='lm', color='black') +
 p9.labs(title='Correlation between Stunting and Life Expectancy', x='Stunting Rate (%)', y='Life Expectancy (Years)'))
```

*This scatterplot visualizes the negative relationship between stunting rates and life expectancy.*

## Conclusion

Through this analysis, we observe that higher childhood stunting rates are associated with lower life expectancy across countries. Although global stunting averages show some improvements over time, stunting continues to threaten long-term economic growth and human development. Targeted policies aiming at reducing stunting can significantly enhance not only individual wellbeing but also national prosperity.
