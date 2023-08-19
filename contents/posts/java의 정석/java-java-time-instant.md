---
title: "[Java] Instant"
description: "Instant"
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

## Instant

- 에포크 타입(EPOCH TIME, 1970-01-01 00:00:00 UTC)부터 경과된 시간을 나노초 단위로 표현
- 단일 진법으로만 다룸

```java
Istant now = Instant.now();
Instant now2 = Instant.ofEpochSecond(now.getEpochSecond());
Instant now3 = Instant.ofEpochSecond(now.getEpochSecond(), now.getNano());
```

- Instant를 생성 시 `now()`와 `ofEpochSecond()` 사용

```java
long epochSec = now.getEpochSecond();
int nano = now.getNano();
```

- Instant는 시간을 초 단위와 나노초 단위로 나누어 저장한다.
- 밀리초 단위의 `EPOCHTIME`을 필요로 하는 경우를 위해 `toEpochMilli()` 정의

```java
long toEpochMilli()
```

### Instant와 Date간의 변환

```java
static Date from(Instant instant) // Instant -> Date
Instant toInstant() // Date -> Instant
```

## 참조

- 자바의 정석