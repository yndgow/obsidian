## 1. 라이브러리 추가

[log4jdbc](https://mvnrepository.com/artifact/org.bgee.log4jdbc-log4j2/log4jdbc-log4j2-jdbc4.1/1.16)


- gradle
```
	implementation 'org.bgee.log4jdbc-log4j2:log4jdbc-log4j2-jdbc4.1:1.16'
```


## 2. 파일 추가

![[log4jdbc.log4j2.properties]]

```
log4jdbc.spylogdelegator.name=net.sf.log4jdbc.log.slf4j.Slf4jSpyLogDelegator
log4jdbc.dump.sql.maxlinelength=0 # 전체 SQL문 제한없이 출력

# Driver 설정 추가 샘플
log4jdbc.drivers=com.mysql.cj.jdbc.Driver
log4jdbc.auto.load.popular.drivers=false
```

## 3. application.yml Datasource 변경

![](https://i.imgur.com/b294wyM.png)




```
driver-class-name: net.sf.log4jdbc.sql.jdbcapi.DriverSpy
url: jdbc:log4jdbc:mysql://localhost:3306/CUGGI?serverTimeZone=Asia/Seoul
```

