
## [[1. AWS 서버 사용하기]]
## [[2. AWS Mysql]]

## [[3. AWS에 스프링 프로젝트 배포하기]]



















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
