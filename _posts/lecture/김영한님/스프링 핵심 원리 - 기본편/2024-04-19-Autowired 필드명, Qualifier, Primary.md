---
title: "[Lecture - 김영한님(스프링 핵심 원리 - 기본편)] @Autowired 필드명, @Qualifier, @Primary"
date : 2024-04-19 16:29:00 +09:00
categories : [Lecture - 김영한님(스프링 핵심 원리 - 기본편)]
tags : [lecture, inflearn, 김영한님, 스프링 핵심 원리 - 기본편, spring, spring boot, Autowired, Qualifier, Primary]
---

inflearn에서 김영한님 강의를 들으면서 내용을 정리해보자.   
[스프링 핵심 원리 - 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8){:target="_blank"}

---

조회 대상 빈이 2개 이상일 때 해결 방법
* @Autowired 필드명 매칭
* @Qualifier -> @Qualifier끼리 매칭 -> 빈 이름 매칭
* @Primary 사용

# @Autowired 필드명 매칭
`@Autowired`는 타입 매칭을 시도하고, 이때 여러 빈이 있으면 필드명(파라미터명)으로 빈 이름을 추가 매칭한다.
```java
// 기존 코드
@Autowired
private DiscountPolicy discountPolicy;

// 필드명을 빈이름으로 변경한 코드
@Autowired
private Discount rateDiscountPolicy;
```
필드명이 `rateDiscountPolicy`이므로 정상 주입된다.
필드명 매칭은 먼저 타입 매칭을 시도하고, 그 결과에 여러 빈이 있을 때 추가로 동작하는 기능이다.

## 정리
1. 타입 매칭
2. 타입 매칭의 결과가 2개 이상일 때 필드명, 파라미터명으로 빈 이름 매칭

---

# @Qualifier 사용
`@Qualifier`는 추가 구분자를 붙여주는 방법이다.   
주입시 추가적인 방법을 제공하는 것이지 빈 이름을 변경하는 것은 아니다.
```java
@Component
@Qualifier("mainDiscountPolicy")
public class RateDiscountPolicy implements DiscountPolicy { }

@Component
@Qualifier("fixDiscountPolicy")
public class FixDiscountPolicy implements DiscountPolicy { }
```
주입시 `@Qualifier`를 붙여주고 등록한 이름을 적어준다.

```java
@Autowired
public OrderServiceImpl(MemberRepository memberRepository, @Qualifier("mainDiscountPolicy") DiscountPolicy discountPolicy) {
    this.memberRepository = memberRepository;
    this.discountPolicy = discountPolicy;
}
```
`@Qualifier`로 주입할 때 `@Qualifier("mainDiscountPolicy")`를 못찾으면 mainDiscountPolicy라는 이름의 스프링 빈을 추가로 찾는다.   
하지만 `@Qualifier`는 `@Qualifier`를 찾는 용도로만 사용하는게 명확하고 좋다.

## 정리
1. `@Qualifier`끼리 매칭
2. 빈 이름 매칭
3. `NoSuchBeanDefinitionException` 예외 발생

---

# @Primary 사용
`@Primary`는 우선순위를 정하는 방법이다.   
`@Autowired`시에 여러 빈이 매칭되면 `@Primary`가 우선권을 가진다.

```java
@Component
@Primary
public class RateDiscountPolicy implements DiscountPolicy { }

@Component
public class FixDiscountPolicy implements DiscountPolicy { }
```
rateDiscountPolicy가 우선권을 가지게 된다.
```java
@Autowired
public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
    this.memberRepository = memberRepository;
    this.discountPolicy = discountPolicy;
}
```

## 정리
`@Primary`와 `@Qualifier` 중에 어떤 것을 사용하면 좋을지 고민이 될 것이다.   
`@Qualifier`의 단점은 주입 받을 때 모든 코드에 `@Qualifier`를 붙여주어야 한다는 점이다.   
반면에 `@Primary`를 사용하면 `@Qualifier`를 붙일 필요가 없다.

---

# @Primary, @Qualifier 활용
코드에서 자주 사용하는 메인 DB의 커넥션을 획득하는 스프링 빈이 있고, 코드에서 특별한 기능으로 가끔 사용하는 서브 DB의 커넥션을 획득하는 스프링 빈이 있다면   
메인 DB의 커넥션을 획득하는 스프링 빈은 `@Primary`를 적용해서 조회하는 곳에서 `@Qualifier` 지정 없이 편리하게 조회하고,   
서브 DB의 커넥션 빈을 획득할 때는 `@Qualifier`를 지정해서 명시적으로 획득하는 방식을 사용하면 코드를 깔끔하게 유지할 수 있다.   
물론 이때 DB의 스프링 빈을 등록할 때 `@Qualifier`를 지정해주는 것은 상관없다.

**- 우선순위**   
`@Primary`는 기본값처럼 동작하는 것이고, `@Qualifier`는 매우 상세하게 동작한다.   
이런 경우 자세한 것이 우선권을 가져간다.   
즉, 자동보다는 수동이, 넓은 범위의 선택권보다는 좁은 범위의 선택권이 우선순위가 높다.   
따라서 `@Qualifier`가 우선권이 높다.
