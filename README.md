# R-Courses-
Markdown Scripts

---
title: "Handling Time and Date"
author: "Andreas Bury"
date: "11 Juni 2020"
output: pdf_document
---


## Module 1

+ In this first and very short module, you will learn how to get the current date and time, check whether the time occurs in AM or PM and whether a particular year is a leap year or not. You will get a broad overview of the case study used in the course along with the underlying data set. You will also learn about the different R packages we will using throughout the course and things to keep in mind while reading data sets with date/time

```{r setup, include=FALSE}
if (!require(lubridate)) install.packages('lubridate')
library(lubridate)

```



```{r}
Sys.Date() # get current date
today()   # same output

Sys.time()  # get current time (and timezone)
now()  # same output

am(Sys.time())  # check whether current tim is AM
pm(Sys.time())  # or PM
 
leap_year(2018) # Check whether 2018 is a leap year
leap_year(2016) # Check whether 2016 is a leap year
leap_year(2020) # Check whether 2020 is a leap year

leap_year(today()) # check whether the current year is a leap year

```
---  

## Case Study

+ extract date, month and year from Due
+ compute the number of days to settle Invoice
+ compute days over Due
+ check if Due year is a leap year
+ check when Due day in february is 29, whether it is a leap year
+ how many Invoiceswere settled within Due date
+ how many Invoices are Due in each quarter
+ what is the average duration between Invoice date and payment date
  
---  

## Module 2

+ In this module we will learn the different ways to represent date/time data and how R stores them internally. 
You will understand what POSIX is and its variations in R. You will explore the Date class which represents calender dates. You will learn how to convert other data types to date/time and the ISO 8601 standard for representing date/time.


+ First we save a date and try to change its type

```{r}
release_date <- 2019-12-12

print(class(release_date))

rm(release_date)

# dont work ! subtract the numbers

release_date <- "2019-12-12"

print(class(release_date))

# still not the class we want

class(Sys.Date())

print(class(now())) # lubridate package

# POSIXct (calender time)
# POSIXlt (local time)

rm(release_date)

```

## Date

+ works only without time or timezone components !
```{r}
unclass(Sys.Date())
# Date as Integer / stored as number of days since 1970-01-01

lubridate::origin

# same principle / stored as number of days since 1970 -01-01


release_date <- as.Date("2019-12-12")

print(class(release_date))

unclass(release_date)

print("Stored as Number (18242) of days since 1970-01-01")

# what happens if the date is pre 1970-01-01

unclass(as.Date("1963-08-28"))  # I have a dream

# stored as negative integers

# convert number to date

# as.Date(18242) -> Error

# origin muss angegeben werden

as.Date(18242, origin = "1970-01-01")

as.Date(7285, origin = "2000-01-01" ) 

as.Date(0, origin = Sys.Date())

# Specify each value of ISO 8601

ISOdate(year = 2019,
        month = 12,
        day = 12,
        hour = 8,
        min = 5,
        sec = 3,
        tz = "CEST")
```
# POSIXct - Calender Time

+ Portable Operating System Interface (Family of Standards specified for maintaining comparable data between different operating systems)


```{r}
# POSIX

class(now())  #lubridate function


unclass(now())

# [1] = stored as number of seconds (!) since base (1970-01-01)

# POSIXct

as.POSIXct("2020-06-14 17:48:30") # store a Date with Hour, Min, Sec Component


release_date <- as.POSIXct("2020-06-14 17:48:30")


unclass(release_date)



```
# POSIIXlt - Time as list


```{r}


# Same Informartion (some additional) as POSIXct but stored in a list
# also stores day of a week and day of the year and offset
# POSIXct in contrary stores only seconds after base!


release_date <- as.POSIXlt("2020-06-14 17:48:30")

unclass(release_date)


# get a list/vector of that POSIXlt

unlist(release_date)

# $year = yearas from 1900

# $mon = Jan(0) - Dec (11)# extract components

release_date$hour
release_date$mon
#....


# some further tipps

# use date class when there is no time component

# when you are dealing with time and time zones then we can use POSIXct/POSIXlt class

# if we want to extract the different components (mon, day, sec etc.) we use POSIXlt
```


# Test 1 Date / POSIXct / POSIXlt

+ R1.0.0 was released on 2000-02-09 08:55:23 UTC. Save it as:

- Date

```{r}
as.Date("2000-02-09")
```



- Date using origin 

```{r}
as.Date(18242, origin = "2000-02-09")
```

- POSIXct

```{r}
as.POSIXct("2000-02-09 08:55:23")
```

- POSIXlt and extract "month day", "day of year", "month", "zone"


```{r}
release_date <- as.POSIXlt("2000-02-09 08:55:23")

release_date$mday
release_date$yday
release_date$mon
release_date$zone




```


- ISODate


```{r}
ISOdate(year = 2000,
        month = 02,
        day = 09,
        hour = 8,
        min = 55,
        sec = 23,
        tz = "CET")
```

