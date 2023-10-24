- PID 확인하기

```ubuntu
ps -ef
ps -ef | 프로세스명
```

- kill pid

```ubuntu
kill -9 PID
```

- 포트 확인하기

```ubuntu
ss -ltn
```

- ubuntu OS가 재시작 되면 Tomcat 자동 재시작하기

```ubuntu
systemctl enable tomcat9.service
<->
systemctl disable tomcat9.service
```
