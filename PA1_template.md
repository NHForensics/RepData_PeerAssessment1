`{r setup, include=FALSE} knitr::opts_chunk$set(echo = TRUE)`

Loading and preprocessing the data
----------------------------------

`{r task 1} data <- read.csv("activity.csv") data$date <- as.Date(data$date)`

What is mean total number of steps taken per day?
-------------------------------------------------

### 1) Calculate the total number of steps taken per day

`{r task 2 question 1} dailysum <- aggregate(steps ~ date,                        data = data,                        FUN = sum,                        na.rm = TRUE) dailysum`

### 2) Make a histogram of the total number of steps taken each day

`{r task 2 question 2} hist(dailysum$steps, xlab = "Steps", main = "Histogram of the number of steps")`

### 3) Calculate and report the mean and median of the total number of steps taken per day

`{r meansteps} meansteps <- mean(dailysum$steps)`

Mean of the total number of steps taken per day = `r meansteps`.

`{r mediansteps} mediansteps <- median(dailysum$steps)`

Median of the total number of steps taken per day = `r mediansteps`.

What is the average daily activity pattern?
-------------------------------------------

`{r task 3} meandailyactivity <- aggregate(steps ~ interval,                                 data = data,                                 FUN = mean,                                 na.rm = TRUE)`

### 1) Make a time series plot (i.e. type = “l”) of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)

`{r task 3 question 1} plot(meandailyactivity$interval, meandailyactivity$steps,       type = "l",       xlab = "Intervals",       ylab = "Average steps per interval")`

### 2) Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

\`\`\`{r task 3 question 2} maxsteps &lt;-
max(meandailyactivity*s**t**e**p**s*)*m**a**x**i**n**t**e**r**v**a**l* &lt;  − *m**e**a**n**d**a**i**l**y**a**c**t**i**v**i**t**y*interval\[
which(meandailyactivity$steps == maxsteps)\]

maxinterval


    ## Imputing missing values

    ### 1) Calculate and report the total number of missing values in the dataset  (i.e. the total number of rows with NAs)

    ```{r task 4 question 1}
    sum(is.na(data$steps))

### 2) Devise a strategy for filling in all of the missing values in the dataset. Create a new dataset that is equal to the original dataset but with the missing data filled in.

\`\`\`{r task 4 questions 2 and 3} intervalmean &lt;-
tapply(data*s**t**e**p**s*, *d**a**t**a*interval, mean, na.rm = TRUE)

stepsNAs &lt;- data\[is.na(data$steps),\] stepswithdata &lt;-
data\[!is.na(data$steps),\]

stepsNAs*s**t**e**p**s* &lt;  − *a**s*.*f**a**c**t**o**r*(*s**t**e**p**s**N**A**s*interval)
levels(stepsNAs$steps) &lt;- intervalmean

stepsNAs*s**t**e**p**s* &lt;  − *a**s*.*n**u**m**e**r**i**c*(*s**t**e**p**s**N**A**s*steps)

newdata &lt;- rbind(stepsNAs, stepswithdata)


    ### 4) Make a histogram of the total number of steps taken each day and calculate and report the mean and median total number of steps taken per day. Do  these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total  daily number of steps?

    ```{r task 4 question 4}
    par(mfrow = c(1, 2))

    hist(dailysum$steps, 
         main = "Total steps without NAs", 
         xlab = "Number of steps")
    abline(v = mean(dailysum$steps), col = "red")
    abline(v = median(dailysum$steps), col = "blue")

    newdailysum <- aggregate(steps ~ date, 
                          data = newdata, 
                          FUN = sum)

    hist(newdailysum$steps, 
         main = "Total steps with NAs imputed", 
         xlab = "Number of steps")
    abline(v = mean(newdailysum$steps), col = "red")
    abline(v = median(newdailysum$steps), col = "blue")

`{r newmean} newmean <- mean(newdailysum$steps)`

The mean of the new data set increased to `r newmean`.

`{r newmedian} newmedian <- median(newdailysum$steps)`

The median of the new data set increased to `r newmedian`.

Are there differences in activity patterns between weekdays and weekends?
-------------------------------------------------------------------------

### 1) Create a new factor variable in the dataset with two levels – “weekday” and “weekend” indicating whether a given date is a weekday or weekend day.

`{r task 5 question 1} newdata$weekday <- weekdays(newdata$date) newdata$wkendorwkday <- ifelse(newdata$weekday == "Sunday"                                 | newdata$weekday == "Saturday", "Weekend", "Weekday")`

### 2) Make a panel plot containing a time series plot (i.e. type = “l”) of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis).

\`\`\`{r task 5 question 2} weekdaydata &lt;- subset(newdata,
wkendorwkday == “Weekday”) weekenddata &lt;- subset(newdata,
wkendorwkday == “Weekend”)

weekdayagg &lt;- aggregate(steps ~ interval, data = weekdaydata, FUN =
mean) weekendagg &lt;- aggregate(steps ~ interval, data = weekenddata,
FUN = mean)

par(mfrow = c(2, 1))
plot(weekdayagg*i**n**t**e**r**v**a**l*, *w**e**e**k**d**a**y**a**g**g*steps,
type = “l”, xlab = “Interval”, ylab = “Steps”, main = “Weekday”)
plot(weekendagg*i**n**t**e**r**v**a**l*, *w**e**e**k**e**n**d**a**g**g*steps,
type = “l”, xlab = “Interval”, ylab = “Steps”, main = “Weekend”) \`\`\`
