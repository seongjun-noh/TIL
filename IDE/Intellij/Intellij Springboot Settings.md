---
title: IntelliJ Spring Boot 설정 가이드
categories: IDE
comments: true
tags:
    - "#IntelliJ"
    - "#SpringBoot"
    - "#IDE"
---

## JSP 설정

### Working Directory 설정
JSP 사용 시 Working Directory를 다음과 같이 설정해야 합니다:
```
$MODULE_WORKING_DIR$
```

## 콘솔 설정

### 콘솔 색상 활성화
Spring Boot 로그의 색상을 활성화하려면 다음 VM 옵션을 추가합니다:
```
-Dspring.output.ansi.enabled=ALWAYS
```

