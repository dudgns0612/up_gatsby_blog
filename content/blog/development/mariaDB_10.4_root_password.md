---
title: 'mariaDB 10.4버전 이후 root 패스워드 변경'
date: 2020-01-14
category: 'database'
draft: false
---

![](./images/banner/mariaDB.png)

## mariaDB 10.4버전 이후 root 패스워드 변경

- mariaDB 최신 버전
mariaDB는 10.4 이후로 update 된 내용을 아래와 같이 기재
대략 시스템의 root와 mariaDB에 root와 동일하게 본다는 뜻
그렇기에 db 접속 시 시스템에 root로 등록하면 mysql 명령어로 접속 시
root 패스워드가 따로 필요 없어졌습니다.

공식 - https://mariadb.com/kb/en/library/authentication-from-mariadb-104/

- 기존에 root 패스워드 변경 시 아래와 같이 명령어 사용했지만 아래와 같은 에러가 표출.

```sh
MariaDB [mysql]> update user set password=password('rootpassword');
ERROR 1348 (HY000): Column 'Password' is not updatable
```


- mariaDB 10.4 이상부터는 이렇게 명령어를 사용해서 root에 패스워드를 변경할 수 있습니다.

```sh
MariaDB [mysql]> set password=password('rootpassword');
Query OK, 0 rows affected (0.012 sec)
```