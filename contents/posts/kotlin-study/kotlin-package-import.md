---
title: "😎 4. 코틀린 패키지와 임포트(package and import)"
description: "코틀린의 패키지와 임포트 관련 내용 "
date: 2023-07-07
update: 2023-07-07
tags:
  - kotlin
  - study
series: "코틀린 공부 기록"
---

## 패키지와 임포트

### 패키지(package)

![package import(Do it! 코틀린 프로그래밍 자료)](https://github.com/C0ribo/code-blog/issues/1#issue-1792906771)

패키지는 모듈 아래의 개념으로 자바처럼 소스파일의 첫 줄에 한다. 
패키지는 지정하지 않으면 해당 파일의 콘텐츠는 이름이 없는 기본 패키지에 해당 한다.

```kotlin
package org.example

fun printMessage() { /*...*/ }
class Message { /*...*/ }
```

패키지는 `package` 키워드로 시작하고 점(`.`)으로 구별으로 패키지 전체 이름이 뒤에 온다. 패키지를 이루는 최상위 선언에는 `타입`, `함수`, `프로퍼티`가 있다. 

#### 기본 패키지

기본 패키지란 코틀린 프로그래밍에서 자주 사용하는 클래스와 함수 등을 미리 만들어 둔 것으로 `import`를 별도로 사용하지 않아도 바로 사용할 수 있다. 왜냐하면 `import`문을 사용하지 않아도 자동적으로 `import`가 되는 패키지이기 때문이다.
- `kotlin.*` : Any, Int, Double 와 같은 핵심 함수와 자료형
- `kotlin.text.*` : 문자와 관련된 api
- `kotlin.sequences.*` : 컬렉션 자료형의 하나로 반복이 허용되는 개체를 열거
- `kotlin.ranges.*` : if문이나 for문에서 사용할 범위 관련 요소
- `kotlin.io.*` : 입출력 관련 api
- `kotlin.collections.*` : List, Set, Map 등의 컬렉션
- `kotlin.annotation.*` : 애노테이션 관련 api

> 자바에서는 패키지 구조와 컴파일 대상 루트에 있는 소스 트리 디렉터리 구조가 같아야 한다. 둘의 경로가 다르면 컴파일 오류가 발생한다. 

### 임포트(import)

`import`는 단일 이름만 가져올 수 있으며 자바와 달리 코틀린은 타입 멤버를 `import` 하는 별도의 `import static` 구문을 가지고 있지 않는다. 이런 모든 선언들은 정규 `import` 키워드를 사용하여 임포트된다.

임포트 디렉티브가 클래스나 함수 등의 최상위 선언만 임포트할 수 있는 것은 아니다. 또한, 다른 선언도 가능하다.
- 최상위 함수와 속성
- 객체 선언에 선언된 함수와 속성
- 열거형(enum) 상수

기본 임포트외에 다른 패키지의 클래스나 함수를 사용하기 위해서는 `import`를 하거나 전체 이름을 입력해야 한다.
하나의 이름을 `import` 할 수 있다.(필요한 클래스나 함수들의 전체 이름을 직접 명시한다.)

```kotlin
import foo.Bar // Bar에 전체 이름없이 접근할 수 있다.
```

범위를 지정하여 범위 내의 모든 컨텐츠(`package`, `class`, `object`등)에 접근할 수 있게 임포트한다.
```kotlin
import foo.* // foo패키지 내 모든 내용에 접근 가능하다.
```
또 다른 형태의 임포트로 어떤 영역에 속한 모든 선언을 한꺼번에 임포트할 수 있다. 전체 이름 뒤에 (`*`)을 붙이면 된다.
```kotlin
import kotlin.math.* // kotlin.math 패키지 안에 있는 모든 선언을 임포트
```

#### as

코틀린에서는 이름이 충돌하는 경우 `as` 키워드를 사용해서 충돌을 없앨 수 있다.
```kotlin
import foo.Bar // Bar에 접근할 수 있다.
import bar.Bar as bBar // bBar는 'bar.Bar'의 약어이다.
```