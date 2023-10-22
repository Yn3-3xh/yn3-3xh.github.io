---
title: "[Study - Internet]<br>Domain Name 이란?"
date : 2022-11-15 19:00:00 +09:00
categories : [Study - Internet]
tags : [study, Internet, Domain Name]
---

## Domain Name의 발생 배경

---

인터넷 상에서 다른 단말에 접근하기 위해서는 숫자와 구분자(.)로 이뤄진 고유의 IP를 알아야 한다.<br>
(ex. 192.168.10.100)

특정 서버에 IP를 매칭해서 하나씩 외우는 것은 불편하다.<br>
(ex. 네이버: 125.209.222.142, 구글: 216.58.197.206)

숫자와 구분자(.)로 구성된 IP를 대신해 **사용자가 기억하기 쉬운 영문, 숫자 및 구분자(.)로 이뤄진 Domain Name이 등장**했다.

## Domain Name 이란?

---

- 넓은 의미: 네트워크상에서 컴퓨터를 식별하는 호스트명
- 좁은 의미: 도메인 레지스트리에게서 등록된 이름
    
    > 도메인 레지스트리: 최상위 도메인에 등록된 모든 Domain Name의 DB

인터넷 인프라의 핵심 부분이며 인터넷에 연결된 모든 컴퓨터는 IPv4 또는 IPv6와 같은 공용 IP 주소를 통해 연결되어 있다. 

이 IP 주소는 사람이 기억하기 어렵고 시간이 지남에 따라 변경될 수도 있기 때문에 Domain Name은 인터넷에서 사용할 수 있는 모든 웹 서버에 대해 사람이 읽을 수 있는 주소를 제공한다.

> **IPv4**<br>
> 32비트 길이의 식별자로 0.0.0.0 ~ 255.255.255.255 까지의 숫자 조합으로 이루어진다.<br>
> <br>
> **IPv6**<br>
> 전세계 공용으로 사용되며 인터넷 사용자 수가 급증하면서 IPv4 주소 고갈 문제를 해결하기 위해 등장한 주소이다.<br>
> 기존의 IPv4 주소 체계를 128비트 크기로 확장한 차세대 인터넷 프로토콜 주소이다.<br>
> 일반적으로 16비트 단위로 나누어지며, 각 16비트 블록은 다시 4자리 16진수로 변환되고 콜론으로 구분된다.

## Domain Name 구조

---

![Domain Name 구조](https://drive.google.com/uc?id=1IFFDc8yXeqdtl6FTbTO0FqzumPzTZI6Y){: width="70%" height="70%"}

점으로 구분되고 오른쪽에서 왼쪽으로 읽는 여러 부분으로 구성된 간단한 구조로, 각 부분은 전체 Domain Name에 대한 특정 정보를 제공한다.

- TLD (최상위 도메인)
    
    TLD는 사용자에게 도메인 이름 뒤에 있는 서비스의 일반적인 목적을 알려준다.
    
    가장 일반적인 TLD(.com, .org, .net)는 웹 서비스가 특정 기준을 충족할 것을 요구하지 않지만 일부 TLD는 더 엄격한 정책을 적용하여 목적이 무엇인지 더 명확하게 한다.
    
- label (레이블 또는 구성요소)
    
    TLD 다음에 오는 것
    
    A부터 Z까지의 대소문자, 0에서 9까지의 숫자 및 - 문자로 구성된다.
    
    TLD 바로 앞에 있는 레이블을 SLD(Secondary Level Domain)라고도 한다.
    

## References

---

[Domain Name과 DNS이란?](https://minemanemo.tistory.com/80)

[Domain Name이란 무엇인가?](https://velog.io/@chlcogh11/Domain-Name%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)
