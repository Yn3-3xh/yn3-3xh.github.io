---
title: "[CS - Data Structure] Stack, Queue"
date : 2024-05-01 00:58:00 +09:00
categories : [CS - Data Structure]
tags : [study, CS, Data Structure, stack, queue]
---

# Stack
입력과 출력이 한 곳(방향)으로 제한된다.
> **LIFO (Last In First Out, 후입선출)**   
> 가장 나중에 들어온 것이 가장 먼저 나옴

함수의 콜스택, 문자열 역순 출력, 연산자 후위표기법 등에 사용된다.

<br>

아래는 스택의 메서드이다.
* push()
  * 데이터 삽입
  * 스택 포인터(SP) 필요
* pop()
  * 데이터 최상위 값 제거
  * 스택 포인터(SP) 필요
* isEmpty()
  * 비어있는지 확인
* isFull()
  * 다 채워져 있는지 확인

> **스택 포인터(SP)**   
> push와 pop을 할 때 해당 위치를 알고 있어야 하므로 SP가 필요하다.   
> SP는 다음 값이 들어갈 위치를 가리키고 있으며 기본값은 -1이다.

## push
```java
public void push(Object obj) {
    if (isFull(obj)) {
        return;
    }
    
    stack[++sp] = obj;
}
```
SP가 최대 크기와 같으면 return하고, 아니면 스택의 최상위 위치에 값을 넣는다.

## pop
```java
public Object pop() {
    if (isEmpty(sp)) {
        return null;
    }
    
    return stack[sp--];
}
```
SP가 0이 되면 null로 return하고, 아니면 스택의 최상위 위치 값을 꺼내온다.

## isEmpty
```java
private boolean isEmpty(int cnt) {
    return sp == -1;
}
```
입력 값이 초기값 -1과 같으면 true, 아니면 false를 return한다.

## isFull
```java
private boolean isFull(int cnt) {
    return sp + 1 == MAX_SIZE;
}
```
SP의 값 + 1이 최대 크기와 값과 같으면 true, 아니면 false를 return한다.

## 동적 배열 스택
위처럼 구현하면 SP와 최대 크기를 비교해서 isFull 메서드로 비교해야되기 때문에 최대 크기가 존재해야 한다.

만약 최대 크기가 없는 스택을 만드려면 arraycopy를 활용한 동적 배열을 사용해야 한다.
```java
public void push(Object obj) {
    if (isFull(sp)) {
        Object[] arr = new Object[MAX_SIZE * 2];
        System.arraycopy(stack, 0, arr, 0, MAX_SIZE);
        stack = arr;
        MAX_SIZE *= 2;
    }
    
    stack[sp++] = obj;
}
```
기존 스택의 2배 크기만큼 임시 배열 arr을 생성한다.   
arraycopy를 통해 stack의 index 0부터 MAX_SIZE만큼을 arr배열의 0번째부터 복사한다.   
복사 후에 arr의 참조값을 stack에 덮어씌운다.   
마지막으로 MAX_SIZE의 값을 증가시킨다.

이렇게 하면 스택이 가득찼을 때 자동으로 확장되는 스택을 구현할 수 있다.

## 연결리스트로 스택 구현
```java
public class StackNode {
    public String data;
    public StackNode link;
  
    public StackNode(String data) {
        this.data = data;
        this.link = null;
    }
  
    public String getData() {
        return this.data;
    }
}
```

```java
public interface MyStack {
    boolean isEmpty();
    boolean isFull();
    void push(String data);
    void pop();
    void peek();
    void clear();
}
```

