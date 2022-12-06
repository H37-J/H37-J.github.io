---
layout:     post
title:      "리눅스의 명령어들"
subtitle:   "리눅스 명령어 정리"
date:       2021-08-30 12:00:00
author:     "H37-J"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - os
---

### 기본 명령어

mkdir: 디렉터리 추가  

find: 디렉터리를 찾거나 파일을 찾음 ex)  find /home -name *.txt  

mv: 디렉터리 이름을 바꾸거나 이동에 사용  

cp: 디렉터리, 파일 복사  

### CPU, Memory 확인

top, htop

### 네트워크

lsof -i :3000 | grep LISTEN (3000포트 사용중인 프로세스 확인)

iptables -I INPUT 1 -p tcp --dport 8080 -j ACCEPT

ps -ef: 실행중인 프로세스 확인.

kill -9 pid: 프로세스 종료  

ifconfig: 네트워크 카드 정보 보기

ping: 연결 확인

netstat -an: 포트 사용량

### 압축관련

tar: 압축   

-z: gzip 압축 명령을 호출하여 압축  
-c 패키지파일  
-v 실행중인 프로세스 표시  
-f 파일이름 지정.  

tar -zcvf test.tar.gz aaa.txt bbb.txt  

tar -xvf: 압축을 품
  
