---
title: RDP 무차별 대입 공격을 직접 하고 로그로 추적하기
date: 2026-11-12 07:30:00 +0900
categories: [Purple Team, Windows]
tags: [rdp, hydra, nmap, eventlog, "4625", "4624"]
published: false
---

## 1. 목표

공격자 입장에서 RDP 비밀번호를 대입해 보고, 방어자 입장에서 Windows 보안 로그만으로 공격을 재구성

## 2. 실습 환경

공격자
Kali Linux : 192.168.128.3

피해자
Windows 11 (계정 victim, RDP 켜짐) : 192.168.128.2



- 맥북 UTM 가상머신, Host Only 네트워크 (인터넷 차단)
- Windows 감사 정책: 로그온 성공·실패 기록

## 3. 실습 과정

공격자 -> 피해자

1. 포트 스캔 (nmap)

```bash
nmap 192.168.128.2
```

![nmap 결과](/assets/lab-nmap.png)


999개의 포트는 막혀있고(filtered) 3389/tcp(RDP)만 열려 있었다

이후 `-sV`(서비스 버전 확인), `-p`(포트 지정) 플래그 사용하여 3389 포트를 정해서 스캔함

2. 비밀번호 대입 (Hydra)

```bash
hydra -t 1 -V -l victim -P pw.txt rdp://192.168.128.2
```

- `-l victim` : 사용자 이름 지정
- `-P pw.txt` : 비밀번호 목록 파일
- `-t 1` : 한 번에 1개씩 시도
- `-V` : 시도 과정 출력

![hydra 결과](/assets/lab-hydra.png)

8번째 시도에서 `victim / 1234` 로 성공했다. 걸린 시간은 15초

## 4. 결과 분석 (방어자)

Windows -> 이벤트 뷰어 -> 이벤트 ID `4625,4624` 필터

- `4624` : 로그인 성공
- `4625` : 로그인 실패

![로그 목록](/assets/lab-log-list.png)


오후 8:31:07 ~ 8:31:20 

- 이벤트
4625 로그 7개 확인 -> 로그인 7번 실패, 약 2초 간격으로

오후 8:31:22
- 이벤트
4624 로그 -> victim 로그인 성공

오후 8:31:22 
-이벤트
4634 로그 -> 같은 세션 로그오프 (0초)












