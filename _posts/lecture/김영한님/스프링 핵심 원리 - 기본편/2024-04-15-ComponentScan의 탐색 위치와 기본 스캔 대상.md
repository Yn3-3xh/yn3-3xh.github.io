---
title: "[Lecture - 김영한님(스프링 핵심 원리 - 기본편)] @ComponentScan의 탬색 위치와 기본 스캔 대상"
date : 2024-04-15 23:51:00 +09:00
categories : [Lecture - 김영한님(스프링 핵심 원리 - 기본편)]
tags : [lecture, inflearn, 김영한님, 스프링 핵심 원리 - 기본편, spring, spring boot, Component, ComponentScan]
---

inflearn에서 김영한님 강의를 들으면서 내용을 정리해보자.   
[스프링 핵심 원리 - 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8){:target="_blank"}

---

# 탐색할 패키지의 시작 위치 지정
모든 자바 클래스를 다 컴포넌트 스캔하면 시간이 오래 걸리기 때문에 꼭 필요한 위치부터 탐색하도록 시작 위치를 지정할 수 있다.
```java
@Configuration
@ComponentScan(
        basePackages = "hello.core.member",
        excludeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = Configuration.class)
)
public class AutoAppConfig {
}
```
* `basePackages`로 탐색할 패키지의 시작 위치를 지정한다.
  * 이 패키지를 포함해서 하위 패키지를 모두 탐색한다.
  * `basePackages = {"hello.core", "hello.service}`처럼 하여 여러 시작 위치를 지정할 수도 있다.
* `basePackagesClasses`로 지정한 클래스의 패키지를 탐색 시작 위치로 지정한다.
* 만약 지정하지 않으면 `@ComponentScan`이 붙은 설정 정보 클래스의 패키지가 시작 위치가 된다.
> **권장하는 방법**   
> 패키지 위치를 지정하지 않고, 설정 정보 클래스의 위치를 프로젝트 최상단에 두는 것 (최근 스프링 부트도 이 방법을 기본으로 제공)

<br>

예를 들어, 프로젝트가 아래와 같은 구조로 되어 있으면
* com.hello
* com.hello.service
* com.hello.repository

프로젝트 시작 루트인 "com.hello"에 AppConfig 같은 메인 설정 정보를 두고, `@ComponentScan` 애너테이션을 붙이고, `basePackages` 지정은 생략한다.   
이렇게 하면 "com.hello"를 포함한 하위는 모두 자동으로 컴포넌트 스캔의 대상이 된다.   
그리고 프로젝트 메인 설정 정보는 프로젝트를 대표하는 정보이기 때문에 프로젝트 시작 루트 위치에 두는 것이 좋다.   
> 스프링 부트를 사용하면 스프링 부트의 대표 시작 정보인 `@SpringBootApplication`을 이 프로젝트 시작 루트 위치에 두는 것이 관례이다. (이 설정안에 `@ComponentScan`이 들어 있다.)

> `useDefaultFilters` 옵션은 기본으로 켜져있는데, 이 옵션을 끄면 기본 스캔 대상들이 제외된다.

---

# 컴포넌트 스캔 기본 대상
컴포넌트 스캔은 `@Component` 뿐만 아니라 아래와 같은 내용도 추가로 대상에 포함한다.
* `@Component` : 컴포넌트 스캔에서 사용
* `@Controller` : 스프링 MVC 컨트롤러에서 사용
* `@Repository` : 스프링 데이터 접근 계층에서 사용
* `@Configuration` : 스프링 설정 정보에서 사용
* `@Service` : 스프링 비즈니스 로직에서 사용

> 애너테이션에는 상속 관계라는 것이 없다.   
> 그래서 애너테이션이 특정 애너테이션을 들고 있는 것을 인식할 수 있는 것은 자바 언어가 지원하는 기능이 아니고, 스프링이 지원하는 기능이다.

<br>

컴포넌트 스캔의 용도 뿐만 아니라 아래 애너테이션들은 스프링 부가 기능을 수행한다.
* `@Controller` : 스프링 MVC 컨트롤러로 인식
* `@Repository` : 스프링 데이터 접근 계층으로 인식하고, 데이터 계층의 예외를 스프링 예외로 변환해준다.
* `@Configuration` : 스프링 설정 정보로 인식하고, 스프링 빈이 싱글톤을 유지하도록 추가 처리를 한다.
* `@Service` : 이 애너테이션은 특별한 처리를 하지 않는다. 대신 개발자들이 핵심 비즈니스 로직이 여기에 있겠구나 라고 비즈니스 계층을 인식하는데 도움이 된다.
