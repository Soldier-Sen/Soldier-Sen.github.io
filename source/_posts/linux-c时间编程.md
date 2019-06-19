---
title: linux c时间编程
date: 2019-06-11 23:12:23
tags: [linux]
categories: 时间编程
---

# 1.Linux下常用时间类型有四种： 
    time_t 、struct  tm、struct  timeval 、struct  timespec
## time_t 时间类型
    #include <time.h>
    typedef    long  time_t
说明  : 实际是一个长整型，其值表示从1970年1月1日00分00秒到当前时刻的秒数，由于time_t类型长度限制，在32位机子上，最大为0x7fffffff, 它表示的时间最大为Tue Jan 19 03:14:07 2038（GMT）。

注：GMT时间也称UTC时间，其实UTC和GMT两者几乎一个概念.

## struct  tm 时间类型
    #include <time.h>
    struct tm {
        int tm_sec;    /* Seconds (0-60) */
        int tm_min;    /* Minutes (0-59) */
        int tm_hour;   /* Hours (0-23) */
        int tm_mday;   /* Day of the month (1-31) */
        int tm_mon;    /* Month (0-11) */
        int tm_year;   /* Year - 1900 */
        int tm_wday;   /* Day of the week (0-6, Sunday = 0) */
        int tm_yday;   /* Day in the year (0-365, 1 Jan = 0) */
        int tm_isdst;  /*夏令时标识符，使用夏令时，tm_isdst为正，不使用夏令时，tm_isdst为0，不了解情况时>，tm_isdst为负*/ 
    };

# 2.Linux常用时间函数

## ctime 函数与 ctime_r 函数
    #include <time.h>
    char *ctime(const time_t *timep);
    char *ctime_r(const time_t *timep, char *buf);

## gmtime 函数与 gmtime_r 函数
    #include <time.h>
    struct tm *gmtime(const time_t *timep);
    struct tm *gmtime_r(const time_t *timep, struct tm *result);

## asctime 函数与 asctime_r 函数
    #include <time.h>
    char *asctime(const struct tm *tm);
    char *asctime_r(const struct tm *tm, char *buf);

## localtime 函数与 localtime_r 函数
    #include <time.h>
    struct tm *localtime(const time_t *timep);
    struct tm *localtime_r(const time_t *timep, struct tm *result);
## mktime 函数
    #include <time.h>
    time_t mktime(struct tm *tm);
