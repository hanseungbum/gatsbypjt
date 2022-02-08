---
title: 클레이튼 수업 3
subtitle: 환경설정
date: 2022-02-07
slug: 리액트 앱 개발 환경 설정
author: han
rating: 5
coverImage: 
---

# 개발환경 설정
리엑트 start 명령어
: npx create-react-app klay-market

node package 설치
+axios
+caver-js
+qrcode

klaytn api 아이디 만들기 
kas console
url : https://console.klaytnapi.com/ko/auth/login

access key : KASK96V7OJU45OCSJNMQVUS0
secret accesskey : grQWVk08S_aslUE5s2QYxBbdk458x9LJjRVfGX5t
authorization : 
Basic S0FTSzk2VjdPSlU0NU9DU0pOTVFWVVMwOmdyUVdWazA4U19hc2xVRTVzMlFZeEJiZGs0NTh4OUxKalJWZkdYNXQ=



# node 버전은 14

 npx clear-npx-cache
 npm uninstall -g create-react-app
 yarn global remove create-react-app
 npx clear-npx-cache
 npx create-react-app --scripts-version 4.0.3 klay-market


## package.json

{
  "name": "klay-market",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "@fortawesome/fontawesome-svg-core": "^1.2.35",
    "@fortawesome/free-solid-svg-icons": "^5.15.3",
    "@fortawesome/react-fontawesome": "^0.1.14",
    "@testing-library/jest-dom": "^5.11.4",
    "@testing-library/react": "^11.1.0",
    "@testing-library/user-event": "^12.1.10",
    "axios": "^0.21.1",
    "bootstrap": "^4.6.0",
    "caver-js": "^1.6.1",
    "node-sass": "^5.0.0",
    "qrcode.react": "^1.0.1",
    "react": "^17.0.2",
    "react-bootstrap": "^1.5.2",
    "react-dom": "^17.0.2",
    "react-scripts": "4.0.3",
    "web-vitals": "^1.0.1"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "eslintConfig": {
    "extends": [
      "react-app",
      "react-app/jest"
    ]
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  },
  "devDependencies": {
    "eslint": "^7.24.0",
    "eslint-plugin-react": "^7.23.2"
  }
}