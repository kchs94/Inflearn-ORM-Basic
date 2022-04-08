# 자바 ORM 표준 JPA 프로그래밍 - 기본편 정리 

## 1.JPA 소개

### 목차

- SQL 중심 개발의 문제점
- JPA 소개

### 상황

- 현대 대부분 애플리케이션을 만들 때 객체지향언어를 사용한다.
- 이러한 객체지향 애플리케이션을 만들 때 잘 맞는 DB가 관계형 DB다.
- RDB를 제어하려면 SQL을 사용해야 한다.
- 그래서 개발자들은 SQL을 작성하게 된다.

### SQL 중심 개발의 문제점

1. CRUD 쿼리를 엄청나게 작성해야 한다.
   - 객체를 SQL로 표현하고
   - SQL을 객체로 표현해야 한다.
2. 패러다임이 일치하지 않는다
   - 객체지향적인 모델링을 하면 오히려 더 복잡해진다.
3. 객체가 자유롭게 객체 그래프를 탐색할 수 없다.
4. 엔티티를 신뢰할 수 없다.


### 이렇게 힘든 상황에 놓인 이유: 패러다임의 불일치

- 객체지향언어는 객체지향적(캡슐화,속성과 기능, 상속 등) 목적을 따른다.
- RDB는 데이터를 잘 정규화해서 보관하는 것을 목적으로 한다.
- 두 사상의 방향이 일치하지 않는다
- 상속
  - 객체지향에는 있지만 RDB에는 없다
- 연관관계
  - 객체지향은 참조변수를 사용하지만 RDB는 외래키를 이용한 join을 사용한다.
- 데이터 타입
- 데이터 식별 방법
- 등.. 비슷한 개념에 방법이 다르다.

### 객체 지향적으로 테이블을 모델링하면 어떨까?

- Long 외래키를 만들지 말고 참조변수를 선언하자
- 결과: 데이터를 조회할 때 오히려 더 SQL 의존적인 모델링보다 더 복잡해진다!

### 객체를 컬렉션처럼 DB에 저장하고 불러올 수 없을까?


### JPA 등장

- Java Persistence API
- 자바 진영의 ORM 기술


### ORM이 무엇이죠?

- Object-Relational Mapping(객체 관계 매핑) 기술
- 객체는 객체답게 설계해라
- RDB는 RDB에 맞게 설계해라
- ORM 프레임워크가 중간에 매핑해준다!!!
- 개발자는 쿼리를 작성하지 않아도 된다. JPA가 작성한다.
- 대중적인 언어는 대부분 ORM이 존재한다.
- 패러다임 불일치를 해결한다.

### JPA는 애플리케이션과 JDBC 사이에서 동작합니다.

- 애플리케이션 안에 JAP 기술이 있다.
- JPA는 객체와 SQL을 매핑해준다.
- 최종적으로는 SQL을 DB에 보내고
- DB는 SQL 결과를 보내면 JPA가 객체로 변환한다.

### JPA가 DB에 객체를 저장하는 법

- MemberDAO에서 Entity 객체를 persist한다.
- JPA는 엔티티를 분석해서 INSERT SQL을 생성한다.
- JDBC API를 이용해서 DB로 쿼리를 보낸다.

### JPA가 DB에서 객체를 받는 법

- MemberDAO에서 find(id)한다.
- JPA는 SELECT SQL을 생성한다.
- JPA는 JDBC API를 사용해서 SELECT 쿼리를 DB에 전송한다.
- DB는 결과를 반환한다.
- JPA는 ResultSet으로 매핑하여 Entity 객체를 MemberDAO에 반환한다.


### JPA 역사

- 오래전 과거
  - EJB라는 과거 자바 ORM
    - 문제: 사용하기 어렵고, 느리고, 잘 동작도 안함
- 과거
  - 하이버네이트
  - 오픈 소스
- 현재
  - JPA
  - 하이버네이트와 거의 복사 붙이기 수준


### JPA 표준 스펙

- JPA는 인터페이스의 집합이다.
- JPA2.1 표준을 구현한 구현체는 3가지가 있다.
  - 하이버네이트(90%), EclipseLink, DataNucleus


