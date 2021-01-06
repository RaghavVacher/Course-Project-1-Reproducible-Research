# Course-Project-1-Reproducible-Research

Course Project - 1
1. Reading the file:
  ```x <- read.csv(choose.files(), sep = ",")```
2. Histogram for steps each day
  ```library(ggplot2)
  library(lubridate)
  x$date <- as.POSIXct(x$date, "%Y/%m/%d")
  aggData <- aggregate(steps ~ date, x, sum, na.rm = T)
  png("Plot.png")
  plot <- ggplot(data = aggData, aes(date, steps), col("blue"))
  plot <- plot + geom_bar(stat = "identity") + xlab("Date") + ylab("Steps per Day")
  plot
  dev.off()
## png 
##   2
  plot
```

3. Mean and median number of steps taken each day

```   meanData <- aggregate(steps ~ date, x, mean, na.rm = T)
  medianData <- aggregate(steps ~ date, x, median) 
  ```
  
4. Time series plot of the average number of steps taken
  ```png("Plot2.png")
  plot2 <- ggplot(meanData, aes(date, steps))
  plot2 <- plot2 + geom_line(color = "blue") + xlab("Date") + ylab("Mean Steps a Day") + ggtitle("Average Number of Steps Taken")
  plot2
  dev.off()
## png 
##   2
  plot2
```

5. The 5-minute interval that, on average, contains the maximum number of steps
 ``` subData <- aggregate(steps ~ date + interval, x, mean, na.rm = T)
  result <- aggregate(steps ~ interval, subData, max)
  head(result)
##   interval steps
## 1        0    47
## 2        5    18
## 3       10     7
## 4       15     8
## 5       20     4
## 6       25    52 
```
6. Code to describe and show a strategy for imputing missing data
 ``` subData <- aggregate(steps ~ date + interval, x, mean, na.rm = T)
  result <- aggregate(steps ~ interval, subData, mean)
  missingIndex<-is.na(x[,1])
  m<-mean(result$steps)
  x[missingIndex,1]<-m
  head(x)
##     steps       date interval
## 1 37.3826 2012-10-01        0
## 2 37.3826 2012-10-01        5
## 3 37.3826 2012-10-01       10
## 4 37.3826 2012-10-01       15
## 5 37.3826 2012-10-01       20
## 6 37.3826 2012-10-01       25
```
7. Histogram of the total number of steps taken each day after missing values are imputed
  ```library(ggplot2)
  library(lubridate)
  x$date <- as.POSIXct(x$date, "%Y/%m/%d")
  aggData <- aggregate(steps ~ date, x, sum, na.rm = T)
  png("Plot3.png")
  plot <- ggplot(data = aggData, aes(date, steps), col("blue"))
  plot <- plot + geom_bar(stat = "identity") + xlab("Date") + ylab("Steps per Day")
  plot
  dev.off()
## png 
##   2
  plot
```

8. Panel plot comparing the average number of steps taken per 5-minute interval across weekdays and weekends
  ```x$weekday <- weekdays(x$date)
  x$weektype <- ifelse(x$weekday == "Saturday" | x$weekday == "Sunday", "Weekend", "Weekday")
  
  aggDat <- aggregate(steps ~ interval + weektype, x, mean, rm.na = T)
  aggDat$time <- aggDat$interval/100
  png("Plot4.png")
  plot4 <- ggplot(data = aggDat, aes(time, steps))
  plot4 <- plot4 + geom_line() + xlab("Time") + ylab("Average Steps per 5 Minute Interval") + ggtitle("Average Steps Taken Per 5-minute Interval Across Weekdays & Weekends") + facet_grid(. ~ weektype)
  plot4
  dev.off()
## png 
##   2
  plot4
  ```
