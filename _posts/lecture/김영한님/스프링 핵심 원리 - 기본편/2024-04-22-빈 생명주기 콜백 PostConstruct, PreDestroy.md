---
title: "[Lecture - 김영한님(스프링 핵심 원리 - 기본편)] 빈 생명주기 콜백 @PostConstruct, @PreDestroy"
date : 2024-04-22 19:58:00 +09:00
categories : [Lecture - 김영한님(스프링 핵심 원리 - 기본편)]
tags : [lecture, inflearn, 김영한님, 스프링 핵심 원리 - 기본편, spring, spring boot, Bean, PostConstruct, PreDestroy]
---

inflearn에서 김영한님 강의를 들으면서 내용을 정리해보자.   
[스프링 핵심 원리 - 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8){:target="_blank"}

---

아래 코드는 빈 생명주기 콜백 할 때 스프링에서 권장하는 방법이다.
```java
public class NetworkClient {

    private String url;

    public NetworkClient() {
        System.out.println("생성자 호출, url = " + url);
    }

    public void setUrl(String url) {
        this.url = url;
    }

    // 서비스 시작시 호출
    public void connect() {
        System.out.println("connect = " + url);
    }

    public void call(String message) {
        System.out.println("call: " + url + " message = " + message);
    }

    // 서비스 종료시 호출
    public void disconnect() {
        System.out.println("close: " + url);
    }

    @PostConstruct
    public void init() {
        System.out.println("NetworkClient.init");
        connect();
        call("초기화 연결 메시지");
    }

    @PreDestroy
    public void close() {
        System.out.println("NetworkClient.close");
        disconnect();
    }
}
```
`@PostConstruct`, `@PreDestroy` 두 어노테이션을 사용하면 가장 편리하게 초기화와 종료를 실행할 수 있다.

**- `@PostContruct`, `@PreDestroy` 어노테이션 특징**
* 최신 스프링에서 가장 권장하는 방법이다.
* 어노테이션 하나만 붙이면 되므로 매우 편리하다.
* 컴포넌트 스캔과 잘 어울린다.
* 유일한 단점은 외부 라이브러리에는 적용하지 못한다는 것이다.
  * 외부 라이브러리를 초기화, 종료해야 한다면 `@Bean`의 기능 `initMethod`, `destroyMethod`를 사용하면 된다.


