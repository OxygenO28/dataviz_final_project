---
title: "Data Visualization for Exploratory Data Analysis"
output: 
  html_document:
    keep_md: true
    toc: true
    toc_float: true
---



# Data Visualization Project 03


In this exercise you will explore methods to create different types of data visualizations (such as plotting text data, or exploring the distributions of continuous variables).


## PART 1: Density Plots

Using the dataset obtained from FSU's [Florida Climate Center](https://climatecenter.fsu.edu/climate-data-access-tools/downloadable-data), for a station at Tampa International Airport (TPA) for 2022, attempt to recreate the charts shown below which were generated using data from 2016. You can read the 2022 dataset using the code below: 


``` r
library(tidyverse)
library(lubridate)
library(plotly)
library(ggridges)
library(viridis)
library(tidytext)
library(ggwordcloud)
weather_tpa <- read_csv("https://raw.githubusercontent.com/aalhamadani/datasets/master/tpa_weather_2022.csv")
# random sample 
sample_n(weather_tpa, 4)
```

```
## # A tibble: 4 × 7
##    year month   day precipitation max_temp min_temp ave_temp
##   <dbl> <dbl> <dbl>         <dbl>    <dbl>    <dbl>    <dbl>
## 1  2022     4    20          0          85       61       73
## 2  2022     8     3          0.98       92       76       84
## 3  2022    12    18          0.04       69       51       60
## 4  2022    12    11          0          70       64       67
```


``` r
weather_tpa$month <- lubridate::month(weather_tpa$month, label = TRUE, abbr = FALSE)

sample_n(weather_tpa, 4)
```

```
## # A tibble: 4 × 7
##    year month      day precipitation max_temp min_temp ave_temp
##   <dbl> <ord>    <dbl>         <dbl>    <dbl>    <dbl>    <dbl>
## 1  2022 November    25       0.00001       82       68     75  
## 2  2022 December     2       0             81       62     71.5
## 3  2022 July        30       0             97       81     89  
## 4  2022 April       24       0             90       69     79.5
```


See Slides from Week 4 of Visualizing Relationships and Models (slide 10) for a reminder on how to use this type of dataset with the `lubridate` package for dates and times (example included in the slides uses data from 2016).

Using the 2022 data: 

(a) Create a plot like the one below:

<img src="https://raw.githubusercontent.com/aalhamadani/dataviz_final_project/main/figures/tpa_max_temps_facet.png" alt="" width="80%" style="display: block; margin: auto;" />


``` r
ggplot(weather_tpa, aes(x = max_temp, fill = month)) +
  geom_histogram(binwidth = 3, color = "white") +
  facet_wrap(~month) +
  theme_bw() +
  theme(legend.position = "none") +
  labs(x = "Maximum temperatures", 
       y = "Number of Days")
```

<img src="Morrison_project_03_files/figure-html/unnamed-chunk-4-1.png" alt="A multi-panel figure featuring 12 individual histograms, one for each month of the year, illustrating the frequency of daily maximum temperatures. Each panel displays the 'Number of Days' on the y-axis and 'Maximum temperatures' on the x-axis, ranging from 50 to 100. The histograms are color-coded, transitioning from dark purple for early months to green and yellow for later months, highlighting how the distribution of daily maximum temperatures shifts throughout the year." style="display: block; margin: auto;" />


Hint: the option `binwidth = 3` was used with the `geom_histogram()` function.

(b) Create a plot like the one below:

<img src="https://raw.githubusercontent.com/aalhamadani/dataviz_final_project/main/figures/tpa_max_temps_density.png" alt="" width="80%" style="display: block; margin: auto;" />


``` r
weather_tpa %>%
  filter(max_temp <= 99.9) %>%
  ggplot(aes(x = max_temp)) +
    geom_density(bw = 0.5, kernel = "biweight", fill = "grey50", alpha = 1, linewidth = 1, lineend = "round") +
    theme_minimal() +
    labs(x = "Maximum Temperature", 
         y = "Density")
```

