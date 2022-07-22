---
title: study backend
subtitle: 백엔드 학습
date: 2022-07-18
slug: springboot
author: han
rating: 5
coverImage: 
---

# 로컬 디비 연결 오류 해결
postman 에서 json타입 post 전송시 
org.springframework.web.HttpMediaTypeNotSupportedException: Content type 'application/json;charset=UTF-8' not supported
에러 발생

이유 : 데이터 요청 방식과 서버에서 받는 데이터 타입이 맞지 않아 발생

구글링 해결방법 : post 요청 header에 'Content type : application/json' 삽입 후 요청
결과 : 실패(요청 header에 이미 'Content type : application/json'가 삽입 되어있었음)

실제 해결 방법 : 

	@PostMapping("/users")
    public string createUser(@RequestBody RequestUser user)
	
	->
	
	@PostMapping("/users")
    public ResponseEntity createUser(@RequestBody RequestUser user)
	
	변경 & intellij 재실행

