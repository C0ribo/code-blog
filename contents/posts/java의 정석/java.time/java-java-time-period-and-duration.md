---
title: "[Java] Period와 Duration"
description: "Period, Duration"
date: 2023-08-21
update: 2023-08-21
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다.
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```

**Period**는 날짜의 차이, **Duration**은 시간의 차이

```markdown
날짜 - 날짜 = Period
시간 - 시간 = Duration
```

## between()

date1과 date2의 차이를 나타내는 Period는 `between()`으로 얻을 수 있다.

```java
LocalDate date1 = LocalDate.of(2014, 1, 1);
LocalDate date2 = LocalDate.of(2015, 12, 31);

Period pe = Period.between(date1, date2);
```

- date1이 date2보다 날짜 상으로 이전이면 **양수**이고, 이후이면 **음수**로 Period에 저장

```java
LocalTime time1 = LocalTime.of(00,00,00);
LocalTime time2 = LocalTime.of(12,34,56); // 12시 34분 56초

Duration du = Duration.between(time1, time2); // 시간 차이 구할때 Duration사용
```

Period, Duration에서 특정 필드의 값을 얻을 때 `get()` 사용

```java
long year = pe.get(ChronoUnit.YEARS); // int getYears()
long month = pe.get(ChronoUnit.MONTHS); //  int getMonths()
long day = pe.get(ChronoUnit.DAYS); // int getDays()

long sec = du.get(ChronoUnit.SECONDS); // long getSeconds()
int nano = du.get(ChronoUnit.NANOS); // int getNano()
```

다만, Period와 달리 Duration에는 `getHours()`, `getMinutes()` 같은 메서드가 없다.

`getUnits()`라는 메서드로 get()에 사용할 수 있는 ChronoUnit의 종류를 확인할 수 있다.

```java
System.out.println(pe.getUnits()); // [Years, Months, Days]
System.out.println(du.getUnits()); // [Seconds, Nanos]
```

Duration에는 `Chrono.SECONDS`와 `Chrono.NANOS`밖에 사용할 수 없다. 직접 계산을 해보자. 

```java
long hour = du.getSeconds() / 3600;
long min = (du.getSeconds() - hour*3600) / 60;
long sec = (du.getSeconds() - hour*3600 - min * 60) % 60;
int nano = du.getNano();
```

이 방법은 안전하지 않다. Duration을 LocalTime으로 변환한 다음에, LocalTime이 가지고 있는 get메서드들을 사용하면 더 간단하게 사용할 수 있다.

```java
LocalTime tmpTime = LocalTime.of(0,0).plusSeconds(du.getSeconds());

int hour = tmpTime.getHour();
int min = tmpTime.getMinute();
int sec = tmpTime.getSecond();
int nano = du.getNano();
```

## between()과 until()

`until()`은 `between()`과 거의 같은 일을 한다. 

`between()`은 **static메서드**이고, `until()`인 **인스턴스 메서드**라는 차이가 있다.

```java
// Period pe = Period.between(today, myBirthDay);
Period pe = today.until(myBirthDay);
long dday = today.until(myBirthDay, ChronoUnit.DAYS);
```

- Period는 **년월일을 분리해서 저장**하기 때문에 D-day를 구하는 경우에 <u>두 개의 매개변수를 받는 `until()`을 사용</u>하는게 낫다.
- 날짜가 아닌 시간에도 `until()`을 사용할 수 있지만, Duration을 반환하는 `until()`은 없다.

```java
long sec = LocalTime.now().until(endTime, ChronoUnit.SECONDS);
```

## of(), with()

- Period : **of(), ofYears(), ofMonths(), ofWeeks(), ofDays()**
- Duration : **of(), ofDays(), ofHours(), ofMinutes(), ofSeconds()**

```java
Period pe = Period.of(1, 12, 31); // 1년 12개월 31일
Duration du = Duration.of(60, ChronoUnit.SECONDS); // 60초
// Duration du = Duration.ofSeconds(60); // 위의 문장과 동일
```

- **with()** : 특정 필드의 값을 변경

```java
pe = pe.withYears(2); // 1년에서 2년으로 변경. withMonths(), withDays()
du. = du.withSeconds(120); // 60초에서 120초로 변경. withNanos()
```

## 사칙연산, 비교연산, 기타 메서드

plus(), minus()외에 곱셈과 나눗셈을 위한 메서드도 있다.

```java
pe = pe.withYears(2).multipliedBy(2); // 1년 빼고, 2배 곱한다
du = du.plusHours(1).dividedBy(60); // 1시간 더하고, 60으로 나눈다.
```

Period에는 날짜의 기간을 표현하기 위한 것으로 나눗셈을 위한 메서드가 없다.

- **isNegative()** : 음수인지 확인
- **isZero()** : 0인지 확인

두 날짜 또는 시간을 비교할 때 어느 쪽이 앞인지 같은지 알 수 있다.

```java
boolean sameDate = Period.between(date1, date2).isZero();
boolean isBefore = Duration.between(time1, time2).isNegative();
```

- **negate()** : 부호를 반대로 변경하는 용도
- **abs()** : 부호를 없애려는 용도

```java
// (1)
du = du.abs();
```

```java
// (2) - Period에는 abs()가 없어서 2번 코드를 사용한다.
if(du.isNegative());
		du = du.negated();
```

(1)번과 (2)번은 같은 코드이다.

```java
pe = Period.of(1, 13, 32).normalized(); // 1년 13개월 32일 -> 2년 1개월 32일
```

- **normalized()** : Period에 있는 메서드로, 월(month)의 값이 12를 넘지 않게 바꿔준다. 일(day)의 길이는 일정하지 않으므로 놔둔다.

### 다른 단위로 변환 toTotalMonths(), toDays(), toHours(), toMinutes()

- to로 시작하는 메서드는 Period와 Duration을 다른 단위의 값으로 변환하는데 사용
- **get()** : 특정 필드의 값을 그대로 가져오는 것이지만, 아래 메서드들을 특정 단위로 변환환 결과로 반환하는 차이가 있다.

> 메서드들의 반환타입은 모두 정수(long타입), 지정된 단위 이하의 값들은 버려진다.
> 

| 클래스 | 메서드 | 설명 |
| --- | --- | --- |
| Period | long toTotalMonths() | 년월일을 월단위로 변환해서 반환(일 단위 무시) |
| Duration | long toDays() | 일단위로 변환해서 변환 |
|  | long toHours() | 시간단위로 변환해서 변환 |
|  | long toMinutes() | 분단위로 변환해서 반환 |
|  | long toMillis() | 천분의 일초 단위로 변환해서 반환 |
|  | long toNanos() | 나노초 단위로 변환해서 반환 |
- LocalDate의 `toEpochDay()`라는 메서드는 Epoch Day인 `1970-01-01`부터 날짜를 세어 반환한다.
    - Period를 사용하지 않고 두 날짜 간 일수를 편리하게 계산가능하다.(두 날짜 모두 Epoch Day이후여야 한다.)

```java
LocalDate date1 = LocalDate.of(2015, 11, 28);
LocalDate date2 = LocalDate.of(2015, 11, 29);

long period = date2.toEpochDay() - date1.toEpochDay(); // 1
```

- LocalTime에도 Dauration을 사용하지 않고 시간차이를 구할 수 있다.

```java
int toSecondOfDay()
long toNanoOfDay()
```

## 참고

- 자바의 정석