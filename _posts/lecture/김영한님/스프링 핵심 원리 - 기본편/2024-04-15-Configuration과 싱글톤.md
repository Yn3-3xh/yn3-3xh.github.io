---
title: "[Lecture - 김영한님(스프링 핵심 원리 - 기본편)] @Configuration과 싱글톤"
date : 2024-04-15 18:44:00 +09:00
categories : [Lecture - 김영한님(스프링 핵심 원리 - 기본편)]
tags : [lecture, inflearn, 김영한님, 스프링 핵심 원리 - 기본편, spring, spring boot, singleton, Configuration]
---

inflearn에서 김영한님 강의를 들으면서 내용을 정리해보자.   
[스프링 핵심 원리 - 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8){:target="_blank"}

---

AppConfig 코드를 보면 싱글톤에 대해 이상한 점이 있다.
```java
@Configuration
public class AppConfig {
    @Bean
    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }

    @Bean
    public OrderService orderService() {
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }
    ...
}
```
* memberService 빈을 만드는 코드를 보면 `memberRepository()`를 호출한다.
  * 이 메서드를 호출하면 `new MemoryMemberRepository()`를 호출한다.
* orderService 빈을 만드는 코드도 동일하게 `memberRepository()`를 호출한다.
  * 이 메서드를 호출하면 `new MemoryMemberRepository()`를 호출한다.

결과적으로 **각각 다른 2개의 MemoryMemberRepository가 생성되면서 싱글톤이 깨지는 것 처럼 보인다.**   
스프링 컨테이너는 이 문제를 어떻게 해결할까?

<br>

```java
public class ConfigurationSingletonTest {
    @Test
    void configurationTest() {
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

        MemberServiceImpl memberService = ac.getBean("memberService", MemberServiceImpl.class);
        OrderServiceImpl orderService = ac.getBean("orderService", OrderServiceImpl.class);
        MemberRepository memberRepository = ac.getBean("memberRepository", MemberRepository.class);

        MemberRepository memberRepository1 = memberService.getMemberRepository();
        MemberRepository memberRepository2 = orderService.getMemberRepository();

        System.out.println("memberService -> memberRepository = " + memberRepository1);
        System.out.println("orderService -> memberRepository = " + memberRepository2);
        System.out.println("memberRepository = " + memberRepository);

        assertThat(memberService.getMemberRepository()).isSameAs(memberRepository);
        assertThat(orderService.getMemberRepository()).isSameAs(memberRepository);
    }
}
```
* 확인해보면 memberRepository 인스턴스는 모두 같은 인스턴스가 공유되어 사용된다.
* AppConfig의 자바 코드를 보면 분명히 각각 2번 `new MemoryMemberRepository`를 호출해서 다른 인스턴스가 생성되어야 하는데 어떻게 된 것일까?

<br>

AppConfig에서 호출되는 메서드에 출력을 찍어서 테스트를 해보자.
```java
@Configuration
public class AppConfig {
    @Bean
    public MemberService memberService() {
        System.out.println("call AppConfig.memberService");
        return new MemberServiceImpl(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository() {
        System.out.println("call AppConfig.memberRepository");
        return new MemoryMemberRepository();
    }

    @Bean
    public OrderService orderService() {
        System.out.println("call AppConfig.orderService");
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }
    ...
}
```
스프링 컨테이너가 각각 `@Bean`을 호출해서 스프링 빈을 생성한다.   
따라서 `memberRepository()`가 3번 호출되어야 하지 않을까?
1. `@Bean`이 붙어 있는 `memberRepository()` 호출
2. `memberService()` 로직에서 `memberRepository()` 호출
3. `orderService()` 로직에서 `memberRepository()` 호출

하지만 출력 결과는 모두 1번만 호출된다.
```text
call AppConfig.memberService
call AppConfig.memberRepository
call AppConfig.orderService
```