<img src="Morrison_project_03_files/figure-html/unnamed-chunk-6-1.png" alt="A density plot showing the distribution of maximum temperatures. The x-axis represents 'Maximum Temperature,' ranging from approximately 45 to just under 100, while the y-axis represents 'Density.' The plot is filled with a solid grey color and displays a multimodal distribution, featuring several peaks with the highest density concentrated between 80 and 98 degrees." style="display: block; margin: auto;" />


Hint: check the `kernel` parameter of the `geom_density()` function, and use `bw = 0.5`.

(c) Create a plot like the one below:

<img src="https://raw.githubusercontent.com/aalhamadani/dataviz_final_project/main/figures/tpa_max_temps_density_facet.png" alt="" width="80%" style="display: block; margin: auto;" />


``` r
ggplot(weather_tpa, aes(x = max_temp, fill = month,  alpha = 0.8)) +
  geom_density(linewidth = 1) +
  facet_wrap(~month) +
  theme_bw() +
  theme(legend.position = "none") +
  labs(x = "Maximum temperatures", 
       y = "density")
```

<img src="Morrison_project_03_files/figure-html/unnamed-chunk-8-1.png" alt="A multi-panel figure containing 12 individual density plots, one for each month of the year, showing the distribution of daily maximum temperatures. Each panel features an x-axis labeled 'Maximum temperatures' ranging from 50 to 100 and a y-axis labeled 'density' ranging from 0.0 to 0.3. The density curves shift across the x-axis for different months, illustrating the seasonal variation in temperatures, with the color of the plot area transitioning from shades of purple in the early months to green and yellow in the later months" style="display: block; margin: auto;" />


Hint: default options for `geom_density()` were used. 

(d) Generate a plot like the chart below:


<img src="https://raw.githubusercontent.com/aalhamadani/dataviz_final_project/main/figures/tpa_max_temps_ridges_plasma.png" alt="" width="80%" style="display: block; margin: auto;" />


``` r
ggplot(weather_tpa, aes(x = max_temp, y = fct_rev(month), fill = stat(x))) +
  geom_density_ridges_gradient(quantile_lines = TRUE, quantiles = 2, scale = 3, linewidth = 1) +
  scale_fill_viridis_c(name = " ", option = "plasma") +
  theme_minimal(base_size = 18) +
  labs(x = "Maximum Temperature (in Fahrenheit Degrees)", y = " ")
```

<img src="Morrison_project_03_files/figure-html/unnamed-chunk-10-1.png" alt="A ridgeline plot, illustrating the distribution of daily maximum temperatures for every month of the year. The x-axis represents the temperature in degrees Fahrenheit, ranging from 40 to over 100, while the y-axis lists the months from January through December. Each month is represented by a density distribution curve filled with a gradient color scale: purple for lower temperatures, transitioning to orange and yellow for higher temperatures. Vertical black lines within each curve denote a median temperature value for that specific month" style="display: block; margin: auto;" />


Hint: use the`{ggridges}` package, and the `geom_density_ridges()` function paying close attention to the `quantile_lines` and `quantiles` parameters. The plot above uses the `plasma` option (color scale) for the _viridis_ palette.


(e) Create a plot of your choice that uses the attribute for precipitation _(values of -99.9 for temperature or -99.99 for precipitation represent missing data)_.


``` r
weather_tpa %>%
  filter(precipitation != -99.99) %>%
  plot_ly(
    x = ~as.factor(month),
    y = ~precipitation,
    color = ~as.factor(month),
    type = "box",
    boxpoints = "outliers",
    marker = list(color = "red")
  ) %>%
  layout(
    title = "Precipitation Distribution at TPA by Month (2022)",
    xaxis = list(title = "Month"),
    yaxis = list(title = "Precipitation (inches)"),
    showlegend = FALSE
  )
```

