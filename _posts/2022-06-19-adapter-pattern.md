---
title: 디자인패턴 - Adapter Pattern
date: 2022-06-19 16:26:00 +09:00
categories: [Design Pattern, Behavioral]
tags: [design pattern, adapter]
---

어댑터(Adapter) 패턴은 `호환되지 않는 인터페이스를 가진 개체들이 서로 협업`할 수 있도록 하는 패턴 구조이다.<br>
다음 조건을 모두 만족한다면 어댑터 패턴으로 리팩토링하는 것이 좋다.<br>
어댑터 구현에는 오브젝트-어댑터와 클래스-어댑터(다중상속)가 있지만 JAVA는 다중상속이 안되므로 오브젝트-어댑터만 정리해보려고 한다.<br>
## 적용 시 고려사항
<ol>
    <li>두 클래스가 동일하거나 유사한 작업을 수행하지만 인터페이스가 다른경우</li>
    <li>두 클래스가 공통 인터페이스를 가지면, 클라이언트 코드가 더 간단하고 명료해질 수 있는 경우</li>
    <li>외부 라이브러리라서 인터페이스를 바꾸고 싶어도 쉽게 바꿀 수 없는 경우, 또는 소스코드를 갖고 있지 않는 경우</li>
</ol>

## UML 클래스 다이어그램

![adapter uml](/assets/img/posts/2022-06-19-adapter-1.png){: width="600" height="300"}<br>
이미지출처 - <https://refactoring.guru/design-patterns/adapter><br><br>

# 변압기를 주제로 어댑터 패턴을 적용해 소스를 구현해봤습니다.

## 인터페이스

```java
//220볼트 인터페이스
interface Volt220 {
    public void connect();
}

//110볼트 인터페이스
public interface Volt110 {
    public void connect();   
}
```

## 인터페이스 상속(Adaptee)

```java
//220볼트 드라이기
public class HairDryer220V implements Volt220 {
    @Override
    public void connect() {
        System.out.println("220V connection success!");
    }
    public void dry() {
        System.out.println("activating 220v hair-dryer");
    }
}

//110볼트 드라이기
public class HairDryer110V implements Volt110 {
    @Override
    public void connect() {
        System.out.println("110V connection success!");
    }

    public void dry() {
        System.out.println("activating 110v hair-dryer");
    }
}
```

## 어댑터(Adapter)

```java
//100볼트 드라이기 220볼트 어댑터
class SetUpTransformer implements Volt220 {
    Volt110 volt110;

    public SetUpTransformer(Volt110 volt) {
        this.volt110 = volt;
    }
    @Override
    public void connect() {
        volt110.connect();
    }

    public void dry() {
        System.out.println("activating 110v to 220v hair-dryer");
    }
}
```

## 사용자

```java
public class Client {
    public static void main(String[] args){
        HairDryer110V hD110V = new HairDryer110V();
        SetUpTransformer sut110to220 = new SetUpTransformer(hD110V);

        sut110to220.connect();
        sut110to220.dry();
    }
}
```

## 실행 결과

![result](/assets/img/posts/2022-06-19-adapter-2.png){: align="left" width="400" height="200"}{: .left}<br><br><br><br><br><br><br>

## 장점
<ol>
    <li>기존에 사용하던 코드를 수정하지 않아도 된다.</li>
    <li>상속이 아닌 구성(Composition)을 이용하기 때문에 Adaptee의 모든 서브클래스에 대해 어댑터를 사용할 수 있다.</li>
    <li>인터페이스 호환성 때문에 같이 쓸 수 없는 클래스들을 연결해서 쓸 수 있다.</li>
</ol>

## 단점
<ol>
    <li>Adaptee객체를 만들어야 한다.</li>
</ol>

## 참고자료

- <https://refactoring.guru/design-patterns/adapter>
- GoF 디자인 패턴
