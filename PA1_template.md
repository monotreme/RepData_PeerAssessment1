---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
---

## Loading and preprocessing the data

The following code reads the data.  Omitting records containing NA, the "aggregate" function is used to calculate the daily steps.
The data frame "activity_raw" holds the data as read from the .csv file.  The data frame "activity" holds the "cleaned" data used for calculations, graphs, etc.
In this first half of the assignment, the "cleaned" data simply omits data with the value NA.

```r
activity_raw <- read.csv("activity.csv");
activity_omitted <- na.omit(activity_raw);
activity <- activity_omitted
activity_by_date <- aggregate(activity$steps, by = list(activity$date), sum);
activity_by_date$steps <- as.numeric(activity_by_date$x);
```


## What is mean total number of steps taken per day?
The histogram of the data follows:


```r
plot_hist <- function(plot_data){
hist(plot_data, xlab= "Daily Steps", main = "Activity Histogram")};
plot_hist(activity_by_date$steps);
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-1-1.png) 

The mean number of daily steps is calculated as:

```r
mean_steps <- round(mean(activity_by_date$steps),digits=0)
```
The mean number of daily steps is 1.0766 &times; 10<sup>4</sup>.
The median value of daily steps is calculated as:


```r
median_steps <- median(activity_by_date$steps)
```
The median number of daily steps is 1.0765 &times; 10<sup>4</sup>.


## What is the average daily activity pattern?

Aggregating the data by interval and taking the mean provides the data to find the average daily activity pattern.


```r
activity_by_interval <- aggregate(activity$steps, by = list(activity$interval), mean);
```


```r
plot(activity_by_interval$x,type="l", ylab="Mean number of steps",xlab="5-minute time interval from midnight", main = "Average Daily Activity")
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5-1.png) 

The maximum number of steps is calculated as:

```r
max_steps <- as.integer(max(activity_by_interval$x))
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
activity_missing <- is.na(activity_raw);
number_of_missing_values <- length(activity_raw[activity_missing]);
```

The number of missing values is 2304.


The strategy used to fill in the missing data is to use the mean value for each day.
Assuming that missing values are indicated by NA, rather than just not being present, the steps with the value NA are replaced by the average daily steps.

The data frame "activity", which holds the "cleaned" data, will now have esimated values instead of simply omitting NA values.

```r
activity <- activity_raw;
for (inter in activity_by_interval$Group.1){
activity$steps[is.na(activity$steps) & activity$interval == inter] <- activity_by_interval$x[activity_by_interval$Group.1 == inter]
};
```

To see what difference using estimated values has instead of ignoring missing data, the histogram, mean and median are shown below:


```r
activity_by_date <- aggregate(activity$steps, by = list(activity$date), sum);
activity_by_date$steps <- as.numeric(activity_by_date$x);


plot_hist(as.numeric(activity_by_date$steps));
```

![plot of chunk unnamed-chunk-10](figure/unnamed-chunk-10-1.png) 

The mean number of daily steps is calculated as:

```r
mean_steps <- as.integer(mean(activity_by_date$steps))
```
The mean number of daily steps is 10766.
The median value of daily steps is calculated as:


```r
median_steps <- as.integer(median(activity_by_date$steps))
```
The median number of daily steps is 10766.



## Are there differences in activity patterns between weekdays and weekends?



```r
activity$day_type <- as.factor(ifelse(weekdays( as.Date(activity$date)) %in% c("Saturday","Sunday"), "Weekend", "Weekday"));
activity_by_interval <- aggregate(activity$steps, by = list(activity$day_type, activity$interval), mean);
activity_by_interval$steps <- as.numeric(activity_by_interval$x)



# Lattice plots 
library(lattice) 
weekday.f<-factor(activity_by_interval$Group.1,levels=c("Weekday","Weekend"),
    labels=c("Weekday","Weekend")) 
# kernel density plot 
attach(activity_by_interval)

# kernel density plots by factor level (alternate layout) 
densityplot(~steps|weekday.f, 
    main="Density Plot Weekend/Weekday",
   xlab="Interval", 
   layout=c(1,2))
```

![plot of chunk unnamed-chunk-13](figure/unnamed-chunk-13-1.png) 
