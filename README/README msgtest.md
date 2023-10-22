# 메신져 기능 테스트

## 참고사이트
https://www.daddyprogrammer.org/post/4077/spring-websocket-chatting/

## 환경
- SpringBoot 2.7.17

## 라이브러리
```yml
implementation 'org.springframework.boot:spring-boot-starter-web'

implementation 'org.springframework.boot:spring-boot-starter-websocket'

compileOnly 'org.projectlombok:lombok'

developmentOnly 'org.springframework.boot:spring-boot-devtools'

annotationProcessor 'org.projectlombok:lombok'

providedRuntime 'org.springframework.boot:spring-boot-starter-tomcat'

testImplementation 'org.springframework.boot:spring-boot-starter-test'

runtimeOnly 'com.h2database:h2'
```

## 기능
- 채팅방만들기
- 다중사용자 참여하여 채팅하기
