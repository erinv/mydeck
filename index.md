---
title       : Accessing Data from a Regional Travel Demand Model
subtitle    : Prepared for Developing Data Products, June 2016
author      : EV
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Background and Statement of Problem

Many public planning agencies have developed models of travel demand in their regions. The latest generation of models are known as "activity-based" models because they synthesize a regional population and associated activities (working, shopping, socializing, etc.) that lead to demand for travel.  Outputs of the models include a list of all the trips made by the synthesized population between geographic entities called Travel Analysis Zones (there are 1454 of them in the San Francisco Bay Area) for each mode of transportation (walk, bike, drive, transit, etc.).

While the travel demand model outputs are potentially of interest to many planners, engineers, and members of the general public, the output data sets are quite large and difficult to wrangle without a tool such as R.  For example, the combined individual and joint trips data set produced by the regional travel demand model in the San Francisco Bay Area contains this many rows:

```r
load("MTCTrips.RData")
nrow(Trips)
```

```
## [1] 12201135
```

---.class #id 

## Solution:  MTCTrips Shiny App

The aim of this Shiny app is to make the travel model output trips more accessible by generating summary tables and plots for variables in the trips data set which include:


```
##  [1] "hh_id"             "person_id"         "person_num"       
##  [4] "tour_id"           "stop_id"           "inbound"          
##  [7] "tour_purpose"      "orig_purpose"      "dest_purpose"     
## [10] "orig_taz"          "orig_walk_segment" "dest_taz"         
## [13] "dest_walk_segment" "parking_taz"       "depart_hour"      
## [16] "trip_mode"         "tour_mode"         "tour_category"    
## [19] "num_participants"  "O_county"          "D_county"
```
(Note:  taz refers to Travel Analysis Zone)

---.class #id 

## Current Status

The current version of the app uses a simplified version of the modeled trips which have been aggregated at the geographic level of the county using the following code:


```r
ctrips_table <- aggregate(Trips, by = list(Trips$O_county, Trips$D_county, Trips$trip_mode), 
    FUN = sum)
```
Resulting in:

```
## 'data.frame':	1296 obs. of  7 variables:
##  $ trip_mode : Factor w/ 16 levels "1","2","3","5",..: 1 2 3 4 5 6 7 8 9 10 ...
##  $ O_county  : Factor w/ 9 levels "1","2","3","4",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ D_county  : Factor w/ 9 levels "1","2","3","4",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ Trips     : int  371906 0 155772 114148 349780 38457 130865 90286 3 12777 ...
##  $ Mode      : Factor w/ 18 levels "1-Drive alone-free",..: 1 11 12 14 16 17 18 2 3 4 ...
##  $ fromcounty: Factor w/ 9 levels "Alameda","Contra Costa",..: 5 5 5 5 5 5 5 5 5 5 ...
##  $ tocounty  : Factor w/ 9 levels "Alameda","Contra Costa",..: 5 5 5 5 5 5 5 5 5 5 ...
```

---.class #id 

## Future Development Plans

Given the short development time, the app is currently a vastly simplified version of what was originally envisioned.  In its present state, the app takes user inputs for two counties ("from" and "two") and produces a simple summary and plot of trips by mode of travel.  The app will require further development to become truly useful.  Desirable improvements include:

1.  Use of a reactive data function to subset the trips data 
2. Use of detailed trips data set as input rather than simplified version (this will require strategies to improve performance or a means to generate messages to the users while data is being subsetted)
3. More flexible user selection of variables of interest
4. Features to download/save output data summaries or plots
5. Some kind of map interface would be cool, too

---.class #id
