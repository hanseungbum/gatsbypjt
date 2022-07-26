---
title: fix db connection error 
subtitle: db 접속 오류 해결
date: 2022-07-26
slug: mariadb-connectionerror
author: han
rating: 5
coverImage: 
---

# 로컬 디비 연결 오류 해결
상황) update user set password = '1234' where user = 'root';
	명령어로 비밀번호를 바꿧더니 권한 오류가 생김.

윈도운에서 마리아 디비 권한오류/비밀번호 오류로 인해서 접속 못할 때, 접속 방법
설치 경로에서
순서) cd mariadb/bin
	경로에서
		mysqld -uroot --skip-grant-tables
		윈도우에서 mariadb 실행 명령어가 이렇게 되어있었음 (mysqld --defaults-file=C:\Windows\System32\mariaDB\my.ini --skip-grant-tables)
	커맨드창 종료하지말고 새로운 커맨드창 실행하여
	다시 경로에서
		mysql -uroot mysql
	접속 가능


localhost 권한이 없거나 접속 deny될때 방법	 
	UPDATE mysql.user SET authentication_string = PASSWORD('1234') WHERE User = 'root' AND Host = 'localhost';
	UPDATE mysql.user SET password = PASSWORD('1234') WHERE User = 'root' AND Host = 'localhost';
방법1)
	두개를 같이 바뀌어야함.	비밀번호를 PASSWORD({pwd})식으로 생성하지 않으면 권한 에러가 뜸
방법2)
	기존 root@localhost 이외의 아이디는 다 삭제함

실행된 mysql서버 종료 방법
	접속중인 커맨드에서 
		mysql) exit;
	경로에서
		mysqladmin -u root -p shutdown
	끈다
	
다시 확인
	cd mariadb/bin
	경로에서
		mysqld -u {userid} -p {pwd} 접속해서 진행;
		