```java
public class StackAsLinkedList implements MyStack {

    private StackNode head;
    private StackNode top;
    private int stackSize;

    public StackAsLinkedList(int size) {
        this.head = null;
        this.top = null;
        this.stackSize = size;
    }

    @Override
    public boolean isEmpty() {
        return this.head == null;
    }

    @Override
    public boolean isFull() {
        if (isEmpty()) {
            return false;
        } else {
            int nodeCnt = 0;
            StackNode tempNode = this.head;

            while (tempNode.link != null) {
                ++nodeCnt;
                tempNode = tempNode.link; // 다음 노드 참조
            }
            return this.stackSize - 1 == nodeCnt;
        }
    }

    @Override
    public void push(String data) {
        StackNode newStackNode = new StackNode(data);

        if (isFull()) {
            System.out.println("Stack is full!");
        } else if (isEmpty()) {
            this.head = newStackNode;
            this.top = this.head;
        } else {
            StackNode tempNode = this.head;
            while (tempNode.link != null) {
                tempNode = tempNode.link;
            }
            tempNode.link = newStackNode;
        }
    }

    @Override
    public void pop() {
        if (isEmpty()) {
            System.out.println("Stack is empty!");
        }

        if (this.top.link == null) {
            this.head = null;
            this.top = null;
        } else {
            StackNode preNode = this.head;
            StackNode tempNode = preNode.link;
            while (tempNode.link != null) {
                preNode = tempNode;
                tempNode = tempNode.link;
            }
            preNode.link = null;
        }
    }

    @Override
    public void peek() {
        if (isEmpty()) {
            System.out.println("Stack is empty!");
        } else {
            StackNode tempNode = this.head;
            while (tempNode.link != null) {
                tempNode = tempNode.link;
            }
            System.out.println(tempNode.getData());
        }
    }

    @Override
    public void clear() {
        if (isEmpty()) {
            System.out.println("Stack is emptry!");
        } else {
            this.head = null;
            this.top = null;
            System.out.println("Stack is clear!");
        }
    }

    public void pirntStackAsLinkedList() {
        StringBuilder sb = new StringBuilder();
        if (isEmpty()) {
            sb.append("Statck is empty!");
        } else {
            StackNode printNode = this.head;
            while (printNode != null) {
                sb.append(printNode.getData()).append(" ");
                printNode = printNode.link;
            }
        }
        System.out.println(sb);
    }

    public static void main(String[] args) {
        StackAsLinkedList stackAsLinkedList = new StackAsLinkedList(5);
        stackAsLinkedList.push("A");
        stackAsLinkedList.pirntStackAsLinkedList();
        stackAsLinkedList.push("B");
        stackAsLinkedList.pirntStackAsLinkedList();
        stackAsLinkedList.push("C");
        stackAsLinkedList.pirntStackAsLinkedList();
        stackAsLinkedList.push("D");
        stackAsLinkedList.pirntStackAsLinkedList();
        stackAsLinkedList.push("E");
        stackAsLinkedList.pirntStackAsLinkedList();
        stackAsLinkedList.push("F");
        System.out.println();

        System.out.println("Stack is full? = " + stackAsLinkedList.isFull());
        System.out.println();

        stackAsLinkedList.pop();
        stackAsLinkedList.pirntStackAsLinkedList();
        System.out.println();

        stackAsLinkedList.peek();
        System.out.println();

        stackAsLinkedList.clear();
        stackAsLinkedList.pirntStackAsLinkedList();
    }
}
```
**- 실행결과**
```text
A 
A B 
A B C 
A B C D 
A B C D E 
Stack is full!

Stack is full? = true

A B C D 

D

Stack is clear!
Statck is empty!
```

---

# Queue
입력과 출력을 한 쪽 끝(front, rear)으로 제한한다.
> **FIFO (First In First Out, 선입서출)**   
> 가장 먼저 들어온 것이 가장 먼저 나옴

버퍼, 마구 입력된 것을 처리하지 못하고 있는 상황, BFS(너비 우선 탐색) 등에 사용된다.

큐의 가장 첫 원소를 front, 끝 원소를 rear라고 부른다.   
큐는 들어올 때 rear로 들어오지만, 나올 때는 front부터 빠지는 특성을 가진다.   
접근 방법은 가장 첫 원소와 끝 원소로만 가능하다.

아래는 큐의 메서드이다.
* enQueue()
  * 데이터 삽입
* deQueue()
  * 데이터 제거
* isEmpty()
  * 비어있는 지 확인
* isFull()
  * 다 채워져 있는지 확인

데이터를 넣고 뺄 때 해당 값의 위치를 기억해야 한다. (스택에서 SP와 같은 역할)   
이 위치를 기억하고 있는 것이 front와 rear이다.
* front : deQueue할 위치 기억
* rear : enQueue할 위치 기억

> 스택의 SP와 마찬가지로 기본값은 -1이다.   
> 즉, front와 rear 모두 기본값이 -1이다.

## enQueue
```java
public void enQueue(Object obj) {
    if (isFull()) {
        return;
    }
    
    queue[++rear] = obj;
}
```
가득 찼다면 꽉 차있는 상태에서 enQueue를 했기 때문에 overflow가 나고, 아니면 rear에 값을 넣고 1 증가시킨다.

