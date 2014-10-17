---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
---

## Loading and preprocessing the data

The following code reads the data.  Omitting records containing NA, the "aggregate" function is used to calculate the daily steps.


```r
activity_raw <- read.csv("activity.csv");
activity <- na.omit(activity_raw);
activity_bydate <- aggregate(activity$steps, by = list(activity$date), sum);
activity_bydate$steps <- as.numeric(activity_bydate$Group.1);
```


## What is mean total number of steps taken per day?
The histogram of the data follows:


```r
hist(activity_bydate$steps, xlab= "Daily Steps", main = "Activity Histogram");
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2-1.png) 

The mean number of daily steps is 30.72.
The median value of daily steps to be 29.  This text used an inline calculation to 2 significant digits. These figures can also be found in the summary data below.



```r
summary(activity_bydate);
```

```
##        Group.1         x             steps      
##  2012-10-02: 1   Min.   :   41   Min.   : 2.00  
##  2012-10-03: 1   1st Qu.: 8841   1st Qu.:16.00  
##  2012-10-04: 1   Median :10765   Median :29.00  
##  2012-10-05: 1   Mean   :10766   Mean   :30.72  
##  2012-10-06: 1   3rd Qu.:13294   3rd Qu.:47.00  
##  2012-10-07: 1   Max.   :21194   Max.   :60.00  
##  (Other)   :47
```


## What is the average daily activity pattern?

The average daily activity pattern is shown 



## Inputing missing values



## Are there differences in activity patterns between weekdays and weekends?
