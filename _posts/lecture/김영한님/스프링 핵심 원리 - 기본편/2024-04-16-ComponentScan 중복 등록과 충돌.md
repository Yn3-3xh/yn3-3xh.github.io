---
title: "[Lecture - 김영한님(스프링 핵심 원리 - 기본편)] @ComponentScan 중복 등록과 충돌"
date : 2024-04-16 11:40:00 +09:00
categories : [Lecture - 김영한님(스프링 핵심 원리 - 기본편)]
tags : [lecture, inflearn, 김영한님, 스프링 핵심 원리 - 기본편, spring, spring boot, ComponentScan]
---

inflearn에서 김영한님 강의를 들으면서 내용을 정리해보자.   
[스프링 핵심 원리 - 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8){:target="_blank"}

---

# 자동 빈 등록 vs 자동 빈 등록
* 컴포넌트 스캔에 의해 자동으로 스프링 빈이 등록되는데, 그 이름이 같은 경우 스프링은 오류를 발생시킨다.
  * ConflictingBeanDefinitionException 예외 발생

---

# 수동 빈 등록 vs 자동 빈 등록
이 경우 수동 빈 등록이 우선권을 가진다. (수동 빈이 자동 빈을 오버라이딩 해버린다.)

* 수동 빈 등록시 남는 로그
  ```text
  Overriding bean definition for bean 'memoryMemberRepository' with a different
  definition: replacing
  ```

<br>

* 최근 스프링 부트에서는 수동 빈 등록과 자동 빈 등록이 충돌나면 오류가 발생하도록 기본 값을 바꾸었다.
  * 스프링 부트인 CoreApplication 실행 시 발생 오류
    ```text
    Consider renaming one of the beans or enabling overriding by setting spring.main.allow-bean-definition-overriding=true
    ```
