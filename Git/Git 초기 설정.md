---
title: Git 초기 설정 가이드
categories: Git
comments: true
tags:
    - "#Git"
    - "#Configuration"
    - "#Development"
---

## 기본 설정

### 1. 줄바꿈 설정
윈도우와 맥의 엔터방식 차이로 인한 오류 방지:
```bash
git config --global core.autocrlf true
```

### 2. 사용자 정보 설정
사용자 이름과 이메일 주소 설정:
```bash
git config --global user.name "(본인 이름)"
git config --global user.email "(본인 이메일)"
```

### 3. 설정 확인
현재 Git 설정 확인:
```bash
git config --list
```

## 추가 설정

### 1. SSL 설정
SSL 인증서 검증 비활성화:
```bash
git config --global http.sslVerify false
```
