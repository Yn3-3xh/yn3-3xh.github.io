---
title: "[Study - CS > Computer Architecture] ARM Processor"
date : 2024-04-03 21:06:00 +09:00
categories : [Study - CS > Computer Architecture]
tags : [study, CS, Computer Architecture, ARM]
---

# ARM
ARM은 Advanced RISC Machine(진보된 RISC 기기)의 약어로, ARM의 핵심은 RISC이다.
> RISC (Reduced Instruction Set Computing, 감소된 명령어 집합 컴퓨팅)   
> "**단순한 명령 집합을 가진 processor**가 복잡한 명령 집합을 가진 processor 보다 훨씬 더 효율적이지 않을까?" 에서 탄생

> processor : 메모리에 저장된 명령어들을 실행하는 유한 상태의 자동기계

## ARM 구조
ARM은 칩의 기본 설계 구조만 만들고, 실제 기능 추가와 최적화 부분은 개별 반도체 제조사의 영역으로 맡긴다.   
따라서 물리적 설계는 같아도 명령 집합이 모두 다르기 때문에 서로 다른 칩이 되기도 하는 것이 ARM이다.

소비자에게는 칩이 논리적 구조인 명령 집합으로 구성되면서, 이런 특성 때문에 물리적 설계 베이스는 같지만 용도에 따라 다양한 제품군을 만날 수 있는 특징이 있다.

아키텍처는 논리적인 명령 집합을 물리적으로 표현한 것이므로, 명령어가 많고 복잡해질수록 실제 물리적인 칩 구조도 크고 복잡해진다.   
하지만 ARM은 RISC 설계 기반으로 '단순한 명령 집합을 가진 processor가 복잡한 것보다 효율적'임을 기반하기 때문에 명령 집합과 구조 자체가 단순하다.   
따라서 **ARM 기반 processor가 더 작고 효율적이며 상대적으로 느리다.**

단순한 명렵 집합은 적은 수의 트랜지스터만 필요하므로, 간결한 설계와 더 작은 크기를 가능하게 한다.   
반도체 기본 부품인 트랜지스터는 전원을 소비해 다이의 크기를 증가시키기 때문에 스마트폰이나 태블릿을 위한 processor에는 가능한 적은 트랜지스터를 가진 것이 이상적이다.
> 다이 : 해당 트랜지스터가 실제로 동작하는 반도체 칩의 핵심 부분

따라서 명령 집합의 수가 적기 때문에 트랜지스터 수가 적고, 이를 통해 크기가 작고 전원 소모가 낮은 ARM CPU가 스마트폰, 태블릿과 같은 모바일 기기에 많이 사용된다.

## ARM의 장점
ARM을 위해 개발된 processor는 오직 ARM processor가 탑재된 기기에서만 실행할 수 있다. (즉, x86 CPU processor 기반 프로그램에서는 ARM 기반 기기에서 실행할 수 없다.)   
따라서 ARM에 실행되던 프로그램은 x86 processor에서 실행되도록 하려면 프로그램 수정이 필요하다. (반대의 경우도 마찬가지이다.)

하지만 하나의 ARM 기기에서 동작하는 OS는 다른 ARM 기반 기기에서도 잘 동작한다.   
이러한 장점 덕분에 수많은 버전의 안드로이드가 탄생하고 있다.   
또한 HP나 블랙베리의 태블릿에도 안드로이드가 탑재될 수 있는 가능성이 생기게 되었다.
> 애플은 IOS 소스 코드를 공개하지 않고 있기 때문에 애플 기기는 불가능하다.

---

# Reference
[gyoogle - ARM 프로세서](https://gyoogle.dev/blog/computer-science/computer-architecture/ARM%20%ED%94%84%EB%A1%9C%EC%84%B8%EC%84%9C.html){:target="_blank"}
