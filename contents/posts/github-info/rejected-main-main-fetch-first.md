---
title: "🤔 1. [rejected] main -> main (fetch first) 해결"
description: "git push하다가 생긴 오류 해결 기록"
date: 2023-07-04
update: 2023-07-04
tags:
  - github
  - commit
series: "github 관련 기록"
---

## 1. 상황

글 하나를 작성하고 난 뒤에 `git push`를 하려고 하니까 오류가 생겼다.
```
![rejected] main -> main (fetch first)
error: 레퍼런스를 'https://github.com/[github 아이디]/[github 레퍼런스]
```

## 2. 원인

로컬 저장소와 원격 저장소가 동기화 되지 않아서 발생한다.
기존 데이터가 손실될 수 있어서 방지하기 위해 push를 막아서 에러가 나타나는 것이다.

## 3. 해결

### 3-1. 동기화 후 push 진행
```
git pull --rebase origin main
```

### 3-2. 강제로 push 진행
```
git push origin +main
```

> 강제로 `push`할 경우에 동기화되지 않은 작업내용이 소실될 수 있다.

## 참조

- [main->main(fetch first)](https://byul91oh.tistory.com/231)
- [main->main(fetch first2)](https://dewworld27.tistory.com/entry/rejected-main-main-fetch-first-GIT-PUSH-%EC%97%90%EB%9F%AC-%ED%95%B4%EA%B2%B0%EB%B2%95-1)