---
title: Singleton Pattern
date: 2021-12-21 19:08:00 +09:00
categories: [Blog, Design Pattern]
tags: [design pattern]
---

싱글톤 패턴이란 오직 1개의 인스턴스만 생성하는 패턴이다. 인스턴스를 1개만 생성하는 이유는 불필요한 메모리 누수를 막기 위함이다.
해당 클래스의 인스턴스가 필요할 때마다 생성한다면 리소스 소모가 많아지게 되고, 잘못 사용하여 메모리 해제가 되지 않게 된다면
메모리 누수로 인해 OOM이 발생하게 된다.

상글톤 패턴은 멀티스레드 환경에서 주의해서 사용해야 하는데 그 방법은 다음과 같다.

## synchronized 

멀티 스레드 환경에서 thread-safe하게 접근하기 위해서는 인스턴스를 가져오는 메소드에 `synchronized` 키워드를 붙이면 된다.
해당 키워드를 넣으면 먼저 온 스레드가 작업 중인 상태에서 다른 스레드가 접근할 경우 
나중에 온 스레드는 대기 상태가 되며, 먼저 온 스레드 작업이 끝나면 다시 시작하게 된다.
`synchronized`키워드를 사용하면 하나의 스레드만 처리가 가능하기 때문에 성능이 떨어진다는 단점이 있어 많이 사용되지는 않고 있다.
구현 소스는 다음과 같다.

```java
public class Custom {

    private static Custom custom = null;
    private Custom() {}

    public static synchronized Custom getInstance() {
        if(custom == null) {
            custom = new Custom();
        }
        return custom;
    }
}
```

## double checked locking

이 방법은 인스턴스가 null인지 먼저 체크한 후에 `synchronized` 키워드를 사용하여
기존 `synchronized`의 단점을 어느정도 보완한 방법이다.
null체크를 `synchronized`전과 후 두번 체크한다고 해서 `double checkec locking`이라 불린다.
이 방법을 사용하려면 스레드간 `sync`가 맞지 않는 것을 방지하기 위해 `volatile`로 선언해야 한다.
하지만 이 방법도 결국 경합이 일어나는 부분에서는 `synchronized`로 인한 스레드 대기상태가 발생되기 때문에
완전한 해결은 아니라 볼 수 있다.
구현 소스는 다음과 같다.

```java
public class Custom {

    private static volatile Custom custom = null;
    private Custom() {}

    public static Custom getInstance() {
        if(custom == null) {
            synchronized(Custom.class) {
                if(custom == null) {
                    custom = new Custom();
                }
            }
        }
        return custom;
    }
}
```

## eager initialization

처음부터 멤버변수에 인스턴스를 구현 시켜놓는 방법이다.
멀티스레드 환경에서 `thread-safe`하게 동작하면서도 스레드 동작을 제어하지 않아 성능이 떨어지지 않는 장점이 있다.
이 방법의 단점은 해당 인스턴스를 사용하든 사용하지 않든 미리 메모리에 올려놓고 있기 때문에
메모리가 낭비가 된다는 단점이 있다.
구현 소스는 다음과 같다.

```java
public class Custom {

    private static Custom custom = new Custom();
    private Custom() {}

    public static Custom getInstance() {
        return custom;
    }
}
```

## static inner class

해당 방법은 클래스 내의 `static inner class`를 선언하여 `eager initialization`방법의 단점을 보완한 방법이다.
`static inner class`를 사용할 경우 해당 클래스 로딩시 메모리에 올리는 것이 아니라
그 클래스가 실제 사용될 때 메모리에 올려서 사용하게 된다.
구현 소스는 다음과 같다.

```java
public class Custom {

    private Custom() {}

    private static class CustomHolder {
        private static final Custom INSTANCE = new Custom();
    }

    public static Custom getInstance() {
        return CustomHolder.INSTANCE;
    }
}
```