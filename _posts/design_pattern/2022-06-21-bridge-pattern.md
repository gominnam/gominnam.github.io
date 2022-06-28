---
title: 디자인패턴 - Bridge Pattern
date: 2022-06-21 18:47:00 +09:00
categories: [Design Pattern, Structural]
tags: [design pattern, bridge]
---

브릿지(Bridge)패턴은 `구현부에서 추상층을 분리하여 각자 독립적으로 변형`하여 밀접하게 관련된 클래스 집합을 서로 독립적으로 개발할 수 있게 하는 구조 패턴이다.

![bridge uml](/assets/img/posts/design-pattern/2022-06-21-bridge-1.png){: width="500" height="250"}<br>
이미지출처 - <https://refactoring.guru/design-patterns/bridge><br><br>

### 적용 시 고려사항

<ul>
    <li>두 클래스가 동일하거나 유사한 작업을 수행하지만 인터페이스가 다른경우</li>
    <li>두 클래스가 공통 인터페이스를 가지면, 클라이언트 코드가 더 간단하고 명료해질 수 있는 경우</li>
    <li>외부 라이브러리라서 인터페이스를 바꾸고 싶어도 쉽게 바꿀 수 없는 경우, 또는 소스코드를 갖고 있지 않는 경우</li>
</ul>
<br>

## UML 클래스 다이어그램

![bridge uml](/assets/img/posts/design-pattern/2022-06-21-bridge-2.png){: width="600" height="300"}<br>
이미지출처 - <https://refactoring.guru/design-patterns/bridge><br><br>

# 상점에 패턴을 적용해 구현해봤습니다.

## 인터페이스(Implementation)

```java
public interface Service {
    public void deal();
    public void guide();
}
```

## 인터페이스 구현(Concrete)

```java
//키오스크 판매
public class Kiosk implements Service{
    @Override
    public void deal(){
        System.out.println("kiosk.. sale completed!");
    }

    @Override
    public void guide(){
        System.out.println("kiosk.. showing sales list..");
    }
}

//직원 판매
public class Employee implements Service{
    @Override
    public void deal(){
        System.out.println("employee sale completed!");
    }

    @Override
    public void guide(){
        System.out.println("employee recommend a product");
    }
}
```

## 추상클래스(Abstraction)

```java
public abstract class Store {
    protected Service service;

    public Store(Service service){
        this.service = service;
    }

    public abstract void sale();
}
```

## 추상클래스 구현(Refined Abstraction)

```java
//무인판매점
public class UnmannedStore extends Store{
    public UnmannedStore(Service service) {
        super(service);
    }

    @Override
    public void sale(){
        service.guide();
        service.deal();
    }
}

//유인판매점
public class MannedStore extends Store{
    public MannedStore(Service service) {
        super(service);
    }

    public void sale(){
        service.guide();
        service.deal();
    }
}
```

## 클라이언트(Client)

```java
public class Client {
    public static void main(String[] args){
        Store unmannedStore = new UnmannedStore(new Kiosk());
        Store mannedStore = new MannedStore(new Employee());

        unmannedStore.sale();
        System.out.println("-----------------------------------------");
        mannedStore.sale();
    }
}
```

## 실행 결과

![result](/assets/img/posts/design-pattern/2022-06-21-bridge-3.png){: align="left" width="400" height="200"}{: .left}<br><br><br><br><br><br><br>

## 장점
<ol>
    <li>구현에서 추상을 분리하여, 이들이 독립적으로 다양성을 가질 수 있도록 한다.</li>
    <li>지속적인 종속 관계를 피할 수 있다.</li>
    <li>확장성이 향상된다.</li>
    <li>클라이언트로부터 구현 정보 은닉.</li>
</ol>

## 단점
<ol>
    <li>디자인이 복잡하다.</li>
    
</ol>


## 참고자료

- <https://refactoring.guru/design-patterns/bridge>
- <https://en.wikipedia.org/wiki/Bridge_pattern>
- GoF 디자인 패턴