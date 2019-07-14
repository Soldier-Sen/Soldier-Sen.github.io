---
title: linux c时间编程
date: 2019-06-11 23:12:23
tags: [linux]
categories: 时间编程
---

# 1. Linux下常用时间类型有四种： 
    time_t 、struct  tm、struct  timeval 、struct  timespec
## time_t 时间类型
    #include <time.h>
    typedef    long  time_t
说明  : 实际是一个长整型，其值表示从1970年1月1日00分00秒到当前时刻的秒数，由于time_t类型长度限制，在32位机子上，最大为0x7fffffff, 它表示的时间最大为Tue Jan 19 03:14:07 2038（GMT）。

<!-- more -->

注：GMT时间也称UTC时间，其实UTC和GMT两者几乎一个概念。UTC（Coordinated Universal Time)世界标准时间 即 GMT(Greenwich Mean Time)格林威治标准时间。

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

## struct timeval 时间类型
    #include <time.h>
    struct timeval {
        time_t      tv_sec;     /* seconds */
        suseconds_t tv_usec;    /* microseconds */
    };
    struct timezone {
        int tz_minuteswest;     /* minutes west of Greenwich */
        int tz_dsttime;         /* type of DST correction */
    };

说明：设置时间函数settimeofday()与获取时间函数gettimeofday()均使用struct timeval类型作为参数。

## struct timespec 时间类型
    #include <time.h>
    strcut timespec {
        time_t tv_sec;  //seconds
        long tv_nsec;   //and nanoseconds
    }
struct timespec有两个成员，一个是秒，一个是纳秒, 所以最高精确度是纳秒。
一般由函数int clock_gettime(clockid_t, struct timespec *)获取特定时钟的时间。
    常用如下4种时钟：
    CLOCK_REALTIME 统当前时间，从1970年1.1日算起；
    CLOCK_MONOTONIC 系统的启动时间，不能被设置；
    CLOCK_PROCESS_CPUTIME_ID 本进程运行时间；
    CLOCK_THREAD_CPUTIME_ID 本线程运行时间；
# 2. Linux常用时间函数
Linux下常用时间函数有：time(), ctime(), gmtime(), localtime(), mktime(), asctime(),difftime(), gettimeofday(), settimeofday().

部分时间相关函数中，带有"_r“的函数为线程安全 ，与之对应的函数不是线程安全的，多线程应用里建议使用xxx_r函数。

## 2.1 time 函数
    #include <time.h>
    time_t time(time_t *tloc); //获取日历时间。
说明：该函数返回从1970年1月1日00时00分00秒至今所经过的秒数。如果time_t *tloc为非空指针，函数也会将返回值存到tloc指针指向的内存。
返回：成功返回秒数，失败返回-1值，错误原因存于erron中。
### 例子：
```
#include <stdio.h>
#include <time.h>
int main() {
    time_t t;
    t = time(NULL);
    printf("ctime:%s\n", ctime(&t));
    return 0;
}
```
输出：
```
time=1561160476
```

## 2.2 ctime 函数与 ctime_r 函数
    #include <time.h>
    char *ctime(const time_t *timep);
    char *ctime_r(const time_t *timep, char *buf);
说明：
返回：一个表示当地时间的字符串，格式为：[Www Mmm dd hh:mm:ss yyyy\n]，输出自动带一个换行符。
Www: 一周的一天（之一Mon，Tue，Wed，Thu，Fri，Sat，Sun）
MMM: 月(Jan, Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov, Dec)
dd : 一个月中的一天
hh : 小时
mm : 分钟
ss : 秒
yyyy : 年

### 例子2.2.1：
```
#include <stdio.h>
#include <time.h>
int main() {
    time_t t;
    t = time(NULL);
    printf("ctime: %s\n", ctime(&t));
    return 0;
}
```

输出：
```
ctime: Sat Jun 22 15:14:27 2019
```

## 2.3 gmtime 函数与 gmtime_r 函数
    #include <time.h>
    struct tm *gmtime(const time_t *timep);
    struct tm *gmtime_r(const time_t *timep, struct tm *result);
说明：将获取到的日历时间转换为GMT时间，格式为struct tm结构；
返回：返回struct tm格式的GMT时间；

