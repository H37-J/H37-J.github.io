---
layout:     post
title:      "내가 생각하는 IoC와 DI에 대하여"
subtitle:   "IoC와 DI"
author:     "H37-J"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - spring
---

### DI

의존성 주입은 제어역전을 구현하는 방법중 하나이다. 기존에 자바에서 객체를 생성하려면 new로 항상 객체를 생성 해야 했고 의존성을 주입 했어야 했다. 하지만 DI를 이용하면 이를 없애고 마치 모듈처럼 객체를 빈으로 관리하여 필요할 때 의존성 주입을 할 수 있다. 즉 결합도가 낮아지고 응집도가 높아진다.

### DI에 대한 예제

다음과 같은 인터페이스가 있다고 가정 해보자.

```java
public interface DataService {
    
    public void service();
}

```

이 인터페이스를 구현하는 두 개의 서비스 클래스가 있다.

```java
@Slf4j
public class MysqlService implements DataService {
    public void service() {
        log.info("mysql 서비스 실행");
    }
}

@Slf4j
public class OracleService implements DataService {
    public void service() {
        log.info("오라클 서비스 실행");
    }
}
```

그리고 다음과 같이 빈 설정을 해준다.

```java
@Configuration
public class DataServiceBean {
    @Bean
    public DataService dataService() {
        return new OracleService();
    }
}
```

클라이언트는 데이터서비스에 대한 의존성 주입을 아래와 같이 받을 수 있다.

```java
@RestController
@RequestMapping("/api/di")
@RequiredArgsConstructor
public class Client {
    
    private final DataService dataService;

    @RequestMapping(value = "/service", method = RequestMethod.GET)
    public void service() {
        dataService.service();
    }

}
```

위에서 오라클 서비스를 주입 받았기 때문에 오라클 서비스가  실행 되었다.
또한, 컴파일 단계에서는 DataServeice인터페이스를 보지만 런타임 단계에서는 IoC가 발생해 역전되어 OracleService를 실행하게 된다

<img src="https://raw.githubusercontent.com/H37-J/H37-J.github.io/main/_posts/spring/img/io.png" alt="사진이 없습니다">

### DI의 장점
* 각 클래스를 빈으로 만들어 모듈처럼 따로 관리 할 수 있다. 즉 결합도가 낮아지고 응집도가 높아진다
* 사용자는 모듈이 어떻게 만들어졌는지에 대한 것은 신경 쓰지 않고 그저 가져다 쓰기만 하면 된다

### IoC란?

개발자가 만든 애플리케이션 코드가 프레임워크에 의해 제어된다. 외부 라이브러리, 프레임워크가 필요한 애플리케이션 코드를 선택하여 흐름을 진행한다. Controller, Serivce같은 객체들의 동작을 개발자가 구현하지만 어느 시점에 호출 할지는 프레임 워크가 결정한다, 예를들어 @Test어노테에이션을 붙이면 Junit 프레임워크가 해당 메서드를 호출 하는것 처럼 사용자의 코드를 프레임워크가 호출한다