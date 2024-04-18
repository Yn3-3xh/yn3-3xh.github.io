---
title: "[CS - Data Structure] Array vs ArrayList vs LinkedList"
date : 2024-04-18 10:57:00 +09:00
categories : [CS - Data Structure]
tags : [study, CS, Data Structure, Array, ArrayList, LinkedList]
---

# Array vs ArrayList vs LinkedList

* Array
  * index로 빠른 조회 가능
  * 사용 용도
    * 데이터 개수가 고정적이고 삽입 및 삭제가 빈번하지 않은 경우
    * 데이터 접근이 빈번한 경우
    * 기본 자료형(int, char, double, ...), Object를 사용하는 경우
* ArrayList
  
  ![ArrayList](https://drive.google.com/thumbnail?id=1jhQlj7CZxrsvPPk89i6TN8EPMcEUntW9&sz=w400)
  * index로 빠른 조회 가능
  * 사용 용도
    * 데이터 개수가 예상 가능하고, 삽입 및 삭제가 빈번하지 않은 경우
    * 데이터 접근이 빈번한 경우
    * 참조 자료형(Integer, Character, Double, ...), Object를 사용하는 경우
  
* LinkedList
  
  ![LinkedList](https://drive.google.com/thumbnail?id=1NT32t9FQ_52CriHZ3QQB93OQ55UTjOJU&sz=w600)
  * 데이터의 삽입 및 삭제가 빠름
  * 사용 용도
    * 데이터 삽입 및 삭제가 빈번한 경우
    * 데이터 접근이 빈번하지 않은 경우
    * 데이터 개수가 많지 않은 경우
    > 삽입 및 삭제도 맨 앞이나 맨 뒤 노드가 아니면 해당 위치로 접근해야 하기 때문에 데이터가 많은 경우 느려질 수 있다.

## Array
Array는 선언할 때 크기와 데이터 타입을 아래 코드처럼 지정해야 한다.
```java
int[] arr = new int[10];
String[] arr = new String[5];
```
이처럼 array는 메모리 공간에 할당할 크기를 미리 정해놓고 사용하는 자료구조이다.   
따라서 계속 데이터가 늘어날 때 최대 크기를 알 수 없을 때는 사용하기에 부적합하다.   
또한 중간에 데이터를 삽입하거나 삭제할 때도 매우 비효율적이다.   

만약 4번째 index 값에 새로운 값을 넣어야 한다면 원래 4번째에 있던 값을 뒤로 밀어내고 해당 index에 덮어씌워야 한다.   
즉, 기본적으로 크기를 정해놓은 배열에서는 해결하기 부적합한 점이 많다.   
대신 배열을 사용하면 index가 존재하기 때문에 위치를 바로 알 수 있어 조회하기에 편한 장점이 있다.

## ArrayList
Array의 크기를 정해놓는 단점을 해결하기 위해 나온 것이 ArrayList이다.
```java
ArrayList<Integer> arrList = new ArrayList<>();
ArrayList<Book> bookList = new ArrayList<>();
```

ArrayList는 Array처럼 크기를 정해주지 않아도 된다.   
하지만 실제로는 Array로 구현되어 있어서 데이터 추가 시 크기가 넘어간다면 내부적으로 크기가 1.5배 큰 배열을 생성하여 기존 배열의 내용을 복사한다.   
따라서 ArrayList도 진정한 의미의 가변적 크기를 갖는 것은 아니다.

ArrayList에 Primitive type(기본 자료형)을 저장할 수는 있다.
```java
ArrayList<Integer> arrList = new ArrayList<>();
arrList.add(10);
```
하지만 위 코드처럼 추가가 될 경우 JVM에서 Autoboxing(내부적으로 primitive type을 타입에 상응하는 object로 변환, int->Integer)을 통해 ArrayList에 Object만 저장되도록 한다.   
따라서 내부적으로는 아래 코드처럼 수행된다.
```java
arrList.add(new Integer(10));
```

일단 크기가 정해져있지 않기 때문에 중간에 데이터를 추가하거나 삭제하더라도 Array에서 갖고 있던 문제점은 해결 가능하다.   
또한 index를 가지고 있으므로 조회도 빠르다.   
하지만 중간에 데이터를 추가 및 삭제할 때 시간이 오래걸린다는 단점이 존재한다. (추가 및 삭제 시 데이터를 당겨오거나 밀어내는 연산이 추가되어 메모리 낭비)

## LinkedList
LinkedList는 단일, 다중 등 여러가지가 존재한다.   
종류가 무엇이든 한 노드에 연결될 노드의 포인터 위치를 가리키는 방식으로 되어있다.

이런 방식을 활용하면서 데이터의 중간에 삽입 및 삭제를 하더라도 전체를 돌지 않아도 이전 값과 다음 값이 가리켰던 주소값만 수정하여 연결시켜주면 되기 때문에 빠르게 진행할 수 있다.

이렇게만 보면 가장 좋은 방법 같지만 n번째 값을 조회한다면 처음부터 n번째까지 순차적으로 조회해야 하기 때문에 비효율적이다.

> [LinkedList](https://yn3-3xh.github.io/posts/%EC%97%B0%EA%B2%B0-%EB%A6%AC%EC%8A%A4%ED%8A%B8-%EA%B5%AC%EC%A1%B0-%EB%B0%8F-%EA%B5%AC%ED%98%84/){:target="_blank"}의 더 자세한 내용은 따로 정리했다.   

---

# 정리
조회가 많을 것 같다면 Array나 ArrayList를 사용하되,   
크기가 고정될 것 같다면 Array, 변동될 것 같다면 ArrayList를 사용해보자.

삽입 및 삭제가 많을 것 같다면 LinkedList를 사용해보자.

---

# Refernces

[gyoggle - Array vs ArrayList vs LinkedList](https://gyoogle.dev/blog/computer-science/data-structure/Array%20vs%20ArrayList%20vs%20LinkedList.html){:target="_blank"}

[letskuku - Array vs ArrayList vs LinkedList](https://velog.io/@letskuku/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Array-vs-ArrayList-vs-LinkedList){:target="_blank"}

[you88 - 배열(array), ArrayList, LinkedList 차이점 + 사용 용도 정리](https://you88.tistory.com/28){:target="_blank"}

[humblechoi - Array vs ArrayList](https://velog.io/@humblechoi/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Array-vs-ArrayList){:target="_blank"}
