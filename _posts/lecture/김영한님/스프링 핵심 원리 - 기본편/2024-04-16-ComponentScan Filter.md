---
title: "[Lecture - 김영한님(스프링 핵심 원리 - 기본편)] @ComponentScan Filter"
date : 2024-04-16 00:54:00 +09:00
categories : [Lecture - 김영한님(스프링 핵심 원리 - 기본편)]
tags : [lecture, inflearn, 김영한님, 스프링 핵심 원리 - 기본편, spring, spring boot, ComponentScan, Filter]
---

inflearn에서 김영한님 강의를 들으면서 내용을 정리해보자.   
[스프링 핵심 원리 - 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8){:target="_blank"}

---

# Filter
* `includeFilter` : 컴포넌트 스캔 대상을 추가로 지정한다.
* `excludeFilter` : 컴포넌트 스캔에서 제외할 대상을 지정한다.

## Filter Type 옵션
* ANNOTATION : 기본값, 어노테이션을 인식해서 동작
* ASSIGNABLE_TYPE : 지정한 타입과 자식 타입을 인식해서 동작
* ASPECTJ : AspectJ 패턴 사용
* REGEX : 정규 표현식
* CUSTOM : `TypeFilter`라는 인터페이스를 구현해서 처리

> `@Component`면 충분하기 때문에 `includeFilter`를 사용할 일은 거의 없다.   
> `excludeFilters`는 여러가지 이류로 간혹 사용할 때가 있지만 많지는 않다.   
> 특히 최근 스프링 부트는 컴포넌트 스캔을 기본으로 제공하는데, 옵션을 변경하면서 사용하기 보다는 스프링의 기본 설정에 최대한 맞추어 사용하는 것을 권장한다.
