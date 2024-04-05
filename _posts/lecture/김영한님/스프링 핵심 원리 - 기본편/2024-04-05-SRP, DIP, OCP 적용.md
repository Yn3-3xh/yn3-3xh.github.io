---
title: "[Lecture - 김영한님] SRP, DIP, OCP 적용"
date : 2024-04-05 09:38:00 +09:00
categories : [Lecture - 김영한님]
tags : [lecture, inflearn, 김영한님, spring, spring boot]
---

inflearn에서 김영한님 강의를 들으면서 내용을 정리해보자.   
[스프링 핵심 원리 - 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8){:target="_blank"}

---

# SRP 단일 책임 원칙
**한 클래스는 하나의 책임만 가져야한다.**

- 클라이언트 객체는 직접 구현 객체를 생성하고, 연결하고, 실행하는 다양한 책임을 가져고 있음
- SRP 단일 책임 원칙을 따르면서 관심사를 분리함
- 구현 객체를 생성하고 연결하는 책임은 AppConfig가 담당
- **클라이언트 객체는 실행하는 책임만 담당**

```java
public class OrderApp {
    public static void main(String[] args) {
        AppConfig appConfig = new AppConfig();
        MemberService memberService = appConfig.memberService();
        OrderService orderService = appConfig.orderService();
    
        Long memberId = 1L;
        Member member = new Member(memberId, "memberA", Grade.VIP);
        memberService.join(member);
    
        Order order = orderService.createOrder(memberId, "itemA", 10000);
    
        System.out.println("order = " + order);
    }
}
```

---

# DIP 의존관계 역전 원칙
프로그래머는 **추상화에 의존해야지, 구체화에 의존하면 안된다.**
> 의존성 주입(DI)은 이 원칙을 따르는 방법 중 하나다.

* 새로운 할인 정책을 개발하고, 적용하려고 하니 클래이언트 코드도 같이 변경해야 했다.
  * 왜냐하면 기존 클라이언트 코드 OrderServiceImpl은 DIP를 지키며, DiscountPolicy 추상화 인터페이스에 의존하는 것 같았지만,
    FixDiscountPolicy 구체화 구현 클래스에도 함께 의존했다.
* 클라이언트 코드가 DiscountPolicy 추상화 인터페이스에만 의존하도록 코드를 변경했다.
  * 하지만 클라이언트 코드는 인터페이스만으로는 아무것도 실행할 수 없다.
* AppConfig가 FixDiscountPolicy 객체 인스턴스를 클라이언트 코드 대신 생성해서 클라이언트 코드에 의존관계를 주입했다.
  * 이렇게해서 **DIP 원칙을 따르면서 문제도 해결**했다.

```java
public class AppConfig {
    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }
  
    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }
  
    public OrderService orderService() {
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }
  
    public DiscountPolicy discountPolicy() {
        return new FixDiscountPolicy();
//        return new RateDiscountPolicy();
    }
}
```

---

# OCP 개방-폐쇄 원칙
**소프트웨어 요소는 확장에는 열려 있으나, 변경에는 닫혀 있어야 한다.**

* 다형성을 사용하고 클라이언트가 DIP를 지킴
* 애플리케이션을 사용 영역과 구성 영역으로 나눔
* AppConfig가 의존관계를 FixDiscountPolicy -> RateDiscountPolicy로 변경해서 클라이언트 코드에 주입하므로 클라이언트 코드는 변경하지 않아도 됨
* 소프트웨어 요소를 새롭게 확장해도 사용 영역의 변경은 닫혀 있다.
  > 변경이 닫혀 있다는 것은 변경할 필요가 없다는 것이다.

```java
public class OrderServiceImpl implements OrderService {
    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;
//    private final DiscountPolicy discountPolicy = new FixDiscountPolicy();
    ...
}
```