```{=html}
<div class="plotly html-widget html-fill-item" id="htmlwidget-58662689bc1828511dbe" style="width:672px;height:480px;"></div>
<script type="application/json" data-for="htmlwidget-58662689bc1828511dbe">{"x":{"visdat":{"87b492d1522":["function () ","plotlyVisDat"]},"cur_data":"87b492d1522","attrs":{"87b492d1522":{"x":{},"y":{},"boxpoints":"outliers","marker":{"color":"red"},"color":{},"alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"box"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"title":"Precipitation Distribution at TPA by Month (2022)","xaxis":{"domain":[0,1],"automargin":true,"title":"Month","type":"category","categoryorder":"array","categoryarray":["January","February","March","April","May","June","July","August","September","October","November","December"]},"yaxis":{"domain":[0,1],"automargin":true,"title":"Precipitation (inches)"},"showlegend":false,"hovermode":"closest"},"source":"A","config":{"modeBarButtonsToAdd":["hoverclosest","hovercompare"],"showSendToCloud":false},"data":[{"fillcolor":"rgba(253,231,37,0.5)","x":["December","December","December","December","December","December","December","December","December","December","December","December","December","December","December","December","December","December","December","December","December","December","December","December","December","December","December","December","December","December","December"],"y":[0,0,0,0,0,0,0,0,0,0,0,0,0,0,1.0900000000000001,0.050000000000000003,0.02,0.040000000000000001,0,0.11,0.59999999999999998,0.14999999999999999,1.0000000000000001e-05,0,0,0,0,0,0,0,0.28999999999999998],"boxpoints":"outliers","marker":{"color":"red","line":{"color":"rgba(253,231,37,1)"}},"type":"box","name":"December","line":{"color":"rgba(253,231,37,1)"},"xaxis":"x","yaxis":"y","frame":null},{"fillcolor":"rgba(194,223,35,0.5)","x":["November","November","November","November","November","November","November","November","November","November","November","November","November","November","November","November","November","November","November","November","November","November","November","November","November","November","November","November","November","November"],"y":[0,0,0.90000000000000002,0,1.0000000000000001e-05,1.0000000000000001e-05,0,0.059999999999999998,0.089999999999999997,2.46,0.76000000000000001,0.01,0.01,0,0,0.14000000000000001,0,0,0,0.39000000000000001,1.0000000000000001e-05,0,0.050000000000000003,0,1.0000000000000001e-05,0,0.27000000000000002,0,0,0.040000000000000001],"boxpoints":"outliers","marker":{"color":"red","line":{"color":"rgba(194,223,35,1)"}},"type":"box","name":"November","line":{"color":"rgba(194,223,35,1)"},"xaxis":"x","yaxis":"y","frame":null},{"fillcolor":"rgba(133,213,74,0.5)","x":["October","October","October","October","October","October","October","October","October","October","October","October","October","October","October","October","October","October","October","October","October","October","October","October","October","October","October","October","October","October","October"],"y":[0,0,0,0,0,0,0,0,0,1.0000000000000001e-05,1.0000000000000001e-05,0.02,0.29999999999999999,0,0,0,1.0000000000000001e-05,0.01,0,0,0,0,0,0,0,0,0.059999999999999998,0.70999999999999996,0,0,0],"boxpoints":"outliers","marker":{"color":"red","line":{"color":"rgba(133,213,74,1)"}},"type":"box","name":"October","line":{"color":"rgba(133,213,74,1)"},"xaxis":"x","yaxis":"y","frame":null},{"fillcolor":"rgba(81,197,106,0.5)","x":["September","September","September","September","September","September","September","September","September","September","September","September","September","September","September","September","September","September","September","September","September","September","September","September","September","September","September","September","September","September"],"y":[2.7400000000000002,1.4099999999999999,0.40000000000000002,0,0,0,0.10000000000000001,1.6399999999999999,0.13,1.0900000000000001,0.02,0.029999999999999999,0.029999999999999999,0.059999999999999998,0,0.87,0.059999999999999998,0,0.02,0.070000000000000007,1.0000000000000001e-05,0,1.0700000000000001,0,0,1.0000000000000001e-05,0.080000000000000002,2.4700000000000002,0,0],"boxpoints":"outliers","marker":{"color":"red","line":{"color":"rgba(81,197,106,1)"}},"type":"box","name":"September","line":{"color":"rgba(81,197,106,1)"},"xaxis":"x","yaxis":"y","frame":null},{"fillcolor":"rgba(43,176,127,0.5)","x":["August","August","August","August","August","August","August","August","August","August","August","August","August","August","August","August","August","August","August","August","August","August","August","August","August","August","August","August","August","August","August"],"y":[0,1.0000000000000001e-05,0.97999999999999998,1.24,1.0000000000000001e-05,0.040000000000000001,0.23999999999999999,0.059999999999999998,1.28,0,0,0,0,1.0000000000000001e-05,1.0000000000000001e-05,0,0.64000000000000001,0,0,0.46999999999999997,0.45000000000000001,0,0,0.60999999999999999,0.31,1.0000000000000001e-05,0.02,0.16,0,0,0.01],"boxpoints":"outliers","marker":{"color":"red","line":{"color":"rgba(43,176,127,1)"}},"type":"box","name":"August","line":{"color":"rgba(43,176,127,1)"},"xaxis":"x","yaxis":"y","frame":null},{"fillcolor":"rgba(30,155,138,0.5)","x":["July","July","July","July","July","July","July","July","July","July","July","July","July","July","July","July","July","July","July","July","July","July","July","July","July","July","July","July","July","July","July"],"y":[1.0000000000000001e-05,1.0000000000000001e-05,0,1.0000000000000001e-05,0.01,0.050000000000000003,0.029999999999999999,0.19,0,0.87,0.63,0,0,2.0800000000000001,0,2.8599999999999999,0.78000000000000003,0,0.02,0,0,1.6399999999999999,1.6899999999999999,0,0.02,1.1200000000000001,0,0,0,0,0],"boxpoints":"outliers","marker":{"color":"red","line":{"color":"rgba(30,155,138,1)"}},"type":"box","name":"July","line":{"color":"rgba(30,155,138,1)"},"xaxis":"x","yaxis":"y","frame":null},{"fillcolor":"rgba(37,133,142,0.5)","x":["June","June","June","June","June","June","June","June","June","June","June","June","June","June","June","June","June","June","June","June","June","June","June","June","June","June","June","June","June","June"],"y":[1.0000000000000001e-05,1.6000000000000001,1.0000000000000001e-05,0.46999999999999997,0,0,0.16,0,0.10000000000000001,0.01,2.8100000000000001,0,0,0,0.11,1.0000000000000001e-05,0,0,0.040000000000000001,0.10000000000000001,0,0,0,0.19,0,0,1.28,0.089999999999999997,0.040000000000000001,1.0700000000000001],"boxpoints":"outliers","marker":{"color":"red","line":{"color":"rgba(37,133,142,1)"}},"type":"box","name":"June","line":{"color":"rgba(37,133,142,1)"},"xaxis":"x","yaxis":"y","frame":null},{"fillcolor":"rgba(45,112,142,0.5)","x":["May","May","May","May","May","May","May","May","May","May","May","May","May","May","May","May","May","May","May","May","May","May","May","May","May","May","May","May","May","May","May"],"y":[0.029999999999999999,0,0.62,0,0,0,0.14999999999999999,0,0,0,0,0,0,0,0,0,0,0,0,0.17000000000000001,1.0000000000000001e-05,0,1.0000000000000001e-05,0.01,0,0,0,0,0,1.6499999999999999,0.080000000000000002],"boxpoints":"outliers","marker":{"color":"red","line":{"color":"rgba(45,112,142,1)"}},"type":"box","name":"May","line":{"color":"rgba(45,112,142,1)"},"xaxis":"x","yaxis":"y","frame":null},{"fillcolor":"rgba(56,89,140,0.5)","x":["April","April","April","April","April","April","April","April","April","April","April","April","April","April","April","April","April","April","April","April","April","April","April","April","April","April","April","April","April","April"],"y":[1.3700000000000001,1.1100000000000001,0,0,0,0,1.3400000000000001,0,0,0,0,0,0,0,0.02,0,0,0,0,0,0,0,0,0,0,0,0,0.14000000000000001,1.22,1.5600000000000001],"boxpoints":"outliers","marker":{"color":"red","line":{"color":"rgba(56,89,140,1)"}},"type":"box","name":"April","line":{"color":"rgba(56,89,140,1)"},"xaxis":"x","yaxis":"y","frame":null},{"fillcolor":"rgba(67,62,133,0.5)","x":["March","March","March","March","March","March","March","March","March","March","March","March","March","March","March","March","March","March","March","March","March","March","March","March","March","March","March","March","March","March","March"],"y":[0,0,0,0,0,0,0,0,0,1.0000000000000001e-05,0,0.40999999999999998,0,0,1.04,1.0000000000000001e-05,0,0,0,0,0,0,0,1.4199999999999999,0,0,0,0,0,0,0.040000000000000001],"boxpoints":"outliers","marker":{"color":"red","line":{"color":"rgba(67,62,133,1)"}},"type":"box","name":"March","line":{"color":"rgba(67,62,133,1)"},"xaxis":"x","yaxis":"y","frame":null},{"fillcolor":"rgba(72,33,115,0.5)","x":["February","February","February","February","February","February","February","February","February","February","February","February","February","February","February","February","February","February","February","February","February","February","February","February","February","February","February","February"],"y":[0,0,0,0,0.02,0.02,0,0.34999999999999998,1.0000000000000001e-05,0,0,1.0000000000000001e-05,0.10000000000000001,0,0,0,0,0,0.13,0,0,0,0,0,0,0,0,0],"boxpoints":"outliers","marker":{"color":"red","line":{"color":"rgba(72,33,115,1)"}},"type":"box","name":"February","line":{"color":"rgba(72,33,115,1)"},"xaxis":"x","yaxis":"y","frame":null},{"fillcolor":"rgba(68,1,84,0.5)","x":["January","January","January","January","January","January","January","January","January","January","January","January","January","January","January","January","January","January","January","January","January","January","January","January","January","January","January","January","January","January","January"],"y":[0,0,0.02,0,0,1.0000000000000001e-05,1.0000000000000001e-05,0,0,0,0,0,0,0,0,0.68000000000000005,0,0,0,0,0,0.01,1.0000000000000001e-05,0,0.67000000000000004,0.080000000000000002,0,1.0000000000000001e-05,0,0,0],"boxpoints":"outliers","marker":{"color":"red","line":{"color":"rgba(68,1,84,1)"}},"type":"box","name":"January","line":{"color":"rgba(68,1,84,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.20000000000000001,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
```



