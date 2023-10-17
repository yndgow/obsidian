# AWS로 mysql DB 사용하기
### 1. 가입하기
가입시 이메일과 해외결제 가능한 신용, 체크카드, 휴대폰이 필요하다.


### 2. EC2

![](https://i.imgur.com/U3j7x57.png)


### 3. 인스턴스 시작

![](https://i.imgur.com/rVQM226.png)


### 4. AWS 생성하기기

![](https://i.imgur.com/m4fFLVo.png)


![](https://i.imgur.com/58EF64D.png)


![](https://i.imgur.com/XxVvpMB.png)


![](https://i.imgur.com/5dwWAmS.png)


![](https://i.imgur.com/zznjs85.png)


![](https://i.imgur.com/x6jIYXt.png)


https://www.putty.org/



![](https://i.imgur.com/49vjAR3.png)


![](https://i.imgur.com/5ETbC7K.png)


ppk browse

![](https://i.imgur.com/Ji8D70R.png)


![](https://i.imgur.com/y6KrxOE.png)


![](https://i.imgur.com/bJV6qPU.png)


### 5. root 로 접속하기
1. AWS EC2 생성하기
2. 해당 키페어를 이용하여 EC2 접속하기 (ec2-user)
    또는 브라우저에서 인스턴스 마우스 오른쪽 메뉴에서 연결
3. 접속후 다음과 같은 순서로 변경합니다.

```bash
sudo passwd root
sudo vi  /etc/ssh/sshd_config
	라인 번호 확인 명령어 (:set number)
	( 38 : PermitRootLogin no를 yes로 변경) 
	외부에서 root 접속 ok 혹은 그대로 두기(키 필요)
	( 65 : PasswordAuthentication no를 yes로 변경)
sudo cp /home/ubuntu/.ssh/authorized_keys /root/.ssh
sudo service ssh restart
```

4. root로 접속하기

![](https://i.imgur.com/RA5krTB.png)

![](https://i.imgur.com/TuWCHLP.png)


## 6. [[AWS Mysql]]

### 7. war 배포하기

⚠️ JSP 프로젝트는 JAR 배포가 안된다고 한다. 
JAR 는 톰캣이 내장되어 있기 때문에 따로 톰캣을 설치해줄 필요가 없지만 JSP는 파일 내부구조상 JSP파일들이 빌드되지 않으므로 WAR로 배포해야한다.



- EXPORT - WAR 프로

![](https://i.imgur.com/2XVsqgz.png)



![](https://i.imgur.com/Sx2D62n.png)


![](https://i.imgur.com/dUw4CMo.png)

Module name is invalid. 오류

**해결법**

![](https://i.imgur.com/OEQE5sU.png)


![0r7AzY5.png](https://i.imgur.com/0r7AzY5.png)



![](https://i.imgur.com/lgoZmZ3.png)


톰캣을 설치하고
```ubuntu
apt get install tomcat9 tomcat9-admin
```

파일질라로 tomcat9  - webapps 폴더에 복사 붙여넣기 한다.

![](https://i.imgur.com/y30k7D2.png)

8080 포트 확인하기

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
