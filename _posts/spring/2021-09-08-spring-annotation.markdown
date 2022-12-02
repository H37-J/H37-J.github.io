---
layout:     post
title:      "빈에 대하여"
subtitle:   "빈에 대한 정리"
author:     "H37-J"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - spring
---

### @SprinbBootApplication

@Confiuration + @EnableAutoConfiguration + @ComponentScan

### @EnableAutoConfiguration

스프링에게 빈을 classpath, 다른 빈, 다양한 설정에 추가 하도록 알린다.

### @Configuration, @ComponentScan

애플리케이션 내의 빈 정의를 찾는다. 이 파일 안에 빈을 정의한다
이 클래스는 자바 기반 구성의 파일임을 알린다.

### @Bean

스프링 빈과 DI요구사항을 정의하는데 사용한다.

### @Service

해당 클래스가 비즈니스 서비스 로직을 다룸을 의미

### @Repository

해당 클래스에 데이터 액세스 로직이 들어있음을 의미

### @RequestBody

HTTP요청 본문의 내용을 자동으로 도메인 객체에 바인딩 함.

### @Lob

대량 객체 컬럼임을 알려준다

### @EnableWebSecurity

스프링 웹 세큐리티를 활성화, MVC intergration을 제공한다.

### @JsonIgnoreType

출력에 무시할 속성을 표시하는데 사용

```java
@JsonIgnorePropertis({"id"})
public static class user {
    public int id;
    public String name;
}
```  