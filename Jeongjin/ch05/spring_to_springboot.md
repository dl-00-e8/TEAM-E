# Ch05 스프링에서 스프링 부트로

## 5.1 스프링 부트 소개
스프링 부트는 엄밀하게 말하면 '스프링 프레임워크 개발 도구'라고 봐야함.
스프링 부트는 엔터프라이즈급 애플리케이션을 개발하기 위해서 필요한 기능들을 제공하는 개발 도구임

공식 사이트에서도 스프링과 스프링 부트는 동의어로 처리되고 있는 상황

### 스프링 부트의 중요한 특징
- Auto Configuration(자동 설정)
Ex) 데이터베이스와 관련된 모듈을 추가하면 자동으로 데이터베이스 관련 설정을 찾아서 실행

- 내장 톰캣 + 단독 실행 가능 도구: 별도의 서버 설정 없이도 개발 가능 + 실행 가능

스프링 부트는 Thymeleaf라는 템플릿 엔진 활용

스프링 부트는 화면을 구성하지 않고 데이터만을 제공하는 API 서버 형태도 이용

스프링 부트는 JPA를 이용

>JPA(Java Persistence API)
>: Java 진영에서 ORM(Object-Relational Mapping) 기술 표준으로 사용하는 인터페이스 모음 / 자바 어플리케이션에서 관계형 데이터베이스를 사용하는 방식을 정의한 인터페이스 / 인터페이스이기 때문에 Hibernate, OpenJPA 등이 JPA를 구현함
>
>JPA는 반복적인 CRUD SQL을 처리해준다. 즉, 매핑된 관계를 이용해서 SQL을 생성하고 실행하기에, SQL이 실행될지에 대한 부분만 생각해주면 된다.

### 스프링 부트의 프로젝트 생성 방식
1) Spring Initializr를 이용한 자동 생성
2) Maven이나 Gradle을 이용한 직접 생성

**프로젝트의 기본 템플릿 구조를 만들어 주는 Spring Initializr 방식을 일반적으로 사용**

스프링 부트 설정은 프로젝트 생성 시에 만들어진 application.properties 파일을 이용하거나 application.yml파일을 이용할 수 있음.

Spring Data JPA를 위한 설정
- spring.jpa.hibernate.ddl-auto=update : 테이블이 없을 때는 자동으로 생성 / 변경이 필요할 때는 alter table 실행
- spring.jpa.properties.hibernate.format_sql=true : 실제로 실행되는 SQL 포맷팅 후 출력
- spring.jpa.show-sql=true : JPA가 실행하는 SQL을 같이 출력하도록 함

### 스프링 부트에서 웹 개발
스프링 부트를 이용해서 웹을 개발하는 일은 웹 관련 설정 파일들이 없기 때문에 이를 대신하는 클래스를 작성해 준다는 점이 다름

Thymeleaf는 JSP와 동일하게 서버에서 결과물을 생성해서 보내는 방식이지만 좀 더 HTML에 가깝게 작성할 수 있고 다양한 기능들을 가지고 있음

### JSON 데이터 만들기
API 서버: JSP나 Thymeleaf처럼 서버에서 화면과 관련된 내용을 만들어 내는 것이 아니라 순수한 데이터만 전송하는 방식

JSON을 이용하는 것이 일반적

JSON(JavaScript Object Notation): 구조를 가진 데이터(객체)를 자바스크립트의 '객체 표기법'으로 표현한 순수한 문자열 / 문자열이기에 프로그래밍 언어에 영향 X

## 5.2 Thymeleaf
**Thymeleaf를 이용하기 위해서 가장 중요한 설정은 '네임 스페이스(xmlns)'에 Thymeleaf를 지정하는 것  
-> 네임 스페이스를 지정하면 'th:'와 같은 Thymeleaf의 모든 기능 사용 가능**

### Thymeleaf 출력
: Model로 전달된 데이터를 출력하기 위해서 HTML 태그 내에 'th:,,'로 시작하는 속성을 이용하거나 inlining을 이용

### Thymeleaft 주석  처리
: '\<!--/* ... */-->'을 이용

### th:with를 이용한 변수 선언
: 임시로 변수를 선언해야 할 경우 'th:with'를 이용해서 간단하게 처리
Ex) th:with="num = ${10}"

