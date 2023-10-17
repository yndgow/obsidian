# 1. mybatis 쿼리 로그로 확인하기
## 1. 라이브러리 추가

[log4jdbc](https://mvnrepository.com/artifact/org.bgee.log4jdbc-log4j2/log4jdbc-log4j2-jdbc4.1/1.16)


- gradle
```
	implementation 'org.bgee.log4jdbc-log4j2:log4jdbc-log4j2-jdbc4.1:1.16'
```


## 2. 파일 추가
- resource 하위에 추가

![[log4jdbc.log4j2.properties]]

```
log4jdbc.spylogdelegator.name=net.sf.log4jdbc.log.slf4j.Slf4jSpyLogDelegator
log4jdbc.dump.sql.maxlinelength=0 # 전체 SQL문 제한없이 출력

# Driver 설정 추가 샘플
log4jdbc.drivers=com.mysql.cj.jdbc.Driver
log4jdbc.auto.load.popular.drivers=false
```

## 3. application.yml 수정

- 기존

![](https://i.imgur.com/b294wyM.png)


- 수정( name 변경, url log4jdbc 추가)

```yml
driver-class-name: net.sf.log4jdbc.sql.jdbcapi.DriverSpy
url: jdbc:log4jdbc:mysql://localhost:3306/CUGGI?serverTimeZone=Asia/Seoul
```

- 추가

```yaml
logging:
  level:
    jdbc:
	  sqlonly: info
	  sqltiming: off
	  resultsettable: info
	  audit: off
	  resultset: off
	  connection: off
```


# 2. 간단한 다른 방법

```yml
mybatis:
  mapper-locations:
  - classpath:mapper/**.xml
  configuration:
    map-underscore-to-camel-case: true
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl

logging:
  level:
    '[org.mybatis]': DEBUG
```

추가만 해주면 쿼리와 파라미터 결과테이블을 볼 수 있다. 위 방법보다 간단하다.