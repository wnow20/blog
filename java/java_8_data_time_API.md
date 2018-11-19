title: Java 8 Date/Time API 实用笔记
date: 2018-11-19 10:06:00
tags: [java]
categories: [java]

---

## LocalDate
### 创建

```java
LocalDate localDate = LocalDate.now();
```

### 指定年月日
```java
LocalDate.of(2015, 02, 20);
 
LocalDate.parse("2015-02-20");
```

### 操作

**加一天**
```java
LocalDate tomorrow = LocalDate.now().plusDays(1);
```

**减一个月**
```java
LocalDate previousMonthSameDay = LocalDate.now().minus(1, ChronoUnit.MONTHS);
```

### 获取

```java
// 一个星期的第几天
DayOfWeek sunday = LocalDate.parse("2016-06-12").getDayOfWeek();

// 一个月的第几天
int twelve = LocalDate.parse("2016-06-12").getDayOfMonth();

// 一天的开始时间
LocalDateTime beginningOfDay = LocalDate.parse("2016-06-12").atStartOfDay();

// 指定月份的第一有
LocalDate firstDayOfMonth = LocalDate.parse("2016-06-12")
  .with(TemporalAdjusters.firstDayOfMonth());
```

### 判断

**是不是闰年**
```java
boolean leapYear = LocalDate.now().isLeapYear();
```

**比较**
```java
boolean notBefore = LocalDate.parse("2016-06-12")
  .isBefore(LocalDate.parse("2016-06-11"));
 
boolean isAfter = LocalDate.parse("2016-06-12")
  .isAfter(LocalDate.parse("2016-06-11"));
```

## LocalTime

### 创建
```java
LocalTime now = LocalTime.now();
```

### 指定时间
```java
LocalTime sixThirty = LocalTime.parse("06:30");

LocalTime sixThirty = LocalTime.of(6, 30);
```

### 操作

**加一个小时**

```java
LocalTime sevenThirty = LocalTime.parse("06:30").plus(1, ChronoUnit.HOURS);
```

### 获取
```java
int six = LocalTime.parse("06:30").getHour();
```

### 判断
```java
boolean isbefore = LocalTime.parse("06:30").isBefore(LocalTime.parse("07:30"));
```

## LocalDateTime
### 创建
```java
LocalDateTime.now();
```

### 指定
```java
LocalDateTime.of(2015, Month.FEBRUARY, 20, 06, 30);
LocalDateTime.parse("2015-02-20T06:30:00");
```

### 操作
```java
// 加一天
localDateTime.plusDays(1);

// 减两个小时
localDateTime.minusHours(2);
```

### 获取
```java
// 获取月份
localDateTime.getMonth();
```

## ZonedDateTime
```java
// create a Zone for Paris
ZoneId zoneId = ZoneId.of("Europe/Paris");
```

```java
// A set of all zone ids can be obtained
Set<String> allZoneIds = ZoneId.getAvailableZoneIds();
```

```java
// convert to a specific zone
ZonedDateTime zonedDateTime = ZonedDateTime.of(localDateTime, zoneId);
```

```java
// parse to get time zone specific date time:
ZonedDateTime.parse("2015-05-03T10:15:30+01:00[Europe/Paris]");
```

```java
// create OffSetDateTime using ZoneOffset
LocalDateTime localDateTime = LocalDateTime.of(2015, Month.FEBRUARY, 20, 06, 30);
ZoneOffset offset = ZoneOffset.of("+02:00");
OffsetDateTime offSetByTwo = OffsetDateTime.of(localDateTime, offset);
```

## Period & Duration

### Period
```java
LocalDate initialDate = LocalDate.parse("2007-05-10");

LocalDate finalDate = initialDate.plus(Period.ofDays(5));

int five = Period.between(finalDate, initialDate).getDays();

// int five = ChronoUnit.DAYS.between(finalDate , initialDate);
```

### Duration
```java
LocalTime initialTime = LocalTime.of(6, 30, 0);

LocalTime finalTime = initialTime.plus(Duration.ofSeconds(30));

int thirty = Duration.between(finalTime, initialTime).getSeconds();

// int thirty = ChronoUnit.SECONDS.between(finalTime, initialTime);
```

## Compatibility with Date and Calendar
```java
// Date convert
LocalDateTime.ofInstant(date.toInstant(), ZoneId.systemDefault());
// Calendar convert
LocalDateTime.ofInstant(calendar.toInstant(), ZoneId.systemDefault());
```

```java
// epoch seconds convert
LocalDateTime.ofEpochSecond(1465817690, 0, ZoneOffset.UTC);
```

```java
// convert to Date
OffsetDateTime odt = OffsetDateTime.now(ZoneId.systemDefault());
return Date.from(localDateTime.toInstant(odt.getOffset()));
```

## Date and Time Formatting
```java
// work with DateTimeFormatter
LocalDateTime localDateTime = LocalDateTime.of(2015, Month.JANUARY, 25, 6, 30);
String localDateString = localDateTime.format(DateTimeFormatter.ISO_DATE);

// custom patterns
localDateTime.format(DateTimeFormatter.ofPattern("yyyy/MM/dd"));

// format style
localDateTime
  .format(DateTimeFormatter.ofLocalizedDateTime(FormatStyle.MEDIUM)
  .withLocale(Locale.UK);
```

## Backport
[threeten](https://www.threeten.org/) Java 6/7 使用 Java 8 的Date/Time API

```xml
<dependency>
    <groupId>org.threeten</groupId>
    <artifactId>threetenbp</artifactId>
    <version>1.3.1</version>
</dependency>
```