### 반복문 처리 방법
- 반복이 필요한 태그에 'th:each'를 적용하는 방법
- \<th:block>이라는 별도의 태그를 이용하는 방법

### 반복문의 status 변수
th:each를 사용할 때, index/count/size/first/last/odd/even 등을 이용해서 자주 사용하는 값 출력 가능

### Thymeleaf 제어문
th:if/th:unless/th:switch 사용 가능함
단 th:if와 th:unless는 사실상 별도의 속성으로 사용할 수 있으므로 if~else와는 조금 다르다.
th:switch는 th:case와 같이 사용하며 Switch문으로 처리할 때 사용 가능

### Thymeleaf 링크 처리
th:href="@링크"와 같은 방식으로 처리

링크에서 쿼리 스트링을 처리 할때 'key=value'의 형태로 필요 파라미터를 처리해야 한다면, '()'를 이용하여 파라미터의 이름과 값을 지정 가능하다.  
Ex) th:href="@{/hello(name='AAA', age=16)}"

GET방식으로 처리되는 링크에서 한글이나 공백 문자는 주의해야 하나, Thymeleaf에서는 이에 대한 URL 인코딩 처리가 자동으로 진행됨

### Thymeleaf 특별한 기능
- 인라인 처리 HTML 코드 내에서 \<script th:inline='javascript'>가 지정됨을 통해 자바스크립트 코드를 이용할 때 같은 객체들을 사용 가능

- Thymeleaf의 레이아웃 기능: \<th:block>을 이용하여 레이아웃을 만들고 특정한 페이지에서는 필요한 부분만을 작성하는 방식으로 개발 가능 / 단, 레이아웃 기능을 위해서 별도의 라이브러리가 필요하므로, build.gradle 수정 필요

## 5.3 Spring Data JPA
JPA(Java Persistence API): 자바로 영속 영역을 처리하는 API
JPA는 스프링과 연동할 때 Spring Datat JPA라는 라이브러리 사용

JPA를 이용하는 개발의 핵심은 객체지향을 통해서 영속 계층을 처리하는 데 있음.  
-> JPA를 이용할 때는 테이블과 SQL을 다루는 것이 아니라 데이터에 해당하는 객체를 엔티티 객체라는 것으로 다루고 JPA로 이를 데이터베이스와 연동해서 관리하게 됨.  
(여기서 엔티티 객체는 Primary Key를 가지는 자바의 객체)

단계  
: 엔티티 객체를 생성하기 위한 엔티티 클래스 정의: 반드시 @Entity가 존재하고 해당 엔티티 객체의 구분을 위한 @Id가 필요함
> 자동으로 키를 생성하는 전략
>- IDENTITY: 데이터베이스에 위임(MYSQL/MariaDB) - auto_increment
>- SEQUENCE: 데이터베이스 시퀀스 오브젝트 사용(Oracle) - @SequenceGenerator 필요 
>- TABLE: 키 생성용 테이블 사용, 모든 DB에서 사용 - @TableGenerator 필요
>- AUTO: 방언에 따라 자동 지정, 기본값

### @MappedSuperClass를 이용한 공통 속성 처리  
데이터베이스의 거의 모든 테이블에는 데이터가 추가된 시간이나 수정된 시간 등이 칼럼으로 작성되기에, 이를 쉽게 처리하고자 @MappedSuperClass를 이용해서 공통으로 사용되는 칼럼들을 지정하고 해당 클래스를 상속해서 이를 손쉽게 처리함

### JpaRepository 인터페이스
: Spring Data JPA를 이용할 때 해당 인터페이스를 이용해 인터페이스 선언만으로 데이터베이스 관련 작업을 어느 정도 처리할 수 있음.

