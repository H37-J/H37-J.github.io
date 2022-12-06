---
layout:     post
title:      "원칙 정리"
subtitle:   "각종 원칙들에 대한 정리"
author:     "H37-J"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - oop
---

### SOLID란?

객체 지향 프로그랭 및 설계의 다섯가지 기본 원칙.

### 의존 관계의 원칙(DIP)

객체가 어떤 모듈에 의존 할 떄, 구체화에 의존하면 안되고 인터페이스 같은 추상적 개념에 의존해야 한다.

다음과 같은 클래스들이 있다.

```java
public class ShinhanBank {

}

public class Client {
    
    private ShinhanBank bank;
}
```

위는 Client 클래스가 ShinhanBank를 의존하고 있다.
그런데 만약 Client 클래스가 신한은행에서 우리은행으로 변경 한다고 하면 Client클래스를 수정해야 할 것이다.
이를 다음과 같이 방지 할 수 있다.

```java
public interface Bank {
    
}

public class ShinhanBank implements Bank {
}

public class WooriBank implements Bank {
}

public class Client {
    
    private Bank bank;
}
```

Client는 인터페이스만을 이용하여 각 객체를 의존 할 수 있다.

### 단일 책임의 원칙(SPR)

모든 클래스는 하나의 책임만 가지며, 클래스는 그 책임을 캡슐화 해야한다.
이것은 클래스나 모듈을 변경 할 때, 단 하나의 이유만으로 변경해야 함을 의미한다.
결제 시스템을 예로 들면, 결제 방법을 바꾸려고 결제 시스템을 바꿀 때, 결제 후 처리과정에 대해 바꾸려고 할 때에는 서로 다른 문제이기 때문에 이것을 모듈로 따로 나누어야 한다.

다음과 같은 결제 시스템이 있다 생각 해보자.

```java
@Service
public class OrderAfterService() {

    public void service() {
        System.out.println("포인트가 적립 되었습니다");
        System.out.println("메일을 보냈습니다");
    }
}


@RequiredArgsConstructor
public class orderService {

    private final OrderAfterService orderAfterService;
    
    public void checkout() {
        orderAfterService.service();
    }
}
```

이는 OrderAfterService 클래스 하나에 너무 많은 로직에 대한 책임이 있다. 또, 나중에 메일에 대한 로ㅈ직이 변경 된다면 포인트 적립에 대한 로직까지 영향을 끼칠 수 있다. 이를 아래와 같이 변경한다

```java
@Service
public class PointService {

    public void service() {
        System.out.println("포인트가 적립 되었습니다");
    }
}

@Service
public class MailService {

    public void service() {
        System.out.println("메일을 보냈습니다");
    }
}
```

위와 같이 변경하면 각 클래스에 단일 책임을 주게 되었고, 메일 서비스를 변경 하게 되더라도 MailService 클래스만 변경하면 된다.


### 개방 폐쇄 원칙(OCP)

클래스, 모듈등은 확장에는 열려 있어야하고, 수정에는 닫혀있어야 한다.
확장에 열려 있어야 하는것은 애플리케이션의 요구 사향이 변경될 때, 그에 맞춰 모듈을 변경 할 수 있어야한다. 수정에 닫혀있어야 한다는것은 모듈의 소스 코드를 수정하지 않아도 모듈의 기능을 확장하거나 변경 할 수 있어야 한다.

아래와 같은 비디오 스트리밍 서비스가 있다.

```java
public class VideoStream {

    String type;

    public void setType(String type) {
        this.type = type;
    }

    public String getType() {
        return type;
    }
}

public class Client {
    
    public void videoService(VideoStream video) {
        switch(video.getType()) {
            case "NetFlix": 
                System.out.pritnln("넷플리스 서비스 실행");
            case "Disney": 
                System.out.println("디즈니 서비스 실행");
            defalut: 
                System.out.println("해당 서비스는 지원하지 않습니다");
        }
    }
}
```

그런데 여기에 유튜브에 대한 비디오 스트리밍 서비스도 추가하려고 한다. case문을 하나 더 늘리면 되겠지만 이는 Client클래스의 비즈니스 로직 자체를 수정하게 되는것이 된다. 이는 OCP 원칙의 수정에 닫혀 있다는 조건을 어기게 된다.
이를 아래와 같이 수정하면 OCP원칙을 지킬 수 있다.


