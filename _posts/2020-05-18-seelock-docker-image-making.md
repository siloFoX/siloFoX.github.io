---
layout: post
title:  "Making SeeLock Docker image"
date:   2020-05-18
excerpt: "SeeLock 의 도커 이미지를 만들어보자"
tag:
- mean
- mean stack 
- mongodb
- mongo
- node
- node.js
- docker
- 도커
- ubuntu
- blog
- project
- seelock
- silofox
comments: true
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/seelock/seelock-feature.jpg?raw=true
---

(미완)

오늘은 SeeLock 의 도커 이미지를 만들어 보려고 한다.

SeeLock 은 크게<br>
Node.js 가 돌아가는 서버와<br>
MongoDB 로 나뉜다.

그 외에는 Shell script code 를 일정시간마다 실행해서<br>
Mongo DB 를 dump 파일로 백업하는 코드가 있다.<br>