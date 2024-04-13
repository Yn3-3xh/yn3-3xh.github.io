---
title: "[Lecture - 김영한님(스프링 핵심 원리 - 기본편)] 스프링 빈 설정 메타 정보 - BeanDefinition"
date : 2024-04-13 18:12:00 +09:00
categories : [Lecture - 김영한님(스프링 핵심 원리 - 기본편)]
tags : [lecture, inflearn, 김영한님, 스프링 핵심 원리 - 기본편, spring, spring boot, BeanDefinition]
---

inflearn에서 김영한님 강의를 들으면서 내용을 정리해보자.   
[스프링 핵심 원리 - 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8){:target="_blank"}

---

# 스프링 빈 메타 정보 - BeanDefinition
스프링은 **BeanDefinition이라는 추상화로 다양한 설정 형식을 지원**한다.   
즉, 역할과 구현을 개념적으로 나눈 것이다.

1. XML을 읽어서 BeanDefinition을 만들면 된다.
2. Java 코드를 읽어서 BeanDefinition을 만들면 된다.
3. 스프링 컨테이너는 Java 코드인지, XML 인지 몰라도 된다. 오직 BeanDefinition만 알면 된다.

<br>

BeanDefinition을 빈 설정 메타 정보라고 한다.
> `@Bean`, `<bean>`당 각각 하나씩 메타 정보가 생성된다.

스프링 컨테이너는 이 메타 정보를 기반으로 스프링 빈을 생성한다.
![스프링 컨테이너 - BeanDefinition](https://drive.google.com/thumbnail?id=11xb44tpUKcjDCBoRCy6-OGmLBUfwGwou&sz=w700)
> 위 그림은 설계 자체를 추상화에만 의존하도록 설계되어 있다. 즉, BeanDefinition 자체가 인터페이스이다.

## BeanDefinition 생성 과정
![BeanDefinition 생성 과정](https://drive.google.com/thumbnail?id=1-otshhhaKvctSE6bW6zZaFRU71l1k6nE&sz=w700)
* `AnnotationConfigApplicationContext`는 `AnnotatedBeanDefinitionReader`를 사용해서 AppConfig.class를 읽고 `BeanDefinition`을 생성한다.
* `GenericXmlApplicationContext`는 `XmlBeanDefinitionReader`를 사용해서 appConfig.xml 설정 정보를 읽고 `BeanDefinition`을 생성한다.
* 새로운 형식의 설정 정보가 추가되면 `XxxBeanDefinitionReader`를 만들어서 `BeanDefinition`을 생성하면 된다.