## deQueue
```java
public Object deQueue(Object obj) {
    if (isEmpty()) {
        return null;
    }
    
    Object objQ = queue[front];
    queue[front++] = null;
    return objQ;
}
```
비어있다면 underflow가 나고, 아니면 front에 위치한 값을 object에 꺼낸 후 꺼낸 위치는 null로 채운다.

## isEmpty
```java
public boolean isEmpty() {
    return front == rear;
}
```
front와 rear가 같아지면 비어진 것으로 true, 아니면 false를 return한다.

## isFull
```java
public boolean isFull() {
    return rear == queueSize - 1;
}
```
rear가 큐 크기와 같아지면 가득 찬 것이므로 true, 아니면 false를 return한다.

## Circular Queue
일반 큐는 rear가 끝에 도달했을 때 큐에 빈 메모리가 있어도 꽉 차있는 것처럼 판단할 수도 있는 단점이 있다.

이를 개선한 것이 **원형 큐**이다.   
이는 논리적으로 배열의 처음과 끝이 연결되어 있는 것으로 간주한다.

원형 큐는 초기 공백 상태일 때 **front와 rear가 0**이다.   
공백, 포화 상태를 쉽게 구분하기 위해 **자리 하나를 항상 비워둔다.**
> (index + 1) % size 로 순환시킨다.

### enQueue
```java
public void enQueue(Object obj) {
    if (isFull()) {
        return;
    }
    
    rear = (++rear) % size;
    queue[rear] = obj;
}
```
일반 큐와 달리 원형 큐에서는 rear값을 `rear = (++rear) % size;`로 구한다.

### deQueue
```java
public Object deQueue(Object obj) {
    if (isEmpty()) {
        return null;
    }
    
    preIndex = front;
    front = (++front) % size;
    Object objQ = queue[preIndex];
    return obj;
}
```
일반 큐와 달리 원형 큐에서는 `front = (++front) % size;`로 다음 요소로 이동시킨다.

### isEmpty
```java
public boolean isEmpty() {
    return front == rear;
}
```
일반 큐와 같다.

### isFull
```java
public boolean isFull() {
    return (rear + 1) % size == front;
}
```
일반 큐와 달리 원형 큐에서는 `(rear + 1) % size`가 front와 같은지 비교해야 한다.

원형 큐에도 단점이 있다.   
메모리 공간은 잘 활용하지만, 배열로 구현되어 있기 때문에 큐의 크기가 제한되어 있다는 것이다.

## LinkedList Queue
원형 큐의 단점을 개선한 것이 **연결리스트 큐**이다.   
연결리스트 큐는 **크기가 제한이 없고 삽입, 삭제가 편리**하다.

### enqueue
```java
public void enQueue(E item) {
    Node tampNode = tail;
    tail = new Node();
    tail.item = item;
    tail.next = null;
    
    if (isEmpty()) {
        head = tail;
    } else {
        tampNode.next = tail;
    }
}
```
* 데이터 추가는 끝 부분인 tail에 한다.
* 기존의 tail은 보관하고, 새로운 tail을 생성한다.
* 큐가 비어있으면 `head = tail`로 같은 노드를 가리키도록 한다.
* 큐가 비어있지 않으면 기존 tail의 ndex에 새로만든 tail을 설정해준다.

### deQueue
```java
public T deQueue() {
    if (isEmpty()) {
        tail = head;
        return null;
    } else {
        T item = head.item;
        head = head.next;
        return item;
    }
}
```
* 데이터는 가장 먼저 들어온 것부터 제거해야 하므로 head로부터 꺼낸다.
* head의 데이터를 미리 저장해둔다.
* 기존의 head를 그 다음 노드의 head로 설정한다.
* 저장해둔 데이터를 return해서 값을 빼온다.

---

# References
[gyoogle - 스택 & 큐](https://gyoogle.dev/blog/computer-science/data-structure/Stack%20&%20Queue.html){:target="_blank"}

[freestrokes - Java 연결 리스트로 스택(Stack) 구현하기](https://freestrokes.tistory.com/85){:target="_blank"}

[realizers - Stack이란? 그리고 구현](https://kdg-is.tistory.com/231){:target="_blank"}
