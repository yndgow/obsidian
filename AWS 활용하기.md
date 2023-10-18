

## 6. [[AWS Mysql]]

### 7. war 배포하기

⚠️ JSP 프로젝트는 JAR 배포가 안된다고 한다. 
JSP는 파일 내부구조상 JSP파일들이 빌드되지 않으므로 WAR로 배포해야한다.

- bootWar 사용하기

기존 Export - war 파일은 jsp 가 담기지 않고 외부 톰캣으로 같이쓰려고 했으나 실패했다. 그러다 bootWar 명령어(gradle)를 사용하여




포트 확인하기

```
ss -ltn
```

ubuntu OS가 재시작 되면 Tomcat 자동 재시작하기

```
systemctl enable tomcat9.service
<->
systemctl disable tomcat9.service
```














### 8. JAR 배포하기

- gradle

해당 스프링 프로젝트로 가서 명령어를 실행시켜준다.

```shell
# 빌드하기
.\gradlew.bat build
gradlew.bat build

# 빌드 삭제하기
.\gradlew.bat clean
gradlew.bat clean
```

빌드가 완성되면  build 폴더가 생성된다. 하위의 libs 폴더를 보면 jar 파일을 발견할 수 있다.

```
java -jar jar파일명
```

명령어를 입력하면 서버가 시작됨을 알 수 있다.