## PART 2 

> **You can choose to work on either Option (A) or Option (B)**. Remove from this template the option you decided not to work on. 


### Option (A): Visualizing Text Data

Review the set of slides (and additional resources linked in it) for visualizing text data: Week 6 PowerPoint slides of Visualizing Text Data. 

Choose any dataset with text data, and create at least one visualization with it. For example, you can create a frequency count of most used bigrams, a sentiment analysis of the text data, a network visualization of terms commonly used together, and/or a visualization of a topic modeling approach to the problem of identifying words/documents associated to different topics in the text data you decide to use. 

Make sure to include a copy of the dataset in the `data/` folder, and reference your sources if different from the ones listed below:

- [Billboard Top 100 Lyrics](https://raw.githubusercontent.com/aalhamadani/dataviz_final_project/main/data/BB_top100_2015.csv)

- [RateMyProfessors comments](https://raw.githubusercontent.com/aalhamadani/dataviz_final_project/main/data/rmp_wit_comments.csv)

- [FL Poly News Articles](https://raw.githubusercontent.com/aalhamadani/dataviz_final_project/main/data/flpoly_news_SP23.csv)


(to get the "raw" data from any of the links listed above, simply click on the `raw` button of the GitHub page and copy the URL to be able to read it in your computer using the `read_csv()` function)


``` r
billboard_data <- "../data/BB_top100_2015.csv"
billboard <- read.csv(billboard_data)
head(billboard)
```

```
##   Rank              Song                             Artist Year
## 1    1       uptown funk   mark ronson featuring bruno mars 2015
## 2    2 thinking out loud                         ed sheeran 2015
## 3    3     see you again wiz khalifa featuring charlie puth 2015
## 4    4        trap queen                          fetty wap 2015
## 5    5             sugar                           maroon 5 2015
## 6    6 shut up and dance                      walk the moon 2015
##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          Lyrics
## 1 this hit that ice cold michelle pfeiffer that white gold this one for them hood girls them good girls straight masterpieces stylin whilen livin it up in the city got chucks on with saint laurent got kiss myself im so prettyim too hot hot damn called a police and a fireman im too hot hot damn make a dragon wanna retire man im too hot hot damn say my name you know who i am im too hot hot damn am i bad bout that money break it downgirls hit your hallelujah whoo girls hit your hallelujah whoo girls hit your hallelujah whoo cause uptown funk gon give it to you cause uptown funk gon give it to you cause uptown funk gon give it to you saturday night and we in the spot dont believe me just watch come ondont believe me just watch uhdont believe me just watch dont believe me just watch dont believe me just watch dont believe me just watch hey hey hey oh    meaning  byamandah   editor    70s girl group the sequence accused bruno mars and producer mark ronson of ripping their sound off in uptown funk their song in question is funk you    see all   stop wait a minute fill my cup put some liquor in it take a sip sign a check julio get the stretch ride to harlem hollywood jackson mississippi if we show up we gon show out smoother than a fresh jar of skippyim too hot hot damn called a police and a fireman im too hot hot damn make a dragon wanna retire man im too hot hot damn bitch say my name you know who i am im too hot hot damn am i bad bout that money break it downgirls hit your hallelujah whoo girls hit your hallelujah whoo girls hit your hallelujah whoo cause uptown funk gon give it to you cause uptown funk gon give it to you cause uptown funk gon give it to you saturday night and we in the spot dont believe me just watch come ondont believe me just watch uhdont believe me just watch uh dont believe me just watch uh dont believe me just watch dont believe me just watch hey hey hey ohbefore we leave lemmi tell yall a lil something uptown funk you up uptown funk you up uptown funk you up uptown funk you up uh i said uptown funk you up uptown funk you up uptown funk you up uptown funk you upcome on dance jump on it if you sexy then flaunt it if you freaky then own it dont brag about it come show mecome on dance jump on it if you sexy then flaunt it well its saturday night and we in the spot dont believe me just watch come ondont believe me just watch uhdont believe me just watch uh dont believe me just watch uh dont believe me just watch dont believe me just watch hey hey hey ohuptown funk you up uptown funk you up say what uptown funk you up uptown funk you up uptown funk you up uptown funk you up say what uptown funk you up uptown funk you up uptown funk you up uptown funk you up say what uptown funk you up uptown funk you up uptown funk you up uptown funk you up say what uptown funk you up
## 2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              when your legs dont work like they used to before and i cant sweep you off of your feet will your mouth still remember the taste of my love will your eyes still smile from your cheeksand darling i will be loving you til were 70 and baby my heart could still fall as hard at 23 and im thinking bout how people fall in love in mysterious ways maybe just the touch of a hand oh me i fall in love with you every single day and i just wanna tell you i amso honey now take me into your loving arms kiss me under the light of a thousand stars place your head on my beating heart im thinking out loud maybe we found love right where we arewhen my hairs all but gone and my memory fades and the crowds dont remember my name when my hands dont play the strings the same way mm i know you will still love me the samecause honey your soul can never grow old its evergreen baby your smiles forever in my mind and memoryim thinking bout how people fall in love in mysterious ways maybe its all part of a plan ill just keep on making the same mistakes hoping that youll understandbut baby now take me into your loving arms kiss me under the light of a thousand stars place your head on my beating heart im thinking out loud that maybe we found love right where we are ohah la la la la la la la la la la la laso baby now take me into your loving arms kiss me under the light of a thousand stars oh darling place your head on my beating heart im thinking out loud that maybe we found love right where we areoh maybe we found love right where we are and we found love right where we are
## 3                                                                                                                                                                                                                                                                                                                                                                                                                                                                 its been a long day without you my friend and ill tell you all about it when i see you again weve come a long way from where we began oh ill tell you all about it when i see you again when i see you again heywiz khalifa dang who knew all the planes we flew good things weve been through that ill be standing right here talking to you bout another path i know we loved to hit the road and laugh but something told me that it wouldnt last had to switch up look at things different see the bigger picture those were the days hard work forever pays now i see you in a better place see you in a better placeuh how can we not talk about family when familys all that we got everything i went through you were standing there by my side and now you gon be with me for the last ride    meaning  byjamesg   editor    see you again is a song recorded by american rapper wiz khalifa featuring american singer charlie puth the track is part of the soundtrack of the 2015    see all   charlie puth its been a long day without you my friend and ill tell you all about it when i see you again i see you again weve come a long way yeah we came a long way from where we began you know we started oh ill tell you all about it when i see you again let me tell you when i see you againaah oh aah oh woooohohohohohoh yeahwiz khalifa first you both go out your way and the vibe is feeling strong and whats small turn to a friendship a friendship turn to a bond and that bond will never be broken the love will never get lost and the love will never get lost and when brotherhood come first then the line will never be crossed established it on our own when that line had to be drawn and that line is what we reach so remember me when im gone remember me when im gonehow can we not talk about family when familys all that we got everything i went through you were standing there by my side and now you gon be with me for the last ridecharlie puth so let the light guide your way yeah hold every memory as you go and every road you take will always lead you home homeits been a long day without you my friend and ill tell you all about it when i see you again weve come a long way from where we began oh ill tell you all about it when i see you again when i see you againaah oh uh aah oh yeah woooohohohohohoh ya ya when i see you again uh see you again woooohohohohohoh yeah yeah uhhuh when i see you again
## 4                                                                                                                                                                                                                  im like hey wassup hello seen yo pretty ass soon as you came in the door i just wanna chill got a sack for us to roll married to the money introduced her to my stove showed her how to whip it now she remix it for low she my trap queen let her hit the bando we be counting up watch how far them bands go we just selling dope talking matching lambos got 50 60 grams prob 100 grams though man i swear i love her how she work that damn pole hit the strip club we be letting bands go everybody hating we just call them fans though in love with the money i aint never letting goand i get high with my baby i just left the mall im getting fly with my baby yeahh and i get right with my baby i be in the kitchen cooking pies with my baby yeahh    meaning  byjamesg   editor    trap queen is the debut single by american rapper fetty wap aka willie maxwell taken from his eponymous debut studio album fetty wap 2015 the song    see all   and i get right with my baby i just left the mall im getting fly with my baby yeahh and i get right with my baby i be in the kitchen cooking pies with my baby yeahhi hit the strip with my trap queen cause all we know is bands i might just snatch up a rari and buy my boo a lambo i might just snatch up a necklace drop a couple on a ring she aint want it for nothin because i got her everything bitch you up on the bando ride with me where i cant go remy boys got extendo count up hella bands tho ill f*ck in your benz hoe fetty wap im living fifty thousand k how i stand tho if you checking for my pockets im likeand i get high with my baby i just left the mall im getting fly with my baby yeahh and i get right with my baby i be in the kitchen cooking pies with my baby yeahhand i get right with my baby i just left the mall im getting fly with my baby yeahh and i get right with my baby i be in the kitchen cooking pies with my babyim like hey wassup hello seen yo pretty ass soon as you came in the door i just wanna chill got a sack for us to roll married to the money introduced her to my stove showed her how to whip it now she remix it for low she my trap queen let her hit the bando we be counting up watch how far them bands go we just selling dope talking matching lambos got 50 60 grams prob 100 grams though man i swear i love her how she work that damn pole hit the strip club we be letting them bands go everybody hating we just call them fans though in love with the money i aint never letting goi be smoking dope and you know backwoods what i roll remy boy fetty eating shit up thats fasho ill run in ya house then ill f*ck ya ho remy boyz are nuttin rereremy boyz are nuttin
## 5                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         im hurting baby im broken down i need your loving loving i need it now when im without you im something weak you got me begging begging im on my kneesi dont wanna be needing your love i just wanna be deep in your love and its killing me when youre away ooh baby cause i really dont care where you are i just wanna be there where you are and i gotta get one little tasteyour sugar yes please wont you come and put it down on me im right here cause i need little love and little sympathy yeah you show me good loving make it alright need a little sweetness in my life your sugar yes please wont you come and put it down on memy broken pieces you pick them up dont leave me hanging hanging come give me some when im without ya im so insecure you are the one thing the one thing im living fori dont wanna be needing your love i just wanna be deep in your love and its killing me when youre away ooh baby cause i really dont care where you are i just wanna be there where you are and i gotta get one little tasteyour sugar yes please wont you come and put it down on me im right here cause i need little love and little sympathy yeah you show me good loving make it alright need a little sweetness in my life your sugar your sugar yes please yes please wont you come and put it down on meyeah i want that red velvet i want that sugar sweet dont let nobody touch it unless that somebodys me i gotta be a man there aint no other way cause girl youre hotter than southern california bayi dont wanna play no games i dont gotta be afraid dont give all that shy shit no make up on thats mysugar yes please wont you come and put it down on me down on me oh right here right here cause i need i need little love and little sympathy yeah you show me good loving make it alright need a little sweetness in my life your sugar sugar yes please yes please wont you come and put it down on meyour sugar yes please wont you come and put it down on me im right here cause i need little love and little sympathy yeah you show me good loving make it alright need a little sweetness in my life your sugar yes please wont you come and put it down on me down on me down on me
## 6                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      oh dont you dare look back just keep your eyes on me i said youre holding back she said shut up and dance with me this woman is my destiny she said oh oh oh shut up and dance with mewe were victims of the night the chemical physical kryptonite helpless to the bass and the fading light oh we were bound to get together bound to get togethershe took my arm i dont know how it happened we took the floor and she saidoh dont you dare look back just keep your eyes on me i said youre holding back she said shut up and dance with me this woman is my destiny she said oh oh oh shut up and dance with mea backless dress and some beat up sneaks my discotheque juliet teenage dream i felt it in my chest as she looked at me i knew we were bound to be together bound to be togethershe took my arm i dont know how it happened we took the floor and she saidoh dont you dare look back just keep your eyes on me i said youre holding back she said shut up and dance with me this woman is my destiny she said oh oh oh shut up and dance with medeep in her eyes i think i see the future i realize this is my last chanceshe took my arm i dont know how it happened we took the floor and she saidoh dont you dare look back just keep your eyes on me i said youre holding back she said shut up and dance with me this woman is my destiny she said oh oh oh shut up and danceoh dont you dare look back just keep your eyes on me i said youre holding back she said shut up and dance with me this woman is my destiny she said oh oh oh shut up and dance with meoh oh oh shut up dance with me oh oh oh shut up dance with me
##   Source
## 1      1
## 2      1
## 3      1
## 4      1
## 5      1
## 6      1
```

This is also the Redesign of a bad chart: 


``` r
# Before
billboard_words <- billboard %>%
  unnest_tokens(word, Lyrics) %>%
  anti_join(stop_words, by = "word") %>%
  count(word, sort = TRUE) %>%
  slice_max(n, n = 100) 

ggplot(billboard_words, aes(label = word, size = n, color = n)) +
  geom_text_wordcloud_area() +
  scale_size_area(max_size = 30) +
  theme_minimal() +
  labs(title = "Most Frequent Words in Billboard Top 100 Track Titles")
```

<img src="Morrison_project_03_files/figure-html/unnamed-chunk-13-1.png" alt=" A word cloud titled 'Most Frequent Words in Billboard Top 100 Track Titles,'. The visualization displays various words from song titles in varying sizes and shades of blue. The words 'love,', 'im' and 'dont' appear in the largest font and lightest blue, indicating they are the most frequently occurring words in the dataset, while numerous other words like 'girl,' 'aint,' 'yeah,' and 'time' appear in smaller sizes around them." style="display: block; margin: auto;" />


``` r
# After
ggplot(billboard_words, aes(label = word, size = n, color = n)) +
  geom_text_wordcloud_area(shape = "circle", seed = 42) +
  scale_size_area(max_size = 30) +
  scale_color_gradient(low = "#69b3a2", high = "#404080") +
  theme_void() +
  labs(title = "Most Frequent Words in Billboard Top 100 Track Titles") +
  theme(plot.title = element_text(hjust = 0.5, face = "bold", size = 16))
```

<img src="Morrison_project_03_files/figure-html/unnamed-chunk-14-1.png" alt=" A word cloud titled 'Most Frequent Words in Billboard Top 100 Track Titles,'. The visualization displays various words from song titles in varying sizes and shades of blue. The words 'love,', 'im' and 'dont' appear in the largest font and darkest blue, indicating they are the most frequently occurring words in the dataset, while numerous other words like 'girl,' 'aint,' 'yeah,' and 'time' appear in smaller sizes around them." style="display: block; margin: auto;" />