### 例子2.3.1：
```
#include <stdio.h>
#include <time.h>
int main() {
    time_t t;
    t = time(NULL);
    printf("time:%ld\n", t);

    struct tm *local = NULL;
    local = localtime(&t);
    printf("localtime:%04d-%02d-%02d %02d:%02d:%02d\n", local->tm_year+1900, local->tm_mon, local->tm_mday, local->tm_hour, local->tm_min, local->tm_sec);
    
    printf("asctime:%s", asctime(local));
    
    time_t t2 = mktime(local);
    printf("mktime:%ld\n\n", t2);
    
    local = gmtime(&t);
    printf("gmtime:  %04d-%02d-%02d %02d:%02d:%02d\n", local->tm_year+1900, local->tm_mon, local->tm_mday, local->tm_hour, local->tm_min, local->tm_sec);
    
    printf("asctime:%s", asctime(local));
    return 0;
}
```
输出：
```
time:1561173424
localtime:2019-05-22 11:17:04
asctime:Sat Jun 22 11:17:04 2019
mktime:1561173424

gmtime:  2019-05-22 03:17:04
asctime:Sat Jun 22 03:17:04 2019
```
本次ubuntu主机设置时区为东八区，所以本地时间与GMT时间有8小时的时差。

## 2.4 asctime 函数与 asctime_r 函数
    #include <time.h>
    char *asctime(const struct tm *tm);
    char *asctime_r(const struct tm *tm, char *buf);
说明：将本地时间转换为符串形式；
返回：时间字符串。
例子参见"例子2.3.1"

## 2.5 localtime 函数与 localtime_r 函数
    #include <time.h>
    struct tm *localtime(const time_t *timep);
    struct tm *localtime_r(const time_t *timep, struct tm *result);
说明：将获取到的日历时间转换为本地时间，结构为struct tm结构；
返回：返回为struct tm类型的指针；
例子参见"例子2.3.1"

## 2.6 mktime 函数
    #include <time.h>
    time_t mktime(struct tm *tm);
说明：将struct tm结构的本地时间转为日历时间，即经过的秒数；
返回：返回日历时间；

例子参见"例子2.3.1"

## 2.7 gettimeofday 函数与 settimeofday 函数
    #include <sys/time.h>
    int gettimeofday(struct timeval *tv, struct timezone *tz);
    int settimeofday(const struct timeval *tv, const struct timezone *tz);
    
    struct timeval {
        time_t      tv_sec;     /* seconds */
        suseconds_t tv_usec;    /* microseconds */
    };
    struct timezone {
        int tz_minuteswest;     /* minutes west of Greenwich */
        int tz_dsttime;         /* type of DST correction */
    };
说明： 获取/设置本地时间，可达微秒级。gettimeofday获取当前时间存于struct timeval结构体中，相应的时区信息存于struct timezone结构体中。注意： tz是依赖系统的，不同系统可能获取不到，因此通常设置为NULL。
返回：

### 例子2.7.1：
```
#include <stdio.h>
#include <sys/time.h>
#include <time.h>

int main() {
    struct timeval tv;
    struct timezone tz;
    gettimeofday(&tv, &tz);
    printf("time(NULL)；%ld\n", time(NULL));
    printf("tv_sec:%ld, tv_usec:%ld\n", tv.tv_sec, tv.tv_usec);
    printf("dsttime:%d, minuteswest:%d\n", tz.tz_dsttime, tz.tz_minuteswest);
    return 0;
}
```
输出：
```
time(NULL)；1561190012
tv_sec:1561190012, tv_usec:285375
dsttime:0, minuteswest:0
```

### 例子2.7.2：
```
#include <stdio.h>
#include <sys/time.h>
#include <unistd.h>

int main() {
    struct timeval tv, set1;
    gettimeofday(&tv, NULL);
    system("date");

    set1.tv_sec =  tv.tv_sec - 30;
    settimeofday(&set1, NULL);
    system("date");

    return 0;
}

```
输出
```
2019年 06月 22日 星期六 16:08:42 CST
2019年 06月 22日 星期六 16:08:12 CST
```

# 2.8 stime 函数
    #include <time.h>
    int stime(const time_t *t);
说明：可以设置系统时间，精确到秒。
返回：成功返回0，错误返回-1，errno错误码，EFAULT表示传递的参数错误，如时间值是无效的值，EPERM表示权限不够，注意只有root用户才有修改系统时间的权限。如果要让普通程序修改系统时间，可以先切换到root用户操作，修改完成后，再切换到普通用户，或者用命令chmod +s给执行文件加上root用户的权限。

### 例子2.8.1：
```
#include <stdio.h>
#include <sys/time.h>
#include <unistd.h>

int main() {
    time_t t;
    t = time(NULL);
    system("date");

    t =  t - 30;
    stime(&t);
    system("date");

    return 0;
}

```
