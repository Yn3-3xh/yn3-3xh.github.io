---
title: "[Lecture - 김영한님(스프링 핵심 원리 - 기본편)] BeanFactory와 ApplicationContext"
date : 2024-04-09 12:10:00 +09:00
categories : [Lecture - 김영한님(스프링 핵심 원리 - 기본편)]
tags : [lecture, inflearn, 김영한님, 스프링 핵심 원리 - 기본편, spring, spring boot]
---

inflearn에서 김영한님 강의를 들으면서 내용을 정리해보자.   
[스프링 핵심 원리 - 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8){:target="_blank"}

---

![BeanFactory와 ApplicationContext 구조도](https://drive.google.com/thumbnail?id=1w45TfUMWIt34dTAfG82P6lkNBx04n6K_&sz=w500)

# BeanFactory
* 스프링 컨테이너의 최상위 인터페이스이다.
* 스프링 빈을 관리하고 조회하는 역할을 담당한다.
* `getBean()`을 제공한다.

# ApplicationContext
* BeanFactory 기능을 모두 상속받아서 제공한다.
* 빈을 관리하고 검색하는 기능을 BeanFactory가 제공해주만, 애플리케이션을 개발할 때는 빈은 관리하고 조회하는 기능 외에도 수 많은 부가기능이 필요하다.

<br>

![ApplicationContext가 제공하는 부가기능](https://drive.google.com/thumbnail?id=1xAgDrluD3OJcqv8a0aR2kr_sLPOPn21p&sz=w700)*ApplicationContext가 제공하는 부가기능*

* MessageSource
  * 한국에서 들어오면 한국어로, 영어권에서 들어오면 영어로 출력하는 국제화 기능
* EnvironmentCapable
  * 로컬, 개발, 운영 등을 구분해서 처리
* ApplicationEventPublisher
  * 이벤트를 발생하고, 구독하는 모델을 편리하게 지원
* ResourceLoader
  * 파일, 클래스 패스, 외부 등에서 리소스를 편리하게 조회

---

# 정리
* ApplicationContext는 BeanFactory의 기능을 상속받는다.
* ApplicationContext는 빈 관리 기능 + 편리한 부가 기능을 제공한다.
* BeanFactory를 직접 사용할 일은 거의 없다.
  * 부가 기능이 포함된 ApplicationContext를 사용한다.
* BeanFactory나 ApplicationContext를 스프링 컨테이너라고 한다.