```java
public interface VideoStream {
    
    public void service() 
}

public class Netflix implemnts VideoStream {
    
    public void service() {
        System.out.pritnln("넷플리스 서비스 실행");
    }
}

public class Disney implemnts VideoStream {
    
    public void service() {
        System.out.pritnln("디즈니 서비스 실행");
    }
}

public class YouTube implemnts VideoStream {
    
    public void service() {
        System.out.pritnln("유튜브 서비스 실행");
    }
}

public class Client {

    public void videoService(VideoStream video) {
        video.service()
    }
}
```

앞으로 어떤 서비스가 추가 되더라도 Client 클래스는 변경할 필요가 없어졌고, 인터페이스를 상속한 객체만 만들어 클라이언트 서비스에 전달하면 어떤 서비스라도 이용 할 수 있게 되었다.


### 인터페이스 분리 원칙(ISP)

어떤 인터페이스를 구현하는 클래스를 구현 하려고 할 때, 그 클래스는 인터페이스의 모든 메서드를 구현할 필요가 있는 인터페이스만 상속 받도록 해야한다. 만약 그렇지 않다면 인터페이스를 더 세분화 하여 나눈다.

다음과 같은 User 인터페이스가 있다고 가정해보자.

``` java
public interface User {
    
    void NormalAct() {

    }

    void AdminAct() {

    }
}
```

그리고 이를 상속하는 두 클래스가 있다.

```java
public class NormalUser implements User {

    @Override
    void NormalAct() {
        System.out.println("일반 행위");
    }

    @Override
    void AdminAct() {

    }

}
```

```java
public class AdminUser implements User {

    @Override
    void NormalAct() {
    }

    @Override
    void AdminAct() {
        System.out.println("관리자 행위");
    }

}
```

그런데 이렇게 구현하면 NormarUser 클래스에서는 필요가 없는 AdminAct 메서드 까지 구현을 해줘야 한다.
이는 인터페이스를 상속 받고 필요없는 메서드까지 구현을 하고 있으므로 ISP 원칙에 어긋나게 된다.
따라서 다음과 같이 인터페이스를 분리한다.

```java
public interface NormalUserInterface {
    
    void NormalAct()
}

public interface AdminUserInterface {
    
    void AdminAct()
}

public class NomralUser implemnts NomalUserInterafce {
    
    @Override
    void NormalAct() {
        System.out.println("일반 행위");
    }
    
}
```

위와 같이 ISP원칙을 지킬 수 있다.

### 리스코프의 원칙(LSP)

객체는 프로그램의 정확성을 깨드리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다.
상위 타입의 객체를 하위 타입의 객체로 치환해도 상위 타입을 사용하는 프로그램은 정상 작동 해야한다.  

다음과 같은 Item 클래스가 있다고 생각 해보자.

```java
public class Item {
    private int price;

    public int getPrice() {
        return price;
    }

    public void setPrice(int price){
        this.price = price;
    }
}
```

그리고 이를 상속하는 상품중 하나인 Phone 클래스가 있다.

```java
public class Phone extends Item {
    
    private double rate;

    public void setRate(double rate) {
        this.rate = rate;
    }

    @Override
    public void setPrice(int price) {
        super.setPrice(price - (int)(rate * price));
    }
}
```

만약 위와 같이 Phone 클래스를 작성 한다면 해당 Item 클래스의 setPrice의 메서드 자체가 할인된 가격을 set 하는 메서드로 변경 되어진다. 이는 Item 클래스의 setPrice에 대한 비즈니스 로직을 바꾸는 것이고 LSP 원칙을 어기게 되는 것이다. 이를 아래와 같이 바꾸면 LSP 원칙을 지키게 된다.

```java
public class Phone extends Item {
    
    private double rate;

    public void setRate(double rate) {
        this.rate = rate;
    }

    public void applyPrice(int price) {
        super.setPrice(price - (int)(rate * price));
    }
}

```

Phone 클래스에서 따로 applyPrice에 메서드를 추가 하였기 때문에 setPrice에 대한 로직은 그대로이다.
따라서 LSP 원칙을 지키게 된다.

