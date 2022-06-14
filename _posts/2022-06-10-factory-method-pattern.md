---
title: 디자인패턴 - Factory Method Pattern
date: 2022-06-10 16:26:00 +09:00
categories: [Design Pattern, Creational]
tags: [design pattern, factory method]
---

팩토리 매소드 패턴이란 생성될 객체의 정확한 클래스를 지정하지 않고 인스턴스를 생성하기 위해 사용하는 패턴이다.<br>
`생성자를 호출하는 대신 인터페이스로 지정하고 자식 클래스 또는 기본 클래스에 구현`하여 재정의된 매소드를 호출하여 선택적으로 객체를 생성할 수 있다.<br>

## UML 클래스 다이어그램

![factory uml](/assets/img/posts/2022-06-10-factory-method-1.png){: width="600" height="300"}<br>

<ol>
    <li>Creator : 팩토리 매서드를 구현하는 클래스</li>
    <li>ConcreteCreator : 팩토리 매서드를 구현하는 클래스로 concreteProduct 객체를 생성</li>
    <li>Product : 팩토리 매서드로 생성될 객체의 공통 인터페이스</li>
    <li>ConcreteProduct : 구체적으로 객체가 생성되는 클래스</li>
</ol>

## Product

```java
//Super Class
public abstract class Computer {
    public abstract String getRam();
    public abstract String getCpu();
    public abstract String getHdd();
    public abstract String toString();
}
```

## ConcreteProduct

```java
//Product PC
public class PC extends Computer{
    private String ram;
    private String cpu;
    private String hdd;

    public PC(String ram, String cpu, String hdd){
        this.ram = ram;
        this.cpu = cpu;
        this.hdd = hdd;
    }

    @Override
    public String getRam(){
        return this.ram;
    }

    @Override
    public String getCpu(){
        return this.cpu;
    }

    @Override
    public String getHdd(){
        return this.hdd;
    }

    @Override
    public String toString(){
        return "this is PC info : cpu=" + getCpu() + ", ram=" + getRam() + ", hdd=" + getHdd();
    }
}

//Product Server
public class Server extends Computer{
    private String ram;
    private String cpu;
    private String hdd;

    public Server(String ram, String cpu, String hdd){
        this.ram = ram;
        this.cpu = cpu;
        this.hdd = hdd;
    }

    @Override
    public String getRam(){
        return this.ram;
    }

    @Override
    public String getCpu(){
        return this.cpu;
    }

    @Override
    public String getHdd(){
        return this.hdd;
    }

    @Override
    public String toString(){
        return "this is Server info : cpu=" + getCpu() + ", ram=" + getRam() + ", hdd=" + getHdd();
    }
}
```

## ConcreteCreator

```java
//Factory Class (Singleton으로 구현해도 되고, 서브클래스를 리턴하는 static 메소드로 구현해도 된다.)
public class ComputerFactory {
    public static Computer getComputer(String type, String ram, String cpu, String hdd){

        if("PC".equalsIgnoreCase(type))
            return new PC(ram, cpu, hdd);
        else if("Server".equalsIgnoreCase(type))
            return new Server(ram, cpu, hdd);
        else
            throw new IllegalArgumentException("Unknown Type : " + type);

    }
}
```

## Creator

```java
public class Factory {
    public static void main(String[] args){
        Computer pc = ComputerFactory.getComputer("PC", "pcRam", "pcCpu", "pcHdd");
        Computer server = ComputerFactory.getComputer("Server", "svRam", "svCpu", "svHdd");

        System.out.println(pc);
        System.out.println(server);
    }
}
```

## Factory 실행 결과

![result](/assets/img/posts/2022-06-10-factory-method-2.png){: align="left" width="400" height="200"}{: .left}<br><br><br><br><br>

## 장점
<ol>
    <li>팩토리 패턴은 클라이언트 코드로부터 서브클래스의 인스턴스화를 제거하여 서로 간의 종속성을 낮추고,
   결합도를 느슨하게 하며, 확장을 쉽게 합니다.</li>
    <li>팩토리 패턴은 클라이언트와 구현 객체들 사이에 추상화를 제공합니다.</li>
    <li>인스턴스 예측 불가능할 때 사용한다. 즉, 어떤 인스턴스를 생성할지 서브클래스가 결정을 내린다.(캡슐화 패턴)</li>
</ol>

## 참고자료

- <https://en.wikipedia.org/wiki/Factory_method_pattern>
- GoF 디자인 패턴