### JPA 버전
- JPA1.0(JSR 220) 2006: 초기 버전, 복합 키와 연관관계 기능이 부족한 버전
- JPA2.0(JSR 317) 2009: 대부분의 ORM 기능을 포함. JPA Criteria 추가. 이때부터 쓸만해짐
- JPA2.1(JSR 338) 2013: 스토어 프로시저 접근, 컨버터(Converter), 엔티티 그래프 기능이 추가

### 그래서 JPA 왜 사용해야 된다고?!

1. SQL 중심적인 개발에서 탈출해서 객체지향적인 개발을 할 수 있다.
2. CRUD를 한 줄에 해결하므로 생산성이 올라간다
   - 저장: `jpa.persist(member)`
   - 조회: `Member member = jpa.find(memberId)`
   - 수정: `member.setName("변경할 이름")`
   - 삭제: `jpa.remove(member)`
3. 유지보수가 쉽다
   - 기존: 필드 변경시 모든 SQL을 수정해야 한다.
   - JPA 도입 후 : SQL을 작성할 필요x
   - 상황 예시
     - 기획자: "회원에 전화번호 넣어야 돼요~ 오늘까지 끝내주세요"
     - JPA 기술을 모르던 충섭:  야근완료 
     - JPA 배운 충섭: 3초완료
4. 패러다임 불일치를 해결해준다
   - 상속 불일치 해결
     - 저장 
       - 개발자가 할 일
         - jpa.persist(album);
       - 나머지 JPA가 처리
         - INSERT INTO ITEM..
         - INSERT INTO ALBUM..
     - 조회
       - 개발자가 할 일
         - `Album album = jpa.find(Album.class, albumId);`
       - 나머지 JPA가 처리
         - SELECT I.\*, A.*
         - FROM ITEM I
         - JOIN ALBUM ON I.ITEM_ID = A.ITEM_ID
   - 연관관계 불일치 해결
     - member.setTeam(team);
     - jpa.persist(member);
   - 객체 그래프 탐색 불일치 해결
     - Member member = jpa.find(Member.class, memberId);
     - Team team = member.getTeam();
   - 비교 불일치 해결
     - String memberId = "100";
     - Member member1 = jpa.find(Member.class, memberId);
     - Member member2 = jpa.find(Member.class, memberId);
     - member1 == member2; // 같다!!
     - 동일한 트랙잭션에서 조회한 엔티는 같음이 보장된다.
5. 성능이 좋아진다
   1. 1차 캐시와 동일성(identity)를 보장한다
      1. 같은 트랙잭션 안에서는 같은 엔티티를 반환한다 - 약간 조회 성능 향상
      > String memberId = "100";  
       Member m1 = jpa.find(Member.class, memberId); // SQL  
       Member m2 = jpa.find(Member.class, memberId); // 캐시  
       println(m1 == m2) // true
      
      1. 트랜잭션을 지원하는 쓰기 지연(transactional write-behind) - Insert
         1. 트랙재션을 커밋할 때까지 SQL을 모은다
         2. JDBC BATCH SQL 기능을 사용해서 한번에 SQL을 전송한다
      
         > transaction.begin(); // 트랜잭션 시작  
          .    
          em.persist(memberA);  
          em.persist(memberB);  
          em.persist(memberC); 여기까지 INSERT SQL을 DB에 보내지 않는다  
          transaction.commit(); // 커밋하는 순간 DB에 모은 것을 보낸다.
      

   3. 지연로딩(Lazy Loading)과 즉시 로딩을 고를 수 있다.
      - 지연 로딩: 객체가 실제 사용될 때 로딩된다.
      - 즉시 로딩: JOIN SQL로 연관된 객체까지 미리 조회 
   4. 데이터 접근 추상화와 벤더 독립성
   5. 표준이다

--- 

# 2.JPA 시작하기

### 목차 

- JPA 프로젝트 생성하기
- JPA 애플리케이션 개발하기


## JPA 프로젝트 생성하는 법

### 1.H2 데이터베이스를 설치한다.

- http://www.h2database.com/
- 실습용으로 딱이다
- 가볍다(1.5M)
- 웹용 쿼리툴을 제공한다
- MySQL, Oracle 방언을 제공한다

### 2.인텔리제이에서 메이븐 프로젝트를 생성합니다.

