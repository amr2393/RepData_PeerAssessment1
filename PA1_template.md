# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

First we load appropriate libraries, set some options, then unzip and load the data:


```r
suppressWarnings(suppressMessages(library(dplyr)))
suppressWarnings(suppressMessages(library(ggplot2)))
options(scipen=999) # Get numbers to print nicely...

unzip('activity.zip')
data = read.csv('activity.csv', stringsAsFactors = F)
```

## What is mean total number of steps taken per day?

The following code computes total steps taken per day:


```r
stepsPerDay = data %>% # Start with the data we loaded
  filter(!is.na(steps)) %>% # Remove entries with NA steps
  group_by(date) %>% # Group into days
  summarise(totalSteps = sum(steps))  #Find total steps each day
```

Then we plot a histogram.


```r
ggplot() + geom_histogram(data = stepsPerDay, aes(x = totalSteps, fill = 'coral'), binwidth = 1000 ) + theme(legend.position="none")
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png) 


```r
meanStepsPerDay = mean(stepsPerDay$totalSteps)
medianStepsPerDay = median(stepsPerDay$totalSteps)
```

So the mean total number of steps taken per day is 10766.1886792, and the median is 10765. 

## What is the average daily activity pattern?

The following code computes average steps taken in each 5 minute interval:


```r
stepsPerInterval = data %>% # Start with the data we loaded
  filter(!is.na(steps)) %>% # Remove entries with NA steps
  group_by(interval) %>% # Group into days
  summarise(meanSteps = mean(steps))  #Find mean steps each interval
```

Plot a time series plot of steps


```r
ggplot() + geom_line(data = stepsPerInterval, aes(x = interval, y = meanSteps), binwidth = 1000 ) + theme(legend.position="none")
```

![](PA1_template_files/figure-html/unnamed-chunk-6-1.png) 

And find the maximum.


```r
maxIntervalStart = stepsPerInterval$interval[which.max(stepsPerInterval$meanSteps)]
```

The interval with the most steps on average is from 835 to 840 minutes.

## Imputing missing values


```r
numberOfNas = sum(is.na(data$steps))
```
Of the 17568 rows in the data, 2304 are *NA* (13.11%)


## Are there differences in activity patterns between weekdays and weekends?
