---
title: "[Lecture - 김영한님(스프링 핵심 원리 - 기본편)] @Configuration과 바이트코드 조작의 마법"
date : 2024-04-15 19:28:00 +09:00
categories : [Lecture - 김영한님(스프링 핵심 원리 - 기본편)]
tags : [lecture, inflearn, 김영한님, 스프링 핵심 원리 - 기본편, spring, spring boot, singleton, Configuration]
---

inflearn에서 김영한님 강의를 들으면서 내용을 정리해보자.   
[스프링 핵심 원리 - 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8){:target="_blank"}

---

# @Configuration과 바이트코드 조작의 마법
스프링 컨테이너는 싱글톤 레지스트리이다. 따라서 스프링 빈이 싱글톤이 되도록 보장해주어야 한다.   
그런데 스프링이 자바 코드까지 어떻게 하기는 어렵기 때문에
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
AppConfig 코드를 보면 3번 호출되어야 하는 것이 맞다.

```java
@Test
void configurationDeep() {
    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
    AppConfig bean = ac.getBean(AppConfig.class);

    System.out.println("bean = " + bean.getClass());
}
```
`AnnotationConfigApplicationContext`에 파라미터로 넘긴 값은 스프링 빈으로 등록된다. 그래서 `AppConfig`도 스프링 빈이 된다.   
`AppConfig` 스프링 빈을 조회해서 클래스 정보를 출력해보면
```text
bean = class hello.core.AppConfig$$SpringCGLIB$$0
```
순수한 클래스라면 아래와 같이 출력되어야 한다.
```text
class hello.core.AppConfig
```
하지만 예상과는 달리 클래스명에 xxxCGLIB가 붙으면서 상당히 복잡해진 것을 볼 수 있다.   
이것은 내가 만든 클래스가 아니라 **스프링이 CGLIB라는 바이트코드 조작 라이브러리를 사용**해서 AppConfig 클래스를 상속받은 임의의 다른 클래스를 만들고, 그 다른 클래스를 스프링 빈으로 등록한 것이다.

![CGLIB 바이트코드 조작](https://drive.google.com/thumbnail?id=1PtAnI9jOU38sMIvYsRzt77SKbW6Sn-Bg&sz=w500)

그 임의의 다른 클래스가 바로 싱글톤이 보장되도록 해준다. (CGLIB의 내부 기술은 복잡하다..)

`@Bean`이 붙은 메서드마다 이미 스프링 빈이 존재하면 존재하는 빈을 반환하고, 스프링 빈이 없으면 생성해서 스프링 빈으로 등록하고 반환하는 코드가 동적으로 만들어진다.   
따라서 싱글톤이 보장되는 것이다.
> AppConfig@CGLIB는 AppConfig의 자식 타입이므로, AppConfig 타입으로 조회할 수 있다.

---

# @Configuration을 적용하지 않고, @Bean만 적용하면 어떻게 될까
`@Configuration`을 붙이면 바이트코드를 조작하는 CGLIB 기술을 사용해서 싱글톤을 보장하지만, 만약 `@Bean`만 적용하면 어떻게 될까?

```text
class hello.core.AppConfig
```
위 출력 결과처럼 AppConfig가 CGLIB 기술 없이 순수한 AppConfig로 스프링 빈에 등록된 것을 확인할 수 있다.

<br>

```text
call AppConfig.memberService
call AppConfig.memberRepository
call AppConfig.orderService
call AppConfig.memberRepository
call AppConfig.memberRepository
```
하지만 위 출력 결과처럼 MemebrRepository가 총 3번 호출된 것을 알 수 있다.   
한번은 @bean에 의해 스프링 컨테이너에 등록하기 위해서이고, 두번은 각각 `memberRepository()`를 호출하면서 발생한 것이다. 
> `@Configuration`을 지우고 인스턴스가 같은지 테스트해보면 주소값이 다르므로 실패하게 된다.    
> 즉, 싱글톤을 보장하지 않는다.

---

# 정리
* `@Bean`만 사용해도 스프링 빈으로 등록되지만, 싱글톤을 보장하지 않는다.
  * `memberRepository()`처럼 의존관계 주입이 필요해서 메서드를 직접 호출할 때 싱글톤을 보장하지 않는다.
* 크게 고민할 것 없이 **스프링 설정 정보는 항상 `@Configuration`을 사용하자.**

