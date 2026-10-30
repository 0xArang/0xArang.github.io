---
title: 제목
date: 2026-10-30 21:00:00 +0900
categories: [Blue Team, Windows]
tags: [windows, eventlog, 4625, powershell]
published: false
---

## 1. 이 작업을 왜 해보았을까?

## 2. 준비 과정

```powershel
auditpol /get /subcategory:"로그온"
```


## 3. 로그인 실패 만들기

![runas 1326 에러](/assets/win-runas-1326.png)

![testuser로 실행 성공](/assets/win-runas-success.png)

## 4. 이벤트 뷰어로 확인

![이벤트 뷰어 목록](/assets/win-eventviewer-list.png)

### 화면은 같은데 로그가 다름

![4625 testuser](/assets/win-4625-testuser.png)

![4625 nouser](/assets/win-4625-nouser.png)

### 예상과 달랐던점

### 성공한 로그인

![4624 자세히](/assets/win-4624-detail.png)

## 5. powershell로 한번에 찾기

![Powershell 하위상태표](/assets/win-ps-substatus.png)

### 이름으로 거르면 놓치는 로그

![4732](/assets/win-4732.png)

## 6. 정리





