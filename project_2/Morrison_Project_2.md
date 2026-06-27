# Project 2

## Executive Summary
This repository contains an exploratory data visualization analysis using R. The project primarily investigates real estate data from the West Roxbury neighborhood in Boston, Massachusetts, alongside a spatial demonstration using Florida Lakes shapefiles.

## Motivation
The goal of this project is to apply principles of visual design, interactive graphing, statistical model visualization, and spatial mapping to real-world datasets. 

## Data Description
* **WestRoxbury.csv**: Contains property assessment data including Total Value, Living Area, Lot Size, and Remodel Status.
* **Florida_Lakes.zip**: Contains spatial shapefiles representing the perimeters of lakes in Florida.

## Summary of Findings
1. There is a strong, positive correlation between a property's Living Area and its Total Value.
2. Loving Area serves as the strongest statistical predictor of Total Value in a multiple linear regression model.
3. Interactive tooltips significantly enhance the readability of dense scatter plots, allowing users to isolate specific property metrics on demand.

## To Reproduce: 
To reproduce this project ensure that all of the required packages are installed, and then run every cell in the R-Notebook.

The file structure is as follows: 
 - Morrison_Project_2.rmd
 - Morrison_Project_2.html
 - Morrison_Project_2.md
 
The required Packages are: 
- tidyverse
- plotly
- broom
- sf
- ggthemes
- scales
- janitor

# Revised Report: 
## Original Plans & Data Preparation
Initially, the goal was to explore how structural attributes impact real estate values in West Roxbury. The necessary data preparation involved using the janitor package to standardize column names, ensuring there were no zero-values for living area, and converting text-based remodeling statuses into a cleaner binary category (“Remodeled” vs “No Remodel”). For the spatial aspect, a secondary dataset (Florida_Lakes.zip) was incorporated and required file extraction and sf geometric parsing before it could be mapped.

## Narrative & Difficulties
The primary story told by the plots is that a home’s total value is heavily dependent on its living area. The interactive scatter plot allows users to hover over individual properties to see specific metrics, while the coefficient plot provides a statistically rigorous view of what drives property value.
One minor difficulty encountered was balancing the scales of the coefficient plot; because the coefficiencts operate on different numeric scales, the estimates are drastically different in size. To address this issue, the data was standardized to ensure proper comparison

## Application of Design Principles
Throughout this notebook, data visualization best practices were applied in the following ways:

- Color Consistency - A distinct and accessible color palette (blues and reds) was used consistently across the charts to separate categories without overwhelming the viewer.
- Data-Ink Ratio - Heavy gridlines and unnecessary backgrounds were removed (using theme_minimal() and theme_map()) to ensure the focus remained on the data points.
- Clarity & Context - Tooltips were formatted with line breaks and dollar signs in the interactive plot, and standard numerical formatting (commas for thousands) was applied to the axes to ensure readability. Annotations were used strategically to guide the reader to the main takeaway in the model plot.
