### 6. mysql

```
apt-get update 
apt-get install mysql-server 
mysql -u root -p 
password -> 그냥 엔터
```


![](https://i.imgur.com/xZEDb7G.png)

-  root 비밀번호 변경

```
alter user 'root'@'%' identified with mysql_native_password by '변경할 비밀번호';
```

- 외부 접속 허용하기

```bash
vi /etc/mysql/mysql.conf.d/mysqld.cnf
bind-address 주석처리 #
```

- 서버에서 외부접속이 안된다면 root 의 접속권한을 체크해서 전체접근허용을 해준다.

```
create user 'root'@'%' identified by '비밀번호';
GRANT ALL PRIVILEGES ON *.* TO root@'%';
FLUSH PRIVILEGES;
```

- Public key retrieval is not allowed 접속주소 뒤에 붙여주거나 옵션에서 설정해준다.

```
useSSL=false&allowPublicKeyRetrieval=true
```
