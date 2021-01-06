Course Project - 1
===================

### 1. Reading the file:

```r
  x <- read.csv(choose.files(), sep = ",")
```

```
## Error in read.table(file = file, header = header, sep = sep, quote = quote, : duplicate 'row.names' are not allowed
```

### 2. Histogram for steps each day


```r
  library(ggplot2)
  library(lubridate)
  x$date <- as.POSIXct(x$date, "%Y/%m/%d")
  aggData <- aggregate(steps ~ date, x, sum, na.rm = T)
  png("Plot.png")
  plot <- ggplot(data = aggData, aes(date, steps), col("blue"))
  plot <- plot + geom_bar(stat = "identity") + xlab("Date") + ylab("Steps per Day")
  plot
  dev.off()
```

```
## RStudioGD 
##         2
```

```r
  plot
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2-1.png)![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2-2.png)

### 3. Mean and median number of steps taken each day


```r
  meanData <- aggregate(steps ~ date, x, mean, na.rm = T)
  medianData <- aggregate(steps ~ date, x, median)
```

### 4. Time series plot of the average number of steps taken


```r
  png("Plot2.png")
  plot2 <- ggplot(meanData, aes(date, steps))
  plot2 <- plot2 + geom_line(color = "blue") + xlab("Date") + ylab("Mean Steps a Day") + ggtitle("Average Number of Steps Taken")
  plot2
  dev.off()
```

```
## RStudioGD 
##         2
```

```r
  plot2
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4-1.png)![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4-2.png)

### 5. The 5-minute interval that, on average, contains the maximum number of steps


```r
  subData <- aggregate(steps ~ date + interval, x, mean, na.rm = T)
  result <- aggregate(steps ~ interval, subData, max)
  head(result)
```

```
##   interval    steps
## 1        0 393.3056
## 2        5 393.3056
## 3       10 393.3056
## 4       15 393.3056
## 5       20 393.3056
## 6       25 393.3056
```

### 6. Code to describe and show a strategy for imputing missing data


```r
  subData <- aggregate(steps ~ date + interval, x, mean, na.rm = T)
  result <- aggregate(steps ~ interval, subData, mean)
  missingIndex<-is.na(x[,1])
  m<-mean(result$steps)
  x[missingIndex,1]<-m
  head(x)
```

```
##      steps       date interval weekday weektype
## 1 393.3056 2012-10-01        0  Monday  Weekday
## 2 393.3056 2012-10-01        5  Monday  Weekday
## 3 393.3056 2012-10-01       10  Monday  Weekday
## 4 393.3056 2012-10-01       15  Monday  Weekday
## 5 393.3056 2012-10-01       20  Monday  Weekday
## 6 393.3056 2012-10-01       25  Monday  Weekday
```

### 7. Histogram of the total number of steps taken each day after missing values are imputed


```r
  library(ggplot2)
  library(lubridate)
  x$date <- as.POSIXct(x$date, "%Y/%m/%d")
  aggData <- aggregate(steps ~ date, x, sum, na.rm = T)
  png("Plot3.png")
  plot <- ggplot(data = aggData, aes(date, steps), col("blue"))
  plot <- plot + geom_bar(stat = "identity") + xlab("Date") + ylab("Steps per Day")
  plot
  dev.off()
```

```
## RStudioGD 
##         2
```

```r
  plot
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-7-1.png)![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-7-2.png)

### 8. Panel plot comparing the average number of steps taken per 5-minute interval across weekdays and weekends


```r
  x$weekday <- weekdays(x$date)
  x$weektype <- ifelse(x$weekday == "Saturday" | x$weekday == "Sunday", "Weekend", "Weekday")
  
  aggDat <- aggregate(steps ~ interval + weektype, x, mean, rm.na = T)
  aggDat$time <- aggDat$interval/100
  png("Plot4.png")
  plot4 <- ggplot(data = aggDat, aes(time, steps))
  plot4 <- plot4 + geom_line() + xlab("Time") + ylab("Average Steps per 5 Minute Interval") + ggtitle("Average Steps Taken Per 5-minute Interval Across Weekdays & Weekends") + facet_grid(. ~ weektype)
  plot4
  dev.off()
```

```
## RStudioGD 
##         2
```

```r
  plot4
```

![plot of chunk unnamed-chunk-8](figure/unnamed-chunk-8-1.png)![plot of chunk unnamed-chunk-8](figure/unnamed-chunk-8-2.png)
  
