<!-- # 쇼핑몰 프로젝트
## :link: 배포 URL
http://211.193.52.145:8080/

## :clipboard: 프로젝트 계획이유
스프링 프레임워크로 쇼핑몰을 구현해보고 싶어서 개발하게 되었습니다. <br/>
단순 포트폴리오 용도로 개발한 이유도 있지만 프레임워크라는 것이 무엇인지 알아보고 싶었고, 그 중에서도 가장 대중적인 스프링 프레임워크를 선택 하게 되었습니다.
프론트엔드 Unify라는 곳에서 UI를 하나 구매하여 거기에 기능들을 붙였고 자바스립트 기능들을 만들었습니다. 또한 필요한 HTML이나 CSS는 직접 추가,수정 하여 개발 하였습니다

## :clipboard: 사용기술
#### Spring boot
* 스프링 부트
* Spring MVC
* Spring Data JPA
* Spring Boot Batch
* Spring AOP
* Lombok
* Specification

#### Build tool
* Maven

### 프론트엔드
* Javascript
* Thymeleaf
* jQuery


#### Database
* Mysql

#### Exception
* ControllerAdvice

#### 유효성 체크
* @Valid
* BindingResult

## :clipboard: 구현기능
#### 유저 사이드
* 로그인
* 회원가입
* 마이페이지(수정,탈퇴)
* 사진등록
* 휴먼계정(로그인차단)
* 나의 기본주소
* 카드(미구현)

#### 관리자 사이드
* 유저관리
* 휴먼계정 관리
* 상품등록
* 상품관리

#### 상품 사이드
* 상세페이지
* 필터링(사이즈,가격,카테고리 조합해서 필터링 가능)
* 댓글
* 추천
* 삭제
* 수정(미구현)

#### 주문
* 포인트(사용시 할인, 구매시 금액의 1%로 적립)
* 주문관리
* 취소

#### Batch
* CSV(주기적 시간마다 데이터베이스의 데이터를 CSV로 추출)
* 휴먼계정 자동 삭제(미구현)

#### 기타
* 페이지네이션
* 검색
* Ajax요청 후 UI 비동기 처리

## :clipboard: 개발후 느낀점
* 더욱더 깔끔한 코드를 갖고 싶다


## :추가 하고싶은 기능들
* 통계기능
* CSV추출
* 상품추천
* Redis를 이용한 캐시처리
* ElasticSerach 로깅
* Batch서버 따로 구현
* MSA작업
* 리팩토링 작업

  -->
