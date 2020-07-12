---
title: "PA1_template.Rmd"
author: "Cesar O. Flores Garcia"
date: "10/19/2014"
output: html_document
---

### Loading Data

First we need to load the data. The data need to be only loaded a single time. That is why we use `cache=TRUE`:


```r
library(data.table)
```

```
## data.table 1.9.4  For help type: ?data.table
## *** NB: by=.EACHI is now explicit. See README to restore previous behaviour.
```

```r
zip_file  <- "activity.zip"
text_file <- "activity.csv"
colClasses <- c("numeric","Date","integer") 
df_activity <- read.csv(unz(zip_file, text_file), header = TRUE, sep = ",", colClasses=colClasses,na.strings="NA")
dt_activity <- as.data.table(df_activity)
```

### Number of Steps by day

The next plot shows the histogram of steps. NA values are ignored, and the number of bins was assigned to 20. Notice that the big amount of days with 0 frequency is due in part to the NA values in some days.
 

Now, we can calculate both the mean and median using:


```r
mean_steps   <- mean(step_daily_average)
median_steps <- median(step_daily_average)
```

After running the last code we know that the mean and median are 9354.2295082 and 1.0395 &times; 10<sup>4</sup>, respectivelly.

### Average Daily Activity Pattern
The next plot shows the average number of steps by interval, averaged across all days
![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4-1.png) 

Now, for finding the interval with the maximum number of steps averaged across al days we use:

```r
max_steps_interval   <- max(dt_temp$average_steps)
max_interval <- dt_temp[average_steps==max_steps_interval,interval]
```

The previous code give us that the 5-minutes interval with the highest number of steps is 835

### Missing Values

First we create a new table were we will fill the step missing values using the mean of the 5-minutes interval.


```r
n_nas <- sum( !is.na( dt_activity$steps ) ) 
dt_activity_fill <- copy(dt_activity)
dt_activity_fill[is.na(steps)]$steps <- with(dt_activity, 
                                             ave(steps, interval, 
                                                 FUN = function(x) mean(x, na.rm = TRUE)))[is.na(dt_activity_fill$steps)]
```

Now, we can repeat the process, for calculating the histogram: 

We can se that the histogram is totally different, specially for the case of 0 steps. And the reason of this is because there are days
that are not filled at all for the column steps. Finally, the news mean and median are 1.0766189 &times; 10<sup>4</sup> and 1.0766189 &times; 10<sup>4</sup>, respectivelly.

### Weekdays vs Weekends

The next plot will plot the difference between weekends and weekdays


```
##            steps       date interval day_type
##     1: 1.7169811 2012-10-01        0  weekday
##     2: 0.3396226 2012-10-01        5  weekday
##     3: 0.1320755 2012-10-01       10  weekday
##     4: 0.1509434 2012-10-01       15  weekday
##     5: 0.0754717 2012-10-01       20  weekday
##    ---                                       
## 17564: 4.6981132 2012-11-30     2335  weekday
## 17565: 3.3018868 2012-11-30     2340  weekday
## 17566: 0.6415094 2012-11-30     2345  weekday
## 17567: 0.2264151 2012-11-30     2350  weekday
## 17568: 1.0754717 2012-11-30     2355  weekday
```


As we can se there appears to exist some difference at the start of the day. This may due to the fact that on weekends, people normally
wake up later than on weekdays.



> Written with [StackEdit](https://stackedit.io/).
