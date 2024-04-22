---
title: "[Lecture - 김영한님(스프링 핵심 원리 - 기본편)] 빈 생명주기 콜백 인터페이스 InitializingBean, DisposableBean"
date : 2024-04-22 14:39:00 +09:00
categories : [Lecture - 김영한님(스프링 핵심 원리 - 기본편)]
tags : [lecture, inflearn, 김영한님, 스프링 핵심 원리 - 기본편, spring, spring boot, bean, InitializingBean, DisposableBean]
---

inflearn에서 김영한님 강의를 들으면서 내용을 정리해보자.   
[스프링 핵심 원리 - 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8){:target="_blank"}

---

```java
public class NetworkClient implements InitializingBean, DisposableBean {

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

    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("NetworkClient.afterPropertiesSet");
        connect();
        call("초기화 연결 메시지");
    }

    @Override
    public void destroy() throws Exception {
        System.out.println("NetworkClient.destroy");
        disconnect();
    }
}
```
* `InitializingBean`은 `afterPropertiesSet()` 메서드로 초기화를 지원한다.
* `DisposableBean`은 `destroy()` 메서드로 소멸을 지원한다.

실행해보면 아래와 같은 결과가 나온다.
```text
생성자 호출, url = null
NetworkClient.afterPropertiesSet
connect = http://hello-spring.dev
call: http://hello-spring.dev message = 초기화 연결 메시지
NetworkClient.destroy
close: http://hello-spring.dev
```
출력 결과를 보면 초기화 메서드가 주입 완료 후에 적절하게 호출 된 것을 확인할 수 있다.   
그리고 스프링 컨테이너의 종료가 호출되자 소멸 메서드가 호출 된 것도 확인할 수 있다.

**- 초기화, 소멸 인터페이스 단점**
1. 스프링 전용 인터페이스라 해당 코드가 스프링 전용 인터페이스에 의존한다.
2. 초기화, 소멸 메서드의 이름을 변경할 수 없다.
3. 코드를 고칠 수 없으며, 외부 라이브러리에 적용할 수 없다.

> 인터페이스를 사용하는 초기화, 종료 방법은 스프링 초창기에 나온 방법들이라 지금은 더 나은 방법들이 있어서 거의 사용하지 않는다.