- 메이븐이란?
  - https://maven.apache.org/
  - 자바용 프로젝트 관리도구다.
    - 장점
      - 라이브러리를 관리해준다.(버전관리, 자동 다운로드, 의존성 관리 등)
      - 프로젝트의 라이프사이클(작성,컴파일, 테스트 등) 각 테스트를 지원한다
  - 자바 라이브러리 중 하나
  - 최근에는 그레이들(Gradle)을 많이 사용

- 프로젝트 생성
  - 자바 8이상 권장합니다
  - 메이븐 설정
    - groupId: jpa-basic
    - artifactId: ex1-hello-jpa
    - version: 1.0.0

### 3.pom.xml에서 필요한 라이브러리를 추가합니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 파일명: pom.xml -->
  <project xmlns="http://maven.apache.org/POM/4.0.0"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
 <modelVersion>4.0.0</modelVersion>
 <groupId>jpa-basic</groupId>
 <artifactId>ex1-hello-jpa</artifactId>
 <version>1.0.0</version>
<dependencies>
<!-- JPA 하이버네이트 -->
<dependency>
<groupId>org.hibernate</groupId>
<artifactId>hibernate-entitymanager</artifactId>
<version>5.3.10.Final</version>
</dependency>
<!-- H2 데이터베이스 -->
<dependency>
<groupId>com.h2database</groupId>
<artifactId>h2</artifactId>
<version>1.4.199</version>
</dependency>
</dependencies>
</project>
```

- 하이버네이트 라이브러리
  - hibernate.core: 핵심 라이브러리
  - javax-persistence-api: 하이버네이 구현체가 JPA 인터페이스를 가지고 있다. 즉 여기에 JPA 인터페이스가 들어있다.
  - 최신 버전도 있지만 5.3.10을 사용하는 이유는 대부분 실무에서 스프링 부트와 JPA를 같이 사용한다. 따라서 스프링부트 버전에 맞는 JPA 버전을 선택해야 한다.
- 스프링부트 버전에 맞는 JPA 버전 선택하는 방법
  - 스프링 홈페이지 -> PRJOJECTS -> SPRING BOOT -> LEARN -> 내가 사용하는 스프링부트 버전의 Reference Doc -> Ctrl + F로 binernate 검 -> 5.3.10구나!
- H2 데이터베이스 라이브러리
  - 데이터베이스에 접근할 수 있도록 해주는 드라이버(라이브러리, 의존성)
  - 컴퓨터에 다운받은 h2 버전과 같아야 오류가 없다.

### 4.persistence.xml 설정파일 만들기

- JPA 설정파일
- JPA는 기본적으로 데이터베이스를 사용하기 때문에 설정 파일에 접근 정보를 설정해야한다
  - java.persistence.jdbc.driver: 드라이버
  - java.persistence.jdbc.user: 사용자명
  - java.persistence.jdbc.password:비밀번호
  - java.persistence.jdbc.url: 접근 url
  - hibernate.dialect: h2 사투리를 사용한다는 것을 알려줌
    - JPA는 특정 데이터베이스에 종속되지 않는다
    - 방언(사투리):SQL 표준을 지키지 않는 특정 DB만의 고유한 기능을 JPA가 알아서 변환해준다.
    - JPA가 Dialect 옵션을 사용하면 자동으로 DB에 맞게 MySQL/Oracle/H2 SQL을 생성한다.
    - H2: org.hibernate.dialect.H2Dialect
    - Oracle: org.hibernate.dialect.Oracle10Dialect
    - MySQL: org.hibernate.dialect.MySQLDialect
- javax.**는 하이버네이트 말고 다른 구현체도 적용이 가능한 옵션
- hibernate.**는 하이버네이트만 적용가능한 옵션
- 경로는 /resource/META-INF/persistence.xml
- persistence-unit name: 설정파일 이름을 설정
- hibernate.show_sql: DB에 쿼리가 들어가는 것을 콘솔에 출력하는 옵션
- hibernate.format_sql: SQL을 깔끔하게 구조적으로 보여주는 옵션
- hibernate.use_sql_comments: 주석을 통해 SQL이 작성된 이유를 보여주는 옵션

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 파일명: persistence.xml-->
<persistence version="2.2"
xmlns="http://xmlns.jcp.org/xml/ns/persistence" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence http://xmlns.jcp.org/xml/ns/persistence/persistence_2_2.xsd">
<persistence-unit name="hello">
<properties>
<!-- 필수 속성 -->
<property name="javax.persistence.jdbc.driver" value="org.h2.Driver"/>
<property name="javax.persistence.jdbc.user" value="sa"/>
<property name="javax.persistence.jdbc.password" value=""/>
<property name="javax.persistence.jdbc.url" value="jdbc:h2:tcp://localhost/~/test"/>
<property name="hibernate.dialect" value="org.hibernate.dialect.H2Dialect"/>
<!-- 옵션 -->
<property name="hibernate.show_sql" value="true"/>
<property name="hibernate.format_sql" value="true"/>
<property name="hibernate.use_sql_comments" value="true"/>
<!--<property name="hibernate.hbm2ddl.auto" value="create" />-->
</properties>
</persistence-unit>
</persistence>
```

