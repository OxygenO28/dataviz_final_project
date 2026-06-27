# Project 1

## Executive Summary
This project is a graphical mapping of reported rat sightings in New York City. 

## Motivation
The goal of this project is to apply principles of visual design, interactive graphing, statistical model visualization, and spatial mapping to real-world datasets. 

## Dataset Description: 
* **rats_nyc.csv**: Contains information about New York City rat sightings, including various variables such as longitude, latitude, date sighted, borough, and the date that the sightings must be addressed by, and many more. 

## Summary of findings: 
This project has found that the majority of the rat sightings occur in Brooklyn, with Manhattan and the Brox coming in second and third respectively. Staten island was the borough with the least number of rat sightings, and Queens had the second least number of reported rat sightings. 

## To reproduce: 
To reproduce this project ensure that all of the required packages are installed, and then run every cell in the R-Notebook.

The file structure is as follows: 
 - Morrison_Project_1.rmd
 - Morrison_Project_1.html
 - Morrison_Project_1.md

The required packages are: 
- tidyverse
- ggplot2
- dplyr
- lubridate
- sf

# Revised Report: 

## Original Plans & Data Preparation
Originally, I had planned to do some more basic visualizations, keeping somewhat inline with the examples provided in the assignment. However, when I was looking at the data I realized it had longitude and latitude coordinates, which would let me do my favorite type of graph: Map graphs.

## Narrative & Difficulties
With the graphs that I have made, you could tell a pretty interesting story on rat distribution in New York. For example, Brooklyn and Manhattan have by far the most reported rat sightings. However, as per the data, there are entire sections of each borough that have zero reported rat sightings. In each borough, these areas of no-rat-reportings generally correspond with islands. However, even removing those islands from consideration, there are still sections with no rat reportings near areas with rat reportings. One such area is smack-dab right in the middle of Manhattan. I personally feel that further analysis with further data, specifically human population data, you could create a very interesting story.

## Application of Design Principles
Throughout this notebook, data visualization best practices were applied in the following ways:

- Color Consistency - A distinct and accessible color palette (blues and reds) was used consistently across the charts to separate categories without overwhelming the viewer.
- Data-Ink Ratio - Heavy gridlines and unnecessary backgrounds were removed (using theme_minimal() and theme_map()) to ensure the focus remained on the data points.