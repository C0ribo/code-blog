---
title: "[Java] 패키지(package)와 import"
description: "패키지 , import, import문, static import"
date: 2023-07-30
update: 2023-07-30
tags:
  - java
  - study
series: "Java 기초"
---

```
개인공부 용도로 쓰이는 글이라 자바의 정석 교재를 밑바탕으로 쓰였습니다. 
오류 및 오타가 난무할 수 있으며, 많이 부족한 글입니다.
```

## package와 import

### 패키지(package)

패키지는 관련된 클래스끼리 묶어 저장하는 디렉터리와 같다. 같은 이름의 클래스라도 패키지가 다르면 완전 다른 파일로 인식한다. 

클래스 이름만으로 클래스를 구분했었지만 사실은 클래스 이름에 패키지명을 포함한 것이다. 

클래스가 물리적으로 하나의 클래스파일(.class)인 것과 같이 패키지는 물리적으로 하나의 디렉토리이다.

패키지는 점`.`으로 구분하며 `[상위 패키지].[하위 패키지].[클래스]`로 나뉜다. 
예로 들면 `java.lang.String`이 있다면 `java\lang\String` 과도 같다.

```
- 하나의 소스파일에는 첫번째 문장으로 단 한 번의 패키지 선언만을 허용
- 모든 클래스는 반드시 하나의 패키지에 속해야 함
- 패키지는 점(.)을 구분자로 하여 계층구조로 구성
- 패키지는 물리적으로 클래스 파일(.class)을 포함하는 하나의 디렉토리
```

### 패키지의 선언

패키지는 클래스나 인터페이스의 소스파일(.java)의 맨 위에 한 줄로만 적어준다.
```java
package 패키지명;
```

하나의 소스파일에 한번만 선언할 수 있다. 그 하나로 해당 소스파일에 포함된 모든 클래스나 인터페이스가 속하게 된다. 

패키지명은 **숫자와 특수문자(`_`와 `$` 제외)는 사용이 불가능**하며 모두 **소문자를 사용**한다. 

여태 패키지를 선언하지 않아도 문제가 없었던 것은, 기본적으로 `이름없는 패키지`를 제공하고 있기 때문이다. 패키지를 지정하지 않은 모든 클래스들은 같은 패키지에 속하게 되는 것이다. 

### import문

import문은 **컴파일러에게 소스파일을 사용한 클래스의 패키지에 대한 정보를 제공**한다. 

다른 패키지의 클래스를 사용하려면 **패키지명이 포함된 클래스 이름을 사용**해야 한다. 클래스의 패키지를 미리 명시해주면 클래스이름에서 패키지명은 생략할 수 있다. 

> import문은 많이 사용할수록 컴파일 시간이 조금 더 걸릴 뿐이다. 

### import문의 선언

import문은 package문과 달리 한 소스파일에 여러 번 선언할 수 있다. 

##### 일반적인 소스파일(*.java)의 구성
```
(1) package문
(2) import문
(3) 클래스 선언
```

##### **import문 선언 방법**  
```
import 패키지명.클래스명;
    또는
import 패키지명.*;
```

import와 패키지명을 생략하고자 하는 클래스의 이름을 패키지명과 함께 써주면 된다. 

```java
import java.util.Calendar; // java.util 패키지에 있는 Calendar클래스만 가져와 사용

import java.util.Date; // java.util 패키지에 있는 Date클래스만 가져와 사용

import java.util.ArrayList; // java.util 패키지에 있는 ArrayList클래스만 가져와 사용

import java.util.*; // java.util 패키지에 있는 클래스 소스 파일을 모두 사용
```
한 패키지에서 여러 클래스를 사용하는 경우 클래스의 이름을 일일이 지정해주는 것보다 `패키지명.*` 을 사용하는 것이 편리하다. 하지만 패키지가 많을 경우 구별하기 힘든 단점이 있다. 

import문에서 클래스의 이름 대신 `*`을 사용하는 것이 <span style="color:red">하위 패키지의 클래스까지 포함하는 것은 아니</span>라는 것이다.

```java
import java.util.*;
import java.text.*;

import java.*; // 사용X
```

> `java.lang` 패키지는 많이 쓰이는 중요 클래스들이 속한 패키지이기 때문에 따로 import문을 정하지 않아도 된다. 


### static import문

static import문을 사용하면 static멤버를 호출할 때 정적 메서드나 필드를 클래스명 없이 생략할 수 있다. 

```java
import static java.lang.Integer.*; // Integer클래스의 모든 static메서드
import static java.lang.Math.random; // Math.random()만. 괄호는 안붙인다.
import static java.lang.System.out; // System.out을 out만 참조가능
```

만약 위의 코드처럼 static import문을 선언했다면 다음과 같이 바뀌게 된다.

```java
System.out.println(Math.random()); // 전

out.println(random()); // 후
```

## 참조 

- 자바의 정석 
- [package and import 블로그](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EA%B0%9D%EC%B2%B4-%EC%A7%80%ED%96%A5OOP-%ED%81%B4%EB%9E%98%EC%8A%A4-%EB%AC%B8%EB%B2%95-%F0%9F%92%AF-%EC%B4%9D%EC%A0%95%EB%A6%AC#super_%ED%82%A4%EC%9B%8C%EB%93%9C)