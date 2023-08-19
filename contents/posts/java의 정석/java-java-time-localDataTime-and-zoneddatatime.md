---
title: "[Java] LocalDateTime과 ZonedDateTime"
description: "LocalDateTime, ZonedDateTime,  ZoneOffset, OffsetDateTime"
date: 2023-08-19
update: 2023-08-19
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다. 
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```

```java
LocalDate + LocalTime -> LocalDateTime
LocalDateTime + 시간대(time zone) -> ZonedDateTime
```

## LocalDate와 LocalTime으로 LocalDateTime만들기

```java
LocalDate date = LocalDate.of(2015, 12, 31);
LocalTime time = LocalTime.of(12,34,56);

LocalDateTime dt = LocalDateTime.of(date, time);
LocalDateTime dt2 = date.atTime(time);
LocalDateTime dt3 = time.atDate(date);
LocalDateTime dt4 = date.atTime(12, 34, 56);
LocalDateTime dt5 = time.atDate(LocalDate.of(2015, 12,31));
LocalDateTime dt6 = date.atStartOfDay(); // dt6 = date.atTime(0,0,0);
```

LocalDateTime에도 날짜와 시간을 직접 지정할 수 있는 `of()`와 `now()`가 정의되어있다.

```java
// 2015년 12월 31일 12시 34분 56초
LocalDateTime dateTime = LocalDateTime.of(2015, 12, 31, 12, 34, 56);
LocalDateTime today = LocalDateTime.now();
```

## LocalDateTime의 변환

```java
LocalDateTime dt = LocalDateTime.of(2015, 12, 31, 12, 34, 56);
LocalDate date = dt.toLocalDate(); // LocalDateTime -> LocalDate
LocalTime time = dt.toLocalTime(); // LocalDateTime -> LocalTime
```

## LocalDateTime으로 ZonedDateTime만들기

- `ZoneId`는 일광 절약시간(DST, Daylight Saving Time)을 자동적으로 처리해준다.
- LocalDate에 시간 정보를 추가하는 `atTime()`을 쓰면 LocalDateTime을 얻을 수 있는 것처럼, LocalDateTime에 `atZone()`으로 시간대 정보를 추가하면 ZonedDateTime을 얻을 수 있다.

> `Zoneld.getAvailableZonelds()`으로 얻을 수 있다.
> 

```java
ZoneId zid = ZoneId.of("Asia/Seoul");
ZonedDateTime zdt = dateTime.atZone(zid);
System.out.println(zdt); // 2015-11-27T17:47:50.451+09:00[Asia/Seoul]
```

- LocalDate에 `atStartOfDay()` 메서드가 있는데, 이 메서드에 매개변수로 `ZoneId`를 지정해도 ZonedDateTime을 얻을 수 있다.

```java
ZonedDateTime zdt = LocalDate.now().atStartOfDay(zid);
System.out.println(zdt); // 2015-11-27T00:00+09:00[Asia/Seoul]
```

- 특정 시간대의 시간을 알고 싶으면 따로 정해야 한다.

```java
ZoneId nyId = ZoneId.of("America/New_York");
ZonedDateTime nyTime = ZonedDateTime.now().withZoneSameInstant(nyId);
```

`now()` 대신 `of()` 사용하면 날짜와 시간을 지정할 수 있다.

## ZoneOffset

UTC로부터 얼마만큼 떨어져 있는지 `ZoneOffset`으로 표현한다.

예로들어 서울은 +9이고, UTC보다 9시간(32400초=60x60x9초) 빠르다.

```java
ZoneOffset krOffset = ZonedDateTime.now().getOffset();
// ZoneOffset krOffset = ZoneOffset.of("+9"); // ±h, ±hh, ±hhmm, ±hh:mm
int krOffsetInSec = krOffset.get(ChronoField.OFFSET_SECONDS); // 32400초
```

## OffsetDateTime

- ZonedDateTime은 ZoneId로 표현하는데 ZoneId가 아닌 `ZoneOffset`을 사용하는 것이 `OffsetDateTime`이다. 

- `ZoneOffset`은 시간대를 시간의 차이로만 구분한다.

- 같은 지역 내 컴퓨터끼리 데이터를 주고 받을 때, 전송시간을 표현하기에는 `LocalDateTime`이면 충분하겠지만 서로 다른 시간대에 존재하는 컴퓨터간의 통신에는 `OffsetDateTime`이 필요하다. 

```java
ZonedDateTime zdt = ZonedDateTime.of(date, time, zid);
OffsetDateTime odt = OffsetDateTime.of(date, time, krOffset);

// ZonedDateTime -> OffsetDateTime
OffsetDateTime odt = zdt.toOffsetDateTime();
```

OffsetDateTime은 ZonedDateTime처럼, LocalDate와 LocalTime에 ZoneOffset을 더하거나, **ZonedDateTime에 toOffsetDateTime()을 호출**해서 얻을 수 있다.

## ZonedDateTime의 변환

**ZonedDateTime**도 LocalDateTime처럼 날짜와 시간에 관련된 다른 클래스로 변환하는 메서드들을 갖고 있다.

```java
LocalDate toLocalDate()
LocalTime toLocalTime()
LocalDateTime toLocalDateTime()
OffsetDateTime toOffsetDateTime()
long toEpochSecond()
Instant toInstant()
```

**GregorianCalendar**와 가장 유사한 것이 **ZonedDateTime**이기 때문에 둘사이의 변환방법만 안다면 다른 날짜와 시간 클래스들로 변환할 수 있다.

```java
// ZonedDateTime -> GregoeianCalendar
GregorianCalendar from(ZonedDateTime zdt)

// GregorianCalendar -> ZonedDateTime
ZonedDateTime toZonedDateTime()
```