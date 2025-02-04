
Overview
This project explores the relationship between cocoa percentages and the countries of bean origin using a dataset of chocolate bars. Through geospatial analysis, we aim to understand how the percentage of cocoa content varies across different regions. The dataset provides insights into chocolate manufacturers, bean origins, ingredients, and taste profiles.

Dataset
The dataset is sourced from TidyTuesday and contains details about chocolate bars, including:

Manufacturer information

Country of bean origin

Cocoa percentage

Flavor characteristics

Ratings

Research Question:
How do cocoa percentages vary across different regions or countries of bean origin?

Approach
This analysis employs geospatial techniques to visualize cocoa percentage variations. We begin by:

Cleaning and preparing the data.

Selecting relevant columns: country_of_bean_origin, cocoa_percent, most_memorable_flavors, and geometry.

Computing the mean cocoa percentage per country.

Using ne_countries() to visualize cocoa percentage variations on a world map.

Creating a table to display mean cocoa percentage along with notable flavor characteristics.

Implementation
Data Preparation:
Convert cocoa_percent from a percentage string to a numeric decimal.

Compute the mean cocoa percentage per country (excluding blends).

Geospatial Analysis:
Merge the dataset with sf world map data.

Visualize cocoa percentage variations using a choropleth map with a color gradient.

Code Snippets:
Convert Cocoa Percentage to Numeric:
chocolate1 <- chocolate %>%
  mutate(cocoa_percent1 = as.numeric(sub("%", "", cocoa_percent)))
Compute Mean Cocoa Percentage per Country:
mean_perc <- chocolate1 %>%
  filter(country_of_bean_origin != 'Blend') %>%
  group_by(country_of_bean_origin) %>%
  summarize(
    mean_percent = mean(cocoa_percent1, na.rm = TRUE),
    most_memorable_characteristics = first(most_memorable_characteristics)
  ) %>%
  rename("sovereignt" = "country_of_bean_origin")
Create a Geospatial Map:
sf_world <- ne_countries(returnclass= 'sf')

sf_world2  <- left_join(sf_world, mean_perc) %>%
  select(sovereignt, mean_percent, geometry)

ggplot(sf_world2) + geom_sf(aes(fill = mean_percent)) +
  scale_fill_continuous_sequential(
    palette = "PuBu", rev = TRUE,
    limits = c(40, 85)
  )
Create a Table with Cocoa Percentage & Flavor Characteristics:
char_table <- mean_perc %>%
  select(mean_percent, most_memorable_characteristics) %>%
  arrange(mean_percent)
char_table
Findings & Discussion
Countries in Africa tend to have the lowest mean cocoa percentages, while those in the Americas and Asia have higher percentages.

Differences in cocoa varieties and growing conditions may explain this pattern.

Higher cocoa percentages are often associated with more bitter-tasting chocolates, as seen in the tibble data.

Conclusion
This analysis provides valuable insights into the regional variations in chocolate production. Further analysis could examine factors such as climate, bean varieties, and chocolate ratings.

Requirements
This project utilizes the following R libraries:

tidyverse

colorspace

tidytuesdayR

sf

rnaturalearth

ggplot2

knitr

How to Run the Code
Load the necessary libraries.

Read the dataset using readr::read_csv().

Follow the steps outlined in the implementation section to reproduce the analysis.

Author: [Your Name]
Date: [Project Date]
License: [Specify License, e.g., MIT]

