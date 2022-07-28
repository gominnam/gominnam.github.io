---
title: 토이프로젝트 - WebSocket
date: 2022-07-26 20:11:00 +09:00
categories: [Project, chatting]
tags: [project, websocket]
---

채팅서버를 구현하기 앞서 서버환경을 어떻게 해야할지 자료를 조사하던중 기존에 구현해봤던 API대신에 WebSocket이라는 것을 사용해야한다.<br><br>

WebSocket은 사용자의 브라우저와 서버간에 단일 TCP 연결을 통해 전이중 통신 채널을 제공하는 컴퓨터 통신 프로토콜이다. 이 API를 사용하면 응답을 위해 서버를 폴링(Polling)하지 않고 서버에 메세지를 보내고 이벤트 기반 응답을 받을 수 있다.
접속까지는 HTTP 프로토콜을 이용하고 그 이후의 통신은 자체적은 WebSocket 프로토콜으로 통신하게 된다.<br>




## 용어정리

- 폴링(Polling) : 폴링(polling)이란 하나의 장치(또는 프로그램)이 충돌 회피 또는 동기화 처리 등을 목적으로 다른 장치(또는 프로그램)의 `상태를 주기적으로 검사하여 일정한 조건을 만족할 때 송수신 등의 자료처리를 하는 방식`을 말한다.<br>




## 참고자료

- <https://www.educba.com/websocket-vs-rest/>
- <https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API>
- <https://daddyprogrammer.org/post/4077/spring-websocket-chatting/>