### 테스트 코드를 통한 CRUD/페이징 처리 확인
- insert 기능 테스트
: JpaRepository의 save()를 통해서 진행  
save()는 현재의 영속 컨텍스트 내에 데이터가 존재하는지 찾아보고 해당 엔티티 객체가 없을 때는 insert, 존재한다면 update를 자동으로 실행
- select 기능 테스트
: findById()를 이용해 처리 
findById()의 리턴 타입은 Optional\<T>임을 기억해야 한다.
- update  기능 테스트
: save()를 이용해 처리
update는 등록 시간이 필요하므로 가능하다면 findById()로 가져온 객체를 이용해 약간의 수정 후 처리하는 방식으로 진행
- delete 기능 테스트
: deleteById()를 이용해 실행  
이 때, @Id에 해당하는 값을 활용
deleteById()는 데이터베이스 내부에 같은 @Id가 존재하는지 먼저 확인하고 delete 문이 실행됨.
- Pageable과 Page\<E> 타입
: 페이징 처리는 Pageable이라는 타입의 객체를 구성해서 파라미터로 전달함
Pageable은 인터페이스로 설계되어 있고, 일반적으로 PageReques.of()라는 기능을 이용하여 개발 가능
> PageRequest.of(페이지 번호, 사이즈): 페이지 번호는 0부터  
> PageRequest.of(페이지 번호, 사이즈, Sort): 정렬 조건 추가  
> PageRequest.of(페이지 번호, 사이즈, Sort, Direction, 속성...): 정렬 방향과 여러 가지 속성 지정  

리턴 타입으로는 Page\<T> 타입을 이용할 수 있으며, 이는 단순 목록뿐 아니라 페이징 처리에 데이터가 많은 경우에는 count 처리를 자동으로 실행  
**대부분의 Pageable 파라미터는 메소드 마지막에 사용하고, 파라미터에 Pageable이 있는 경우에는 메소드의 리턴 타입을 Page\<T>타입으로 설계함**

JpaRepository에는 findAll()이라는 기능을 제공하며 기본적인 페이징 처리를 지원
리턴 타입은 Page\<T>이며 이는 내부적으로 페이징 처리에 필요한 여러 정보(다음 페이지 존재 여부, 이전 페이지 존재 여부, 전체 데이터의 개수는 몇 개 인지 등의 기능)을 처리

### 쿼리 메소드와 @Query
쿼리 메소드는 보통 SQL에서 사용하는 키워드와 칼럼들을 같이 결합해서 구성하면 그 자체가 JPA에서 사용하는 쿼리가 되는 기능이다.  
일반적으로 메소드 이름은 'findBy..' 또는 'get..'으로 시작하고 칼럼명과 키워드를 결합하는 방식으로 구성한다.  
  
@Query 어노테이션의 value로 작성하는 문자열을 JPQL이라고 하는데 이는 SQL과 유사하게 JPA에서 사용하는 쿼리 언어(query language)라고 생각하면 된다. 
  
JPA는 데이터베이스에 맞게 독립적으로 개발이 가능하므로 특정한 데이터베이스에서만 동작하는 SQL 대신에 JPA에 맞게 사용하는 JPQL을 이용하는 것

작성된 JPQL은 SQL과 상당히 유사하다.

@Query를 이용함으로써 얻을 수 있는 쿼리 메소드에 비교한 장점
- 조인과 같이 복잡한 쿼리를 실행할 수 있는 기능
- 원하는 속성들만 추출해서 Object[]로 처리하거나 DTO로 처리하는 기능
- nativeQuery 속성값을 true로 지정해서 특정 데이터베이스에서 동작하는 SQL을 사용하는 기능

### Querydsl을 이용한 동적 쿼리 처리
JPQL이 정적으로 고정되기 때문에 이를 해결하기 위한 방법으로 Querydsl을 사용한다.  
Querydsl을 이용하기 위해서는 Q도메인이라는 존재가 필요하며, 이는 Querydsl의 설정을 통해서 기존의 엔티티 클래스를 Querydsl에서 사용하기 위해서 별도의 코드로 생성하는 클래스이다.

**Querydsl을 사용하기 위해서는 프로젝트 설정 변경 필요**

### 기존의 Repository와 Querydsl 연동하기
1. Querydsl을 이용할 인터페이스 선언
2. '인터페이스 이름 + Impl'이라는 이름으로 클래스를 선언 - 이 때, QuerydslRepositorySupport라는 부모 클래스를 지정하고 인터페이스를 구현
3. 기존의 Repository에는 부모 인터페이스로 Querydsl을 위한 인터페이스를 지정

### Q도메인을 이용한 쿼리 작성 및 테스트
Querydsl의 목적: '타입' 기반으로 '코드'를 이용해서 JPQL 쿼리를 생성하고 실행하는 것
코드를 만드는 대신 클래스가 Q도메인 클래스인 것

실습 ~p.433 진행 중