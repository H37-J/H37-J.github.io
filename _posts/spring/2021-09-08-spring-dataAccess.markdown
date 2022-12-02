---
layout:     post
title:      "데이터 액세스에 대하여"
subtitle:   "데이터 액세스에 대한 정리"
author:     "H37-J"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - spring
---

### 개념

스프링은 무겁고 불편한 EJB설정을 대신할 선언적 구성관리와 POJO기반 개발을 지원하려고 나왔다.

## 하이버네이트

ORM라이브러리중 가장 성공 하였다. 하이버네이트가 나오고 EJB는 엔터티 빈을 JPA로 교체 하였다.  
하이버네이트의 핵심 개년은 SessionFactory가 관리하는 Session인터페이스를 중심으로 전개된다.
하이버네이트를 표준 자바 퍼시스턴스 API의 퍼시스턴스 제공자로 사용 할 수 있다

### 즉시로딩

@ManyToMany(fecth=FetchType.EAGER) 처럼 즉시로딩으로 설정하면 하이버네이트는 객체를 쿼리할때마다 연관 레코드도 모두 조회한다. 하지만 이것은 데이터 조회성능에 영향을 끼친다.

### JPA

JPA의 목적은 JEE 환경에서 ORM프로그래밍 모델을 표준화 하는것이다.
JPA의 핵심은 EntityManager인터페이스이며 인터페이스 구현체는 EntityManger Factory타입의 팩터리에서 얻을 수 있다.