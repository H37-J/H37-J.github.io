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
<img src="./img/io.png" alt="사진이 없습니다">

</img>
