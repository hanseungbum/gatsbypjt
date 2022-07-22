---
title: connect backend to db
subtitle: 백엔드 디비랑 연결 오류 수정
date: 2022-07-20
slug: springboot-mariadb
author: han
rating: 5
coverImage: 
---

# 3d interactive Web
Access denied for user 'root'@'localhost' (using password: YES)
Current charset is UTF-8. If password has been set using other charset, consider using option 'passwordCharacterEncoding'
오류1

jdbc:mariadb://localhost:3306/server_renew?passwordCharacterEncoding=UTF-8 
프로퍼티 수정함
-> 오류1 해결 권한 오류 발생
-> mariadb로 들어가서 han id생성해서 localhost권한 줘서 해결

CREATE USER 'han'@'localhost' IDENTIFIED BY '0000'
GRANT ALL PRIVILEGES ON *.* TO 'han'@'localhost' WITH GRANT OPTION;
CREATE USER 'han'@'%' IDENTIFIED BY '0000';
GRANT ALL PRIVILEGES ON *.* TO 'han'@'%' WITH GRANT OPTION;

FLUSH PRIVILEGES;


