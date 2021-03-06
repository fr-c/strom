# Analysis of Health and Economic Impacts of Weather Events in the US
## Synopsis

This quick analysis aims at defining which types of weather events have the strongest impacts on:
1. population health (measured in terms of fatalities and injuries) 
2. the economy (measured through property and crop damage)

## Data Processing

We start by loading the data into R.


```r
storm <- read.csv(bzfile("repdata_data_StormData.csv.bz2"))
```

We then create specific data frames to look at particular values (sum of fatalities, injuries, property damage and crop damage)


```r
library(dplyr)
```

```
## Warning: package 'dplyr' was built under R version 3.3.2
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
by_ev <- group_by(storm, EVTYPE)

sum_fatal <- summarise(by_ev, sum(FATALITIES))
names(sum_fatal)[2] <- "SUM_FATAL"
fatal <- sum_fatal[sum_fatal$SUM_FATAL != 0, ]
fatal <- fatal[order(fatal$SUM_FATAL, decreasing = TRUE),]
top_fatal <- fatal$SUM_FATAL[1:10]
names(top_fatal) <- fatal$EVTYPE[1:10]

sum_inj <- summarise(by_ev, sum(INJURIES))
names(sum_inj)[2] <- "SUM_INJ"
inj <- sum_inj[sum_inj$SUM_INJ != 0, ]
inj <- inj[order(inj$SUM_INJ, decreasing = TRUE),]
top_inj <- inj$SUM_INJ[1:10]
names(top_inj) <- inj$EVTYPE[1:10]

sum_prop <- summarise(by_ev, sum(PROPDMG))
names(sum_prop)[2] <- "SUM_PROP"
prop <- sum_prop[sum_prop$SUM_PROP != 0, ]
prop <- prop[order(prop$SUM_PROP, decreasing = TRUE),]
top_prop <- prop$SUM_PROP[1:10]
names(top_prop) <- prop$EVTYPE[1:10]

sum_crop <- summarise(by_ev, sum(INJURIES))
names(sum_crop)[2] <- "SUM_CROP"
crop <- sum_crop[sum_crop$SUM_CROP != 0, ]
crop <- crop[order(crop$SUM_CROP, decreasing = TRUE),]
top_crop <- crop$SUM_CROP[1:10]
names(top_crop) <- crop$EVTYPE[1:10]

##Those crop and prop values make no sense because the unit is not taken into account, due to lack of time
```

## Results

Here are the 10 event types that caused the most fatalities and the 10 that caused the most injuries between 1950 and 2011:


```r
par(mfrow = c(1, 2), oma=c(0,0,2,0))

barplot(top_fatal, cex.names=0.7, las=2, main="Total fatalities")
barplot(top_inj, cex.names=0.7, las=2, main="Total injuries")
title("Most impacting event types on population health - 1950-2011", outer=TRUE)
```

![](USWeatherEvents_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

Here are the 10 event types that caused the most property damage and the 10 that caused the most crop damage between 1950 and 2011:


```r
par(mfrow = c(1, 2), oma=c(0,0,2,0))

barplot(top_prop, cex.names=0.7, las=2, main="Total property damage")
barplot(top_crop, cex.names=0.7, las=2, main="Total crop damage")
title("Most impacting event types on the economy - 1950-2011", outer=TRUE)
```

![](USWeatherEvents_files/figure-html/unnamed-chunk-4-1.png)<!-- -->


## Conclsion

This rough analysis shows that *tornados* are the most harmful weather events in the US in terms of casualties.
However, the first step to getting a more accurate picture would be to clean up the categorization of event types in the dataset (many distinct EVTYPE values for a single actual event type, such as *thunderstorm wind*)

The analysis must be corrected for the economic impact, since the unit of damage value is not taken into account.