## JPA 애플리케이션 개발하기

### JPA 작동 방식

- Persistence 클래스에서 설정정보를 읽어서 엔티티매니저팩토리(EntityManagerFactory)를 생성한다.
- 앤티티매니저팩토리는 EntityManager를 요청마다 생성한다.

### 1.DB에 Member 테이블 만들기

- 테이블이 있어야 데이터를 넣고 뺄 수 있다.
- 터미널에서 h2/bin/h2.sh 실행하기
- open ./h2.sh (h2.sh 실행하기)

```h2
create table Member(
    id bigint not null,
    name varchar(255),
    primary key (id)
);
```

### 2. DB 테이블에 대응되는 Member Entity 생성하기

- java/hellojpa/Member 생성하기

```java
package hellojpa;

import javax.persistence.Entity;
import javax.persistence.Id;
         // JPA가 관리할 객체
@Entity // JPA를 사용하는 클래스로 인식, 해당 엔티티 클래스에 대응하는 DB 테이블 'Member' 자동 맵핑
//@Table(name = "USER")   // DB에 있는 'USER' 테이블에 수동 매핑
public class Member {
    
    @Id // JPA에게 기본키(PK)가 무엇인지 알려준다.
    private Long Id;
    
    //@Column(name = "username") // 열 수동 매핑
    private String name;
    
    //Getter, Setter 생성
}
```

### 3. 회원등록/단 건 조회/ 삭제/ 수정실습하기

#### hellojpa/JpaMain.java 생성하기

```java
package hellojpa;
// 기본 셋팅

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;
import javax.persistence.Persistence;
import java.util.List;

public class JpaMain {

    public static void main(String[] args) {
        // 팩토리를 만드는 순간 DB 연결도 되고 웬만한게 다 된다.
        // 애플리케이션 로딩 시점에 딱 하나만 만들어야 한다.
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

        // 이제 실제 동작하는 코드를 작성한다
        // DB 데이터를 저장하기, 불러오기 등.
        // 일관적인 한 단위(ex 트랜잭션)마다 em을 꼭 생성해야 한다.
        EntityManager em = emf.createEntityManager();

        em.close(); // DB 연결한 상태에서 동작하기 때문에 사용을 다하면 꼭 닫아줘야 한다.
        emf.close();    // 실제 애플리케이션이 종료되면 엔티티매니저팩토리를 종료한다.
    }
}

```

#### 좋지 않은 코드 - 1.회원등록
- 문제가 생겼을 때  자원을 닫을 수 없으므로
```java

EntityTransaction tx = em.getTransaction();
tx.begin();

Member member = new Member();
member.setId(2L);
member.setName("HelloB");

// 멤버 저장
em.persist(member);

tx.commit();

em.close();

emf.close();    // 실제 애플리케이션이 종료되면 엔티티매니저팩토리를 종료한다.
```

#### 안전한 코드 -1.회원등록
- try-catch-finally 사용
- 실무에서는 스프링을 사용하면 이러한 처리를 알아서 해주기 때문에 persist만하면 된다.
```java
EntityTransaction tx = em.getTransaction();
tx.begin();
try {
    Member member = new Member();
    member.setId(2L);
    member.setName("HelloB");
    em.persist(member); // 저장
    tx.commit();    // 정상이면 커밋
} catch (Exception e) {
    tx.rollback();  // 문제있으면 롤백
} finally {
    em.close(); // DB 연결한 상태에서 동작하기 때문에 사용을 다하면 꼭 닫아줘야 한다.
}
emf.close();    // 실제 애플리케이션이 종료되면 엔티티매니저팩토리를 종료한다.
```

