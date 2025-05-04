---
title: JSP 수정사항 재시작 없이 반영하기
categories: Spring
comments: true
tags:
    - "#SpringBoot"
    - "#JSP"
    - "#Development"
---

## JSP 개발 모드 설정

JSP 파일의 수정사항을 서버 재시작 없이 즉시 반영하려면 application.yml에 다음 설정을 추가해야 합니다.

### Spring Boot 2.x 이하
```yaml
server:
  jsp-servlet:
    init-parameters:
      development: true
```

### Spring Boot 2.x 이상
```yaml
server:
  servlet:
    jsp:
      init-parameters:
        development: true
```

이 설정을 통해 JSP 파일을 수정할 때마다 자동으로 변경사항이 반영됩니다.
