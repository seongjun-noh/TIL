# Tomcat 버전 정보 노출 방지

## 1. 응답 헤더 버전 노출 제거
응답 헤더에서 버전 정보가 노출되지 않도록 설정하려면 Tomcat의 기본 설정 파일인 server.xml 파일을 수정해야 합니다.

server.xml 파일의 Connector 하위 항목에서 "server" 설정을 추가하면 됩니다.

### server.xml 설정 예시
```xml
<Connector port="8080" protocol="HTTP/1.1"
    maxThreads="250"
    connectionTimeout="20000"
    redirectPort="8443"
    server="Test"
/>
```

![응답 헤더 변경 결과](https://blog.kakaocdn.net/dn/cfqxQ2/btsEtuVfvMe/5ROUQGLkTRFK2juNGUuQp1/img.png)

개발자도구로 확인 시 Server 헤더가 위 설정 값으로 변경된 것을 확인할 수 있습니다.

## 2. 에러페이지 설정
톰캣 기본 에러페이지 호출 시에도 버전이 노출됩니다.

![기본 에러페이지 버전 노출](https://blog.kakaocdn.net/dn/bv5qtw/btsEuzIUukE/6Ob2iEHhMuYTKPl2SoEFn1/img.png)

### server.xml 추가 설정
```xml
<Host name="localhost" appBase="."
    unpackWARs="true" autoDeploy="true">
    
    <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
        prefix="localhost_access_log" suffix=".txt"
        pattern="%{X-Forwarded-For}i %l %u %t "%r" %s %b"/>
    
    <Valve className="org.apache.catalina.valves.ErrorReportValve" 
        showReport="false" 
        showServerInfo="false"/>
</Host>
```

![수정된 에러페이지](https://blog.kakaocdn.net/dn/tlCbz/btsEtmiLMMw/ZbRkRKI5Bx4DAEgEhnLEd1/img.png)

위 설정 추가 후 에러페이지 호출 시 버전 정보가 노출되지 않는 것을 확인할 수 있습니다.