#### 2.회원 단건 조회
```java
try {
    Member findMember = em.find(Member.class, 1L);
    System.out.println("findMember.id = " + findMember.getId());
    System.out.println("findMember.name = " + findMember.getName());
    
    tx.commit(); // 정상이면 커밋
}
```

#### 3.회원 삭제
```java
Member findMember = em.find(Member.class, 1L);

em.remove(findMember);
```

#### 4.회원 수정

```java
try {
    Member findMember = em.find(Member.class, 1L);
    findMember.setName("HelloJPA");

// em.persist(member) 안해도 DB 수정이 반영된다!!! 콘솔에 출력된 쿼리를 보면 UPDATE가 보내진다!!
// JPA를 통해서 엔티티를 가져오면 그 엔티티는 이제 JPA가 관리를 한다.
// 변경이 생기면 UPDATE 쿼리를 만들어서 보낸다.
// 즉 트랙잭션 커밋을 하기 전에 UPDATE 쿼리를 보내고 커밋을 한다.

    tx.commit(); // 정상이면 커밋
}

```

### 엔티티매니저팩토리와 엔티티매니저
- 엔티티 매니저 팩토리
  - 엔티티 매니저 팩토리는 DB당 딱 하나만 생성된다.
  - 서버가 올라가는 시점에 생성된다.
  - 애플리케이션 전체에서 공유한다
- 엔티티 매니저
  - 쓰레드간 공유하면 안된다.
  - 요청이 올 때마다 생성되고 버린다.
- JPA의 모든 데이터 변경은 트랙잭션 안에서 실행되야한다.
  - RDB는 데이터 변경을 트랜잭션 안에서 실행하도록 설계되어있다.
  - 트랙잭션을 명시적으로 사용하지 않아도 내부적으로 DB가 트랙잭션을 건다

### 단순한 조회는 알겠는데 복잡한 조건이 있는 조회는 어떻게 해야될까?
- 단순한 조회
  - EntityManager.find()
  - 객체 그래프 탐(a.getB().getC())
- 나이가 18살 이상인 회원을 모두 검색하고 싶다면?
- 실무의 고민
  - 테이블이 겁나 많다!
  - 내가 원하는 데이터를 최적화해서 가져오고 싶다.
  - 통계성 쿼리도 날려야되는데,,,
- 이럴 때를 대비해서 JPA는 **JPQL** 기술이 있다!
  - JPQL이란 객체를 대상으로 하는 객체지향 쿼리를 작성할 수 있게 해주는 기술이다
  - JPQL은 테이블을 보고 쿼리를 작성하지 않는다. 객체를 보고 쿼리를 작성한다.

### JPQL

- 한마디로 객체지향SQL
- JPQL을 사용하면 검색할 때도 엔티티 객체 중심으로 개발할 수 있다.
- 문제는 검색 쿼리다.
- 모든 DB 데이터를 객체로 변환해서 검색하는 것은 불가능하다.
- 애플리케이션이 필요한 데이터만 DB에서 불러오려면 결국 검색 조건이 포함된(WHERE) SQL이 필요하다.
- JPA는 SQL을 추상화한 JPQL이라는 객체지쿼리언어를 제공한다.
  - SQL을 추상화했기 때문에 특정 DBMS SQL에 의존하지 않는다.
- SQL과 문법이 유사하고, SELECT, FROM, WHERE, GROUP BY, HAVING, JOIN을 지원한다
- JPQL은 엔티티 객체를 대상으로 쿼리를 작성하기 땜문에 DB를 바꾸더라도(=방언이 변경되더라도) 코드를 바꿀 필요가 없다.
  - 설정 옵션만 해당 방언으로 변경하면 된다.
- JPQL을 사용하게되면 방언이 적용된 SQL이 나가게 된다.

### JPQL 실습

#### JPQL로 전체 회원 검색
```java
        List<Member> result = em.createQuery("select m from Member as m", Member.class)
        .setFirstResult(5)
        .setMaxResults(8)
        .getResultList();
```

