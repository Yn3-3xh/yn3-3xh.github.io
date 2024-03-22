---
title: "[Study - Front-end basic] HTML & Internet"
date : 2023-02-09 23:00:00 +09:00
categories : [Study - Front-end basic]
tags : [study, TML, Server&Client, Web Hosting]
---

# HTML

---

- 퍼블릭 도메인 (Public Domain)
    
    저작권이 없는 도메인
    

- 문장 중 줄바꿈
    
    `<p>` 태그를 통해서 단락의 경계를 분명히 하면서 CSS를 통해서 `<p>` 태그의 디자인을 자유롭게 변경할 수 있기 때문에 `<br>` 태그 보다 `<p>` 태그가 더 좋은 선택이다.
    
    > `<p>` 태그 : 문맥을 나눈다.<br>
    > `<br>` 태그 : 단순히 줄바꿈을 한다.

- `<title>` 태그
    
    `<title>` 태그는 검색엔진이 웹페이지를 분석할 때 가장 중요하게 생각하는 태그이기 때문에 `<title>` 태그를 쓰지 않으면 정말 큰 손해이다.
    
    영어가 아닌 문자로 웹페이지를 만들면 웹 브라우저의 인코딩 형식이 일치하지 않을 때 문자가 깨지는 경우가 있으니 주의해야 한다.
    

# Internet

---

## Server & Client

인터넷이 동작하기 위해서는 서로 정보를 주고 받을 수 있는 최소 2대의 컴퓨터가 있어야 한다.

![Server&Client1](https://drive.google.com/thumbnail?id=15yKLUesMMeGMYevBqiihLrsTShK7Tyn9&sz=w500)

그리고 각각의 프로그램을 개발하여 하나는 웹 브라우저, 다른 하나는 웹 서버라는 이름을 붙인다.

웹 서버가 설치된 컴퓨터에는 info.cern.ch 라는 주소를 부여하고, 이 컴퓨터의 어떤 디렉토리에 index.html 이라는 파일을 저장한다.

이후에 웹 브라우저가 설치된 컴퓨터의 주소창에 http://info.cern.ch/index.html 을 친다.

![Server&Client2](https://drive.google.com/thumbnail?id=1nIZOjSArKfBSysDqrioCoDkNLlQQ1pmk&sz=w500)

웹 브라우저가 설치된 컴퓨터는 인터넷을 통해서 전기적인 신호를 info.cern.ch 라는 주소의 컴퓨터에게 보낸다.

그 전기적인 신호는 index.html 이라는 파일의 코드를 원한다.

info.cern.ch에 설치된 웹 서버라는 프로그램이 어떤 디렉토리에서 index.html이라는 파일을 찾고, 그 내용을 읽어서 전기적인 신호를 바꾼 후에 웹 브라우저가 설치된 컴퓨터에 신호를 보낸다.

웹 브라우저가 설치된 컴퓨터에는 index.html 파일의 내용인 코드가 도착한다.

웹 브라우저는 그 코드를 읽어서 웹페이지를 화면에 출력한다.

이 관계를 보면 웹 브라우저가 설치된 컴퓨터와 웹 서버가 설치된 컴퓨터가 서로 정보를 주고 받는다.

이 중 **웹 브라우저가 깔린 컴퓨터는 정보를 요청**하고, **웹 서버가 깔린 컴퓨터는 정보를 응답**한다.

여기서 **요청하는 컴퓨터를 클라이언트 컴퓨터**, **응답하는 컴퓨터를 서버 컴퓨터**라고 부른다.

## Web Hosting

![Web Hosting1](https://drive.google.com/thumbnail?id=16PFyaxy67eW7czKAqiRV2J1Iv0YY7DsN&sz=w500)

‘my’ 컴퓨터는 현재로서 컨텐츠를 서비스 할 수 없다.

‘github’의 pages 기능을 이용하여 파일들을 업로드하고, pages 기능을 활성화하면 ‘github’의 서버 컴퓨터에 웹 서버가 켜지고, 웹 서버의 주소를 알려준다.

이제 웹 서버의 주소를 ‘visitor’에게 알려주면 ‘my’ 컴퓨터가 아닌 ‘github’의 컴퓨터에 설치된 웹 서버에 접속하게 된다.

![Web Hosting2](https://drive.google.com/thumbnail?id=1OeQRzW5HSnnrDiFoV8pKeYTCwUj7w0_C&sz=w500)

웹 서버를 직접 운영하는 것에 비해서 웹 호스팅을 이용하면 쉽다.

단점은 인터넷의 원리가 감춰져 있다는 것이다.

# Reference

---

[WEB1 - HTML & Internet - 생활코딩](https://opentutorials.org/course/3084)
