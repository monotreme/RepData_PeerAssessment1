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
activity_by_date <- aggregate(activity$steps, by = list(activity$date), sum);
activity_by_date$steps <- as.numeric(activity_by_date$Group.1);
```


## What is mean total number of steps taken per day?
The histogram of the data follows:


```r
hist(activity_by_date$steps, xlab= "Daily Steps", main = "Activity Histogram");
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-1-1.png) 

The mean number of daily steps is calculated as:

```r
mean_steps <- round(mean(activity_by_date$steps),digits=2)
```
The mean number of daily steps is 30.72.
The median value of daily steps is calculated as:


```r
median_steps <- median(activity_by_date$steps)
```
The median number of daily steps is 29.


## What is the average daily activity pattern?

Aggregating the data by interval and taking the mean provides the data to find the average daily activity pattern.


```r
activity_by_interval <- aggregate(activity$steps, by = list(activity$interval), mean);
```


```r
plot(activity_by_interval$x,type="l", ylab="Mean number of steps",xlab="5-minute time interval from midnight")
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5-1.png) 

The maximum number of steps is calculated as:

```r
max_steps <- round(max(activity_by_interval$x),digits=0)
```
The maximum of the average number of steps is 206.

The interval can be found by the command:

```r
max_steps_interval <- activity[which.max(activity_by_interval$x),]$interval
```

This occurs at interval  (time) 835.

## Inputing missing values

The number of missing values was calculated as shown below:

```r
activity_missing <- is.na(activity_raw)
number_of_missing_values <- length(activity_raw[activity_missing])
```

The number of missing values is 2304.


The strategy used to fill in the missing data is to use the mean value for each interval for missing intervals, which has been calculated earlier, and stored in activity_by_interval. 
The missing intervals are found by first constructing vector containing all the intervals, and another vector with all of the dates contained in the existing data set.
A new vector will be created for all intervals within timeframe of the original data set.  Where the original dataset had NA values, these will be filled in.

```r
intervals <-unique(activity$interval)
dates <-levels(activity$date)

for (day in dates){
  missing_data <- is.na(activity[])
}


new_data <-NULL
for (counter in interval) {i = I + 1}
```

```
## Error in eval(expr, envir, enclos): object 'interval' not found
```

## Are there differences in activity patterns between weekdays and weekends?
