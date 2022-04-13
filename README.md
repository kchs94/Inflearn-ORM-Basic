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

---

## 3. 영속성 관리    

### JPA에서 가장 중요한 2가지

- 객체와 관계형 DB 매핑하기
  - 설계와 관련된 관점
  - 정적인 관점
- 영속성 컨텍스트
  - JPA가 실제 내부적으로 어떻게 동작할까?
  - 동적인 관점

### 엔티티 매니저 팩토리와 엔티티 매니저

<img width="1074" alt="image" src="https://user-images.githubusercontent.com/49191949/162744095-aaf2f6f5-a771-4300-88a0-f2e46894541b.png">

- 고객의 각 요청마다 엔티티 매니저를 생성한다.
- 엔티티 매니저는 커넥션 풀을 사용해서 DB에 접근한다.

### 영속성 컨텍스트란 뭘까?

- 컨텍스트 = 문맥, 환경
- 영속하다: 영원히 계속하다.
- 영속성 컨텍스트란 엔티티를 영구적으로 저장하는 환경을 말한다.
- em.persist(entity)
  - 처음에는 엔티티를 DB에 저장한다고 설명했다.
  - 사실은...
  - 영속성 컨텍스트로 엔티티를 영속한다라는 의미다.
  - 즉, 엔티티를 DB가 아닌 엔티티 컨텍스라는 곳에 저장하라는 의미.

### 엔티티 매니저? 영속성 컨텍스트?

- 영속성 컨텍스트는 논리적인 개념이기에 눈에 보이지 않는다.
- 엔티티 매니저를 통해 영속성 컨텍스트에 접근할 수 있다.

### 환경

#### J2SE 환경

<img width="870" alt="image" src="https://user-images.githubusercontent.com/49191949/162745165-793e01e8-8200-43ad-8ab9-700b8d146681.png">

- 엔티티를 생성하면 1:1로 영속성 컨텍스트가 생성되는 환경
- 쉽게 생각하자면 엔티티 매니저 안에 영속성 컨텍스트가 생성된다고 생각하자.

#### J2EE

<img width="935" alt="image" src="https://user-images.githubusercontent.com/49191949/162745475-f5fb4073-08ef-4c92-a4e2-6d07ad458c5a.png">

- 엔티티 매니저와 영속성 컨텍스트가 N:1인 환경
- 나중에 배운다.


### 엔티티의 생명주기

<img width="580" alt="image" src="https://user-images.githubusercontent.com/49191949/162745672-7fc5d1ec-3a9e-4a23-89e5-4ffadaed72b0.png">

- 비영속(new/transient)
  - 영속성 컨텍스트와 관계가 없는 새로운 상태
  - 처음 객체를 생성한 상태
- 영속(managed)
  - 영속성 컨텍스트에 관리되는 상태
  - persist()한 후의 객체 상태
  - em.find()해서 1차캐시에 없고 DB에 있을 때에도 영속 상태로 된다.
- 준영속(detached)
  - 영속성 컨텍스트에 저장되었다가 분리된 상태
- 삭제(removed)
  - 영속성 컨텍스트에 삭제된 상태

### 비영속(new)

<img width="566" alt="image" src="https://user-images.githubusercontent.com/49191949/162746070-56257b76-7d07-42d4-a4b1-2701ab2506cf.png">

- 객체를 생성만한 상태
- JPA와 관련이 없는 상태

```java
// 객체를 생성한 상태
Member member = new Member();
member.setId("member1");
member.setUsername("회원1");
```

### 영속(managed)

<img width="354" alt="image" src="https://user-images.githubusercontent.com/49191949/162747101-02e291cf-150a-447c-a7bd-76ba5645af6c.png">

- persist()로 인해 엔티티 매니저 안 영속성 컨텍스트에 들어간 상태
- 1차 캐시에 저장된 상태

```java
// member 엔티티의 상태:비영속
Member member = new Member();
member.setId("member1");
member.setUsername("회원1");

EntityManger em = emf.creatEntityManager();
em.getTransction().beign();

// member 엔티티의 상태: 영속
em.persist(member);
```

#### persist()에서는 DB에 저장되지 않는다.

```java
// persist()할 때 insert 쿼리가 db에 전송될까?
System.out.println("=====전=====");
em.persist(member);
System.out.println("=====후=====");

tx.commit();

```

<img width="288" alt="image" src="https://user-images.githubusercontent.com/49191949/162748270-756e5b5b-1a99-4e4f-a9ca-3eeed05af5a7.png">

- 전, 후 사이에 insert 가 생성되지 않았다.
- tx.commit()에서 insert 쿼리가 생성되고 DB에 전송된다.

### 준영속(detached), 삭제(removed)

- 준영속
  - // 회원 엔티티를 영속성 컨텍스트에 분리한 상태
  - em.detached(member);
- 삭제
  - // 객체를 삭제한 상태
  - em.remove(member)
  - 실제 DB에서 데이터를 삭제한 상태
  - DELETE 쿼리가 나가게 된다.

### 영속성 컨텍스트는 무엇이 좋을까?

- 애플리케이션하고 DB사이에 무엇(영속성 컨텍스트)가 존재하는 상황
  - 버퍼링, 캐시 등에서 이점
1. 1차 캐시
2. 동일성(Identity) 보장
3. 트랙잭션을 지원하는 쓰기지연(transactional write-behind)
4. 변경 감지(dirty checking)
5. 지연 로딩(lazy Loading)

### 장점1:1차 캐시를 통한 엔티티 조회

<img width="515" alt="image" src="https://user-images.githubusercontent.com/49191949/162749654-59a8fe5a-1ef8-441b-8f03-59cab5a58bcf.png">

- 영속성 컨텍스트는 1차 캐시를 갖고 있다.
- 1차 캐시
  - 키(key): db에 매핑 기본키(PK)
  - 값(value): 엔티티 객체값
- 예시
  - 키 = "member1"
  - 값 = 멤버객체값
- 엔티티 조회 할 때
  - 첫 번째로 영속성 컨텍스트 1차 캐시에 엔티티가 있는지 본다.
    - 있으면 가져온다.
    - 없으면(DB에 있다고 가정)
      - DB에서 조회한 후 1차 캐시에 저장 후 반환한다.

### 1차 캐시에서 조회

<img width="780" alt="image" src="https://user-images.githubusercontent.com/49191949/162750585-44cbc14f-5dc6-405b-9c55-9fb07872e0c2.png">

```java
Member member = new Member();   // value
member.setId("member1");    // key
member.setUsername("회원1");

// 1차 캐시에 엔티티 저장
em.persist(member);

// 1차 캐시에서 조회
Member findMember = em.find(Member.class, "member1");
```

### 데이터베이스에서 조회

<img width="759" alt="image" src="https://user-images.githubusercontent.com/49191949/162751035-d7273e94-3b08-4e44-922b-a84d2c6e14bf.png">

`Member findmember2 = em.find(Member.class, "member2")`

- member2 엔티티는 캐시에 없었던 상황
- 사실, 성능상 큰 이점을 주진 않는다. 1차 캐시는 여러명의 고객이 공유하는 캐시가 아니다.
- 고객의 한 요청이 들어오면 엔티티 매너지가 생성되고 영속성 컨텍스트, 1차 캐시도 생성된다. 그리고 비즈니스 로직이 끝나면 앞에 3개 모두 소멸된다.
- 애플리케이션 전체에서 공유하는 캐시는 2차 캐시다. 1차 캐시는 한 트랜잭션 안에서 비즈니스 로직이 굉장히 복잡할 때와 같은 상황에서 성능상 이점이 있다.

#### 조회할 때 조회 쿼리가 생성될까?

<img width="831" alt="image" src="https://user-images.githubusercontent.com/49191949/162753684-cc024ad2-2b6b-4d07-9241-f495c9773cce.png">

- 콘솔 출력을 보면 em.find()에서 조회 쿼리가 생성되지 않았음을 알 수 있다.
- 즉, db에 엔티티를 저장한 것이 아니라 1차 캐시에 저장한 후 1차 캐시에서 조회한 사실을 알 수 있다.

### 같은 엔티티 두 번 조회하는 상황

<img width="1098" alt="image" src="https://user-images.githubusercontent.com/49191949/162754963-a547d8d1-04e2-439e-adae-6fa16469b512.png">

- 첫 번째 조회에서는 1차 캐시에 없으니 DB에서 조회함 따라서 조회 쿼리 생성
- 두 번째 조회는 1차 캐시에서 조회하므로 조회 쿼리가 생성되지 않음

### 장점2: 영속성 엔티티의 동일성 보장

<img width="662" alt="image" src="https://user-images.githubusercontent.com/49191949/162755782-01b165ca-bf95-4688-9c78-dcff2020b514.png">

<img width="683" alt="image" src="https://user-images.githubusercontent.com/49191949/162755676-1598fea9-bace-4b45-9da6-8330f8e792bf.png">

- 1차 캐시로 반복 가능한 읽기(REPEATABLE READ) 등급의 트랜잭션 격리 수준을 데이터베이스가 아닌 애플리케이션 차원에서 제공한다.

### 장점3:엔티티 등록할 때 - 트랙잭션을 지원하는 쓰기 지연

<img width="696" alt="image" src="https://user-images.githubusercontent.com/49191949/162758875-cbbf65cb-8af7-45bb-8add-c1f67adae681.png">

- persist()에서 INSERT SQL을 DB에 전송하지 않는다.
- commit()에서  INSERT SQL을 DB에 전송한다.

<img width="757" alt="image" src="https://user-images.githubusercontent.com/49191949/162758991-1fc13d55-a281-49f6-a185-62f9e0e12591.png">

- 첫 번째 persist
  - 우선, 1차 캐시에 엔티티를 저장한다.
  - 그리고 동시에 쓰기 지연 SQL 저장소에 SQL INSERT A 쿼리를 쌓는다.
- 두 번째 persist
  - 1차 캐시에 엔티티 memberB 저장한다.
  - 그리고 동시에 쓰기 지연 SQL 저장소에 SQL INSERT B 쿼리를 쌓는다.

<img width="715" alt="image" src="https://user-images.githubusercontent.com/49191949/162759107-7a28642d-a9a7-4336-895b-d4eb90be7532.png">

- 트랙재션을 commit()하는 시점 
  - 쓰기 지연 SQL 저장소에 있는 쿼리들이 flush()되면서 전송된다.
  - 그리고 실제 DB 트랜잭션이 커밋된다.

#### 실습

- 객체를 set으로 설정하기 귀찮으니 생성자를 만들자
  - JPA는 기본적으로 기본 생성자를 만들어 줘야 한다.
  - 내부적으로 리플렉션을 사용하기 때문에 동적으로 객체를 생성해야 한다. 그래서 기본생성자가 필요하다.

- Member.java

<img width="618" alt="image" src="https://user-images.githubusercontent.com/49191949/162761642-ecb0416b-6557-4246-95c1-c97631ab7fe7.png">

- main 함수와 출력 결과

<img width="934" alt="image" src="https://user-images.githubusercontent.com/49191949/162761883-3cc975a9-39d8-4e2e-9f29-7555126de637.png">

- 출력 결과를 보면 persist()가 아닌 commit()에서 INSERT 쿼리가 DB에 전송되는 것을 알 수 있다.

<img width="1209" alt="image" src="https://user-images.githubusercontent.com/49191949/162763531-90aaf280-70e3-41dc-8535-8bb4a9f685d3.png">

- <property name="hibernate.jdbc.batch_size" value="10"/>
- JDBC 배치를 설정하는 옵션
  - 위의 설정은 쿼리를 최대 10개까지 모아서 한번의 네트워크로 보낼 수 있다.
  - 즉 버퍼링 기능

### 장점4: 엔티티 수정 감지(더티 체킹)

<img width="696" alt="image" src="https://user-images.githubusercontent.com/49191949/162764250-1c828f50-7bc8-41c4-b9b9-42a228558bb7.png">

- JPA는 변경 감지 기능을 제공한다.
- 엔티티 값을 수정한 후에 persist()를 하지 않아도 DB값이 변경된다.
- 마치 자바의 컬렉션처럼!

<img width="711" alt="image" src="https://user-images.githubusercontent.com/49191949/162764389-d0f5b8c0-3689-4891-91d7-cb3f14d544b2.png">

- 비밀은 영속성 컨텍스트 안에 있다.
- 트랜잭션을 commit()을 하면
  - 내부적으로 flush() 호출을 한다.
  - 두 번쨰로, 엔티티마다 엔티티와 스냅샷을 비교한다.
    - 1차 캐시에는 사실 스냅샷도 있다.
    - 스냅샷: 영속성 컨텍스트에 들어온 최초 시점의 엔티티 상태를 저장한다.
    - 변화가 있으면?
      - 쓰기 지연 SQL 저장소에 UPDATE 쿼리를 생성한다.
      - 그리고 DB에 반영하고 커밋한다.
- remove도 같은 메커니즘이다.



#### 실습

- DB에 키가 150L인 엔티티의 이름을 A에서 B로 바꿔보기

<img width="485" alt="image" src="https://user-images.githubusercontent.com/49191949/162764953-c193e88d-2602-48ce-97bb-e34979c13753.png">

<img width="928" alt="image" src="https://user-images.githubusercontent.com/49191949/162766161-916f2fae-85c1-41e3-8794-d6b6993bbfc8.png">

- em.persist()를 사용하지 않았는데도
- sql 출력을 보면 update 쿼리가 생성된 것을 볼 수 있다!

<img width="327" alt="image" src="https://user-images.githubusercontent.com/49191949/162766437-6fe272ec-46b9-44b9-abd4-8578da182939.png">

- DB에 변경이 반영된 것을 알 수 있다.

### flush란?

- 영속성 컨텍스트 변경내용을 데이터베이스에 반영하는 것을 말한다.

### 플러시(flush)는 발생 과정

- 변경감지(더티 체킹, dirty checking)을 한다.
- 수정된 엔티티에 대한 SQL을 쓰기 지연 SQL 저장소에 등록한다.
- 쓰기 지연 SQL 저장소의 쿼리를 데이터베이스에 전송할 때(등록, 수정, 삭제 쿼리)

### 영속성 컨텍스트를 플러시하는 방법 3가지
1. em.flush() - 직접 호출
   - 직접 사용할 일은 거의 없다. 나중에 테스트할 때 가끔 사용.
2. **트랜잭션 커밋** - 플러시 자동 호출
3. JPQL 쿼리 실행 - 플러시 자동 호출

#### 실습

- 커밋하기 전에 미리 DB에 SQL을 전송하고 싶다.

<img width="1003" alt="image" src="https://user-images.githubusercontent.com/49191949/162771623-f7707948-0444-49c6-889c-c56c9c46f038.png">

- ==== 선 위에 SQL 쿼리라 전송되었다.
- flush()에서 쿼리가 바로 DB에 반영이 되었음을 알 수 있다.

### JPQL 쿼리 실행시 플러시가 자동으로 호출이 되는 이유

<img width="713" alt="image" src="https://user-images.githubusercontent.com/49191949/162769692-6b8a0109-041b-43d5-9e4f-915fede59888.png">

- JPQL로 select를 해야되는데 DB에 저장된게 없다. 
- 그래서 JPA는 이러한 문제를 방지하기 위해 JPQL을 실행하기 전에 무조건 flush()을 호출한다.

### 플러시 모드 옵션

<img width="568" alt="image" src="https://user-images.githubusercontent.com/49191949/162769840-1da754a3-030b-428e-8a51-634328a8592d.png">

- FlushModeType.AUTO
  - 커밋이나 쿼리를 실행할 때 플러시하기 (디폴트)
- FlushModeType.COMMIT
  - 커멋할 때만 플러시하기
- 거의 다룰 일이 없고, 디폴트를 사용하기를 권장함

### 플러시는

- 영속성 컨텍스트를 비우지 않는다.
- 영속성 컨텍스트 변경내용을 데이터베이에 동기화하는 것이다.
- 트랙잭션이라는 단위가 중요하다. --> **커밋 직전에만 동기화(flush)** 하면된다.

### 준영속 상태

- 영속 상테의 엔티티를 영속성 컨텍스트에서 분리시킨다.(detached)
- 영속성 컨텍스트가 제공하는 기능을 사용하지 못한다.
  - 더티 체킹 등

### 준영속 상태 만드는 법

- em.detach(entity)
  - 특정 엔티티만 준영속 상태로 전환한다.
- em.clear()
  - 영속성 컨텍스트를 완전히 초기화 한다.
- em.close()
  - 영속성 컨텍스트를 종료한다.

#### 실습

- "ZZZZZZ" 이름을 가진 엔티티를 "AAAAA"로 바꾸기

<img width="914" alt="image" src="https://user-images.githubusercontent.com/49191949/162775682-44737f52-a98b-4b58-a37b-e8c3fdf9a738.png">

- 결과를 보면 이름을 바꿨는데도 SELECT 쿼리 이후에 UPDATE 쿼리가 나가지 않음
  - em.detach()로 JPA가 더이상 관리를 하지 않기 때문이다.
  - 직접 쓸일은 거의 없고 나중에 웹 애플리케이션 개발할 때 도움이 될 수 있다.

- em.clear 사용하기

<img width="942" alt="image" src="https://user-images.githubusercontent.com/49191949/162778100-13016197-44c2-438d-8c60-ce29b35cb565.png">

- 영속성 컨텍스트가 완전히 초기화 되었다. 1차 캐시가 초기화 되었다.
- 그래서 select 쿼리가 2번 실행됬다.
- 근데 왜 =====이전에 실행될까??(질문)
- 언제사용
  - 뭔가 1차 캐시 관계없이 테스트케이스 작성할 때 도움이 된다.

---

## 4. 엔티티 매핑

### 목차

- 객체와 테이블 매핑하는 방법
- 데이터베이스와 스키마 자동 생성하기
- 필드와 컬럼 매핑하기
- 기본키 매핑하기
- 실전 예 - 1.요구사항을 분석하고 기본 매핑하기

### 객체와 테이블 매핑하기

#### 엔티티 매핑 소개

- 객체와 테이블 매핑은 @Entity, @Table
- 필드와 컬럼 매핑은 @Column
- 기본키 매핑은 @Id
- 연관관계 매핑은 @ManyToOne, @JoinColumn

### 객체와 테이블 매핑

### @Entity

- @Entity가 붙은 클래스를 엔티티라고 한다.
- 엔티티는 JPA가 관리한다.
- 엔티티와 테이블을 매핑하려면 필수적으로 작성해야 한다.
- 주의
  - 기본 생성자를 반드시 작성해야 한다.(파리미터가 없는 public 또는 protected)
    - JPA 스펙상 규정이 되어있다.
  - final 클래스, enum 클래스, interface, inner 클래스는 사용(매핑)할 수 없다.
  - DB에 저장할 필드에 final를 사용할 수 없다.


### @Entity 속성

- name
  - JPA에서 사용할 엔티티 이름을 지정한다.
  - 디폴트값은 클래스명이다.(예:Member)
  - 중복되는 클래스명이 없다면 가급적 디폴트값을 사용한다.
  - 일반적으로 거의 사용하지 하지는 않는다.

### @Table

- name 속성을 사용해서 엔티티와 매핑할 테이블을 직접 지정한다.

<img width="1251" alt="image" src="https://user-images.githubusercontent.com/49191949/162898687-ff646785-3f38-4785-9427-c7e512566b0a.png">

- DB마다 다르지만 스키마를 구분할 수도 있다.
- 어떤 DB의 경우 카탈로그도 있는데 이것도 구분할 수 있다.

<img width="682" alt="image" src="https://user-images.githubusercontent.com/49191949/162960864-65ad9aae-7402-48ac-9e12-7b4cf207b22d.png">


### 데이터베이스 스키마(테이블) 자동 생성하기 

- DDL을 애플리케이션이 실행되면  모든 엔티티에 대응하는 테이블을 (기존에 있으면 삭제후 - create) 자동으로 생성한다.
  - JPA 사용전: DB에 테이블을 먼저 생성하고 그리고 나서 객체를 만듬
  - JPA 사용후: 객체를 만들고 매핑해 놓으면, 필요하면 테이블을 생성해준다.
- 테이블 중심이 아닌 객체 중심적이다.
- 데이터베이스 방언 옵션을 사용하여 적절한 DDL을 생성한다.
- 개발 단계나 로컬 PC에서 개발할 때 도움이 된다.
- 생성된 DDL은 운영 서버 사용하면 안된다.
  - 만약 운영에서 사용한다고하면 다듬어야 한다.

<img width="642" alt="image" src="https://user-images.githubusercontent.com/49191949/162962766-5c431a58-38c4-4670-b7e0-a9a9a62de077.png">

### 데이터베이스 스키마 자동 생성하기 - 속성

- hibernate.hbm2ddl.auto

<img width="1232" alt="image" src="https://user-images.githubusercontent.com/49191949/162899134-9b1e8ef1-7961-4f38-9356-f89de438d53e.png">



### 데이터베이스 스키마 자동 생성하기 - 실습

- 스키마 자동 생성 설정하기
<img width="1105" alt="image" src="https://user-images.githubusercontent.com/49191949/162963581-3ab4a74d-f83e-4063-bc47-9d82491246d2.png">

- 스키마 자동생성하기 실행 및 옵션 확인
  - update: 기존 테이블을 생성한 후 엔티티에 필드를 추가한 다음에 실행하면 alter로 열을 추가한다.(열을 지우는 것은 안된다, 위험한 동작이라)
    <img width="673" alt="image" src="https://user-images.githubusercontent.com/49191949/162963916-5717c036-ea1e-4869-b85b-99cd8a7e588e.png">
  - crate-drop: 마지막에 테이블을 드랍한다. 따라서 테이블이 삭제된다.
  - validate: 엔티티와 테이블이 같은지 확인한다. 같지 않으면 에러를 출력한다.
    <img width="1273" alt="image" src="https://user-images.githubusercontent.com/49191949/162964631-466c6f84-526f-4121-9318-58d44d630f87.png">
  - none: 이 옵션을 주석처리한 것과 같은 효과( 그냥 주석처리하자)
- 데이터베이스 방언 별로 달라지는 것 확인하기(varchar)
  - Oracle로 변경. 오라클은 가변문자는 varchar2 타입이다.
    <img width="836" alt="image" src="https://user-images.githubusercontent.com/49191949/162965237-fdd18c00-0227-4703-ba01-94450d8c9810.png">
    <img width="317" alt="image" src="https://user-images.githubusercontent.com/49191949/162965616-59ac893b-4be9-4ae2-8a2e-4ede2f100a46.png">
  
### 데이터베이스 스키마 자동생성 - 주의할 점

- **운영 장비에서는 절대 create, create-drop, update를 사용하면 안된다.**
- 개발 초기 단계에서는 create 또는 update를 사용해서 로컬 서버로 쓰면된다.
- 테스트 서버(중간서버, 여러 개발자가 함께 사용하는 서버)는 update 또는 validate를 사용
- 스테이징과 운영 서버에서는 validate 또는 none을 사용
  - 운영 서버에서 alter를 잘못 사용하면 시스템이 중단 상태가 될 수 있다.
- 결론: 위의 3가지 값은 개인적으로 사용할 때만 사용하고 같이 사용하는 서버에서는 권장하지 않는다.


### DDL 생성 기능 추가 설명

- 제약조건 추가하기: 회원 이름은 필수, 10자 초과x
  - @Column(nullable = false, length = 10)
- 유니크 제약조건 추가하기
  - @Table(uniqueConstraints = {@UniqueConstraint( name = "NAME_AGE_UNIQUE", columnNames = {"NAME", "AGE"})})

<img width="785" alt="image" src="https://user-images.githubusercontent.com/49191949/162967855-5e00f0cd-6e60-4041-9009-38974f169af1.png">

- DDL 생성만 도와주고 JPA의 실행(런타임) 로직에 영향을 주지 않는다.

### 필드와 컬럼 매핑하기

### 요구사항 추가

1. 회원은 일반 회원과 관리자로 구분한다.
2. 회원 가입일과 수정일이 있어야 한다.
3. 회원을 설명할 수 있는 필드가 있어야 한다. 이 필드는 길이 제한이 없다.

<img width="601" alt="image" src="https://user-images.githubusercontent.com/49191949/162900313-a192f1f4-5f98-46d5-9294-0d5135054cdf.png">

<img width="944" alt="image" src="https://user-images.githubusercontent.com/49191949/162970062-9c030cc7-e8fb-4628-985e-f7186164c9bf.png">

- H2 DB에서
- Id는 bigint 생성
- Lob의 문자열 타입은 clob으로 생성
- enum은 varchar로 매핑

### 매핑 어노테이션 정리

- hibernate.hbm2ddl.auto

<img width="702" alt="image" src="https://user-images.githubusercontent.com/49191949/162900419-27801195-890b-4016-8e0d-6cd383252262.png">

- @Transient는 맵핑 안하고 싶을 때 사용한다.
  - DB랑 관계없이 사용하고 싶을 때, 메모리에서만 사용하고 싶을 때 

### @Column

<img width="703" alt="image" src="https://user-images.githubusercontent.com/49191949/162900478-5f1b045b-0eef-4fd8-b7a3-5f9766d2a087.png">

- 가장 많이 사용하고 가장 중요한 애노테이션 
- updatable=false로 하면 애플리케이션에서 컬럼이 절대 변경되지 않는다.
  - 물론 DB에서 직접적으로 접근하면 변경할 수 있다.
- nullable
  - 디폴트값은 true
  - false로 하면 null을 허용하지 않는다. (NOT NULL 제약조건)
    <img width="811" alt="image" src="https://user-images.githubusercontent.com/49191949/162972066-4200a91b-acf7-41b0-bb63-f4519c403376.png">
- unique
  - 유니크 제약조건을 걸고 싶으면 unique = true하면된다.
  - 거의 사용하지 않는다.
  - 제약조건을 만들어주긴 하는데 이름이 랜덤으로 생성되어 운영에서 사용하기 어렵다.
  - 운영에서는 이름을 보고 유니크 제약을 알아야 하기 때문이다.
  - @Table의 uniqueConstraint 속성을 이용하면 이름을 줄 수 있어 그나마 여기서 사용한다.
- 
### @Enumerated

- 자바 enum 타입을 매핑할 때 사용한다.
- 2가지 값이 있다.
  - ORIDINAL: enum 순서를 저장 -> integer
    <img width="619" alt="image" src="https://user-images.githubusercontent.com/49191949/162974441-9b1cfc5c-0679-4a3f-9b12-d5f9231f0a0c.png">
    <img width="535" alt="image" src="https://user-images.githubusercontent.com/49191949/162974704-b93b890e-ce89-45a1-a5a1-66ed35df17b2.png">
    - DB에 0(숫자)로 들어간다
  - STRING: enum 이름을 저장
    <img width="657" alt="image" src="https://user-images.githubusercontent.com/49191949/162975798-a0ebed7f-9aa1-4ccb-b8f3-4b1adc60199a.png">
    <img width="535" alt="image" src="https://user-images.githubusercontent.com/49191949/162975895-3ae3067e-d320-43d6-9310-b3dceaf1b30f.png">
  - 
- ** 주의! ORIDINAL(디폴트값)는 사용하지 않는다!!**
  - 왜?: 예를들어 enum의 순서가 변경되면 값이 바뀔 수 있다.
  - 운영상에서 굉장히 위험하다.
  - 그래서 꼭 String을 명시하자

<img width="702" alt="image" src="https://user-images.githubusercontent.com/49191949/162900644-03e1e9c5-30a8-43c0-b01c-ada834585b4a.png">


### @Temporal
- 자바 옛날버전만 필요하고 지금은 사실 필요없다.
  - 자바 8이후 LocalDate, LocalDateTime이 추가되었다.
- 자바는 Date 하나로 날짜시간을 모두 표현하지만 DB는 3가지로 구분해서 쓴다.그래서 매핑 정보를 줘야한다.
- 날짜 타입(java.util.Date, java.util.Calender)을 매핑할 때 사용한다.
- 참고: LocalDate, LocalDateTime을 사용할 때는 생략이 가능하다.(최신 하이버네이트)

<img width="701" alt="image" src="https://user-images.githubusercontent.com/49191949/162900894-bea2f633-226a-4d2a-87ac-7df1eb19739e.png">

<img width="961" alt="image" src="https://user-images.githubusercontent.com/49191949/162976940-8b59b65b-a10f-4a84-ab50-d9900be5bb0c.png">

- 하이버네이트 최신버전 쓰면 안써도 되는 애노테이션이다.

### @Lob

- DB에 varchar를 넘어서는 큰 것을 넣고 싶을 때 사용한다.
- 데이터베이스 BLOB, CLOB 타입과 매핑된다.
- @Lob에는 지정할 수 있는 속성이 없다.
- @매핑하는 필드 타입이 문자면 CLOB 매핑, 나머지는 BLOB으로 매핑한다.
  - CLOB: String, char[], java.sql.CLOB
  - BLOB: byte[],java.sql.BLOB


### @Transient

- 필드 매핑을 하지 않는다.
- 데이터베이스에 저장 및 조회하지 않는다.
- 주로 메모리상에서만 임시로 어떤 값을 보관하고 싶을 때 사용한다.

```java
@Transient
private Integer temp;
```

#### 필드와 컬럼 매핑은 어렵지 않다 연관관계 매핑이 어렵다.

### 기본키 매핑하기

### 기본키 매핑 애노테이션

- @Id
- @GeneratedValue

<img width="712" alt="image" src="https://user-images.githubusercontent.com/49191949/162901773-1bb0ccaf-62df-4b43-a9ce-9772e2aaac35.png">

### 기본키 매핑하는 방법

- 직접 할당하기: @Id**만** 사용한다.
  - 애플리케이션에서 셋팅해주는 
- 자동 생성(@GeneratedValue)하기: 
  - IDENTITY: 데이터베이스가 값을 셋팅한다. MySQL을 사용할 때 
    - 예) AUTO_INCREMENT
  - SEQUENCE: 데이터베이스 시퀀스 오브젝트 사용. ORACLE
    - @SequenceGenerator이 필요하다
  - TABLE: 키 생성용 테이블을 사용, 모든 DB에서 사용
    - @TableGenerator가 필요하다
  - AUTO:DB 방언에 따라 자동으로 지정해준다. 디폴트값이다.
    - 오라클이면 시퀀스
    - MYSQL이면 IDENTITY
  


### IDENTITY 전략 - 특징

- 기본키 생성을 데이터베이스에 맡깁니다.(=DB 너가 알아서 해줘)
- 주로 MySQL, PostgreSQL, SQL Server, DB2에서 사용합니다.
- 예) MySQL의 AUTO_INCREMENT
- id는 쿼리에서 null로 들어간다.
- JAP는 보통 트랙재션 커밋할 때  INSERT SQL을 실행합니다.
- AUTO_INCREMENT는 데이터베이스에 INSERT SQL을 실행한 이후에 ID 값을 알 수 있습니다.
- IDENTITY 전략은 em.persist() 시점에 즉시 INSERT SQL을 실행하고 DB에서 식별자를 조회합니다.
- H2
  <img width="509" alt="image" src="https://user-images.githubusercontent.com/49191949/162982482-2710da41-65bf-4924-ba4f-2b0833dabc7f.png">
- MySQL
  <img width="944" alt="image" src="https://user-images.githubusercontent.com/49191949/162982626-433f5204-ab48-4f87-b40b-4b2286d12944.png">
- DDL 옵션을 끄면 id가 자동으로 올라간다
  <img width="284" alt="image" src="https://user-images.githubusercontent.com/49191949/162983263-29e1d5f8-5f48-46f1-93e5-2b48089b3a81.png">
- 애플리케이션에서 id를 null 설정한 뒤에 INSERT 쿼리를 날리고 DB에서 값을 설정한다.
  - 그렇기 때문에 DB로 들어간 후에 ID 값을 조회할 수 있는 단점이 생긴다.
  - 영속성 컨텍스트에 관리를 받으려면 무조건 PK값이 필요하기 때문이다.
  - 그렇기에 IDENTITY 전략일 때만 em.persist()에 바로 insert 쿼리를 db에 날린다.
    <img width="1002" alt="image" src="https://user-images.githubusercontent.com/49191949/163130508-d895acb2-b891-4560-9009-ac9bdac6c1d3.png">
  - select가 출력되지 않는 이유는 JPA 내부적으로 insert 후 바로 값을 가져오는 상황을 처리하기 때문.
  - 그래서 쿼리를 모아서 보내는 전략이 IDENTITY에서는 불가능하다.
### IDENTITY 전략 - 기본키 매핑

<img width="629" alt="image" src="https://user-images.githubusercontent.com/49191949/162908399-2127c824-9432-47e7-b45c-880344f978f7.png">


### SEQUENCE 전략의 특징은?

- 데이터베이스 시퀀스는 유일한 값을 순서대로 생성하는 특별한 데이터베이스 오브젝트다.(예:오라클 시퀀스)
- 오라클, PostgreSQL, DB2, H2 에서 사용한다.

### 시퀀스 전략 - 매핑하는 법

<img width="703" alt="image" src="https://user-images.githubusercontent.com/49191949/162909564-4ab8d69c-0d82-41a3-8963-d4ab080b1ee0.png">

- int: null 표현이 안되서 탈락
- Integer: 10억이 넘으면 돌아가서 탈락
- Long을 사용한다.
  - Long은 int의 용량 2배이지만 지금은 거의 성능상 영향을 주지 않는다.
  - 애플리케이션 전체 성능에서 볼때 

<img width="926" alt="image" src="https://user-images.githubusercontent.com/49191949/162985259-e01ac92c-caf6-4fa8-8541-d2237a237359.png">

- 시퀀스에서 값을 가져와서 sql에 셋팅한 다음에 db로 쿼리가 날라간다.
- 이름을 안주면 하이버네이트 시퀀스를 사용한다.

<img width="261" alt="image" src="https://user-images.githubusercontent.com/49191949/162985811-8079b88a-f5fb-41ad-b1db-c2e8690c1357.png">

- @TableGenerator과 generator 속성을 사용하여 시퀀스를 지정할 수 있다.

<img width="1285" alt="image" src="https://user-images.githubusercontent.com/49191949/162987737-c9bba05f-e7df-4158-85db-2340c14e0031.png">

<img width="331" alt="image" src="https://user-images.githubusercontent.com/49191949/163168529-8d42eff2-f80d-4aba-89c6-5f03854e3857.png">

- DB 안 시퀀스 오브젝트에서 값이 있어 애플리케이션으로 가져와야 한다.
  - em.persist()를 하기 전 항상 pk값이 있어야 영속성 컨텍스트에 넣을 수 있다.
  - JPA는 시퀀스 전략이면 persist 이전에 pk값을 가져오도록 처리한다.
  - call next value for MEMBER_SEQ
    - DB한테 MEMBER_SEQ 다음 값 내놔라라고 명령한 것
  - 시퀀스 전략은 commit할 때 쿼리를 날린다. 즉 버퍼링 기능 가능
  - 고민
    - em.persist를 사용할때마다 next call을 불러 네트워크를 사용하니깐 성능에 별로 안좋을거 같은데?
    - 이것을 해결하기 위해 allocationSize을 사용하면 db에 미리 50개(디폴트)를 올려놓고 메모리에 채우는 방식
      <img width="851" alt="image" src="https://user-images.githubusercontent.com/49191949/163171172-d331a82a-8f2e-43a8-b509-4028d0259a98.png">

    - 1부터 시작해서 50개씩 늘리겠다.
      <img width="885" alt="image" src="https://user-images.githubusercontent.com/49191949/163171984-6ef331e0-e46c-4851-9a95-066357abefe3.png">
    - 

### 시퀀스 - @SequenceGenerator

- 주의: allocationSize 디폴트 값은 50이다.

<img width="700" alt="image" src="https://user-images.githubusercontent.com/49191949/162909731-98325554-8223-4bc9-ad57-681dbb39b475.png">


### 시퀀스 전략과 최적화

- initialValue와 allocationSize로 성능 최적화를 할 수 있다.

### TABLE 전략

- 키 생성 전용 테이블을 만들어서 데이터베이스 시퀀스를 흉내내는 전략
- 장점: 모든 DB에서 적용가능
  - 어떤 db는 AUTO_INCREMENT, 어떤 DB는 시퀀스.... 고민할 필요가 없다.
- 단점: 성능이 꾸리다.
- 사실 잘 사용하지는 않는다.

### TABLE 전략 - 매핑 예시

<img width="456" alt="image" src="https://user-images.githubusercontent.com/49191949/162910755-77d63336-b9bf-4eb3-a0c5-51f95ba42374.png">



<img width="642" alt="image" src="https://user-images.githubusercontent.com/49191949/162910814-0deb6034-6298-4ee7-940b-18bdff12fdec.png">

<img width="519" alt="image" src="https://user-images.githubusercontent.com/49191949/163127356-85c17e3d-10a6-46bc-a348-b0927262aa61.png">

<img width="452" alt="image" src="https://user-images.githubusercontent.com/49191949/163127433-34b69e33-46f0-4edc-b8e4-854f2084cc4b.png">

- 이 테이블을 이용하여 Member의 id 값이 할당된다.
- 운영 서버에서는 사용하지 않는다. 
  - 보통  DB에서 관례적으로 사용하는 것이 있다.
### @TableGenerator - 속성

<img width="708" alt="image" src="https://user-images.githubusercontent.com/49191949/162910912-4eb508c9-926b-4372-9a38-22cf8099f0c4.png">

- initailValue와 allocationSize로 성능 최적화가 가능하다.

### 여러가지 식별자 전략 중 무엇을 사용해야 할까?

- 기본키는 1.null이 아니고, 2.유일하고, 3.변하면 안된다(기본키 제약조건)
  - 변하면 안된다라는 것이 지키기 어렵다. 
  - 먼 미래까지(5년~10년까지) 바뀌면 안된다.
- 미래까지 이 제약조건을 지키는 자연키(비즈니스적으로 의미있는키, 주민번호, 전화번호)는 찾기 어렵다. 그러므로 대리키(대체키)를 사용하자
- 예를들어, 주민번호도 기본키로 적절하지 않다.
- 권장하는 방법: Long + 대체키 + 키 생성전략
  - 10억넘어도 Long은 동작
  - 즉, AUTO_INCREMENT나 시퀀스 오브젝트 둘 중에 하나를 사용해라!



### 실전 예제 - 1. 요구사항 분석과 기본 매핑하기

### 요구사항 분석

- 회원은 상품을 주문할 수 있다.
- 주문 시 여러 종류의 상품을 선택할 수 있다.


### 기능 목록

<img width="356" alt="image" src="https://user-images.githubusercontent.com/49191949/162911529-745350e6-7cfa-43fb-ab47-2e92cdfe3fb8.png">


- 회원 기능
  - 등록
  - 조회
- 상품 기능
  - 등록
  - 수정
  - 조회
- 주문 기능
  - 상품주문
  - 주문내역 조회
  - 취소

### 도메인 모델을 분석해보자

<img width="646" alt="image" src="https://user-images.githubusercontent.com/49191949/162911766-0cda503c-925a-457d-8d1f-66188f4b1aa6.png">


- 회원과 주문의 관계
  - 회원은 여러 번 주문할 수 있다.(1:N)
- 주문과 상품의 관계
  - 주문할 때 여러 상품을 선택할 수 있다.
  - 반대로 같은 상품도 여러 번 주문될 수 있다.
  - 주문상품이라는 모델을 만들어서 N:N 관계를 1:N, N:1 관계로 만든다


### 테이블 설계

<img width="646" alt="image" src="https://user-images.githubusercontent.com/49191949/162912149-aa5877d2-df04-4309-9aa1-71de6ad02de6.png">


### 엔티티 설계와 매핑

<img width="682" alt="image" src="https://user-images.githubusercontent.com/49191949/162912227-d7a711fd-c49b-4380-ac5e-27e171942df7.png">
- 테이블 설계를 보고 엔티티를 설계하면 처음에는 이렇게 테이블과 똑같이 설계한다.(SQL 중심개발)

### 실습

#### 엔티티 작성하기

#### 제대로 되는지 실행해보기
- h2 서버 실행하기
- jpashop 데이터베이스 만들기
- 연결하기
- JpaMain 메인함수 실행하기

- 스프링부트 없이 순수하게 jpa로 실행하면 필드명이 memberId면 칼렴명도 memberId로 된다
  - 실무에서는 스프링부트와 결합되는데 그때는 스프링부트가 관례를 설정할 수 있다. 예를들면 member_id, MEMBER_ID로
  - 강사가 선호하는 방식은 스프링부트와 같은 방식이다.
- 애매하면 직접 칼럼명을 작성해주면 된다.

- (255) 실으면 Column(length = )로 지정가능 아니면 개발에서 그대로 사용하고 운영에서 스크립트를 바꿔도 된다.
  - 가급적이면 제약/매핑도 개발단계에서 다 추가하는걸 추천한다.
  - 인덱스도 가급적이면 개발 환경에 넣는다.그래야 개발자들이 객체보고 jpql 쿼리를 정확하게 작성할 수있다.

### 문제점

<img width="883" alt="image" src="https://user-images.githubusercontent.com/49191949/163193339-bffa322b-76a9-431d-9bee-d90c599443c8.png">

- 특정 주문과 관련된 회원을 조회하려니 바로 얻을 수 없고 회원 id를 얻은 후 다시 db에서 조회해야 한다.
- 비 객체지향적
- 그래서 테이블 설계를 보고 엔티티를 똑같이 만들면 객체지향적인 설계를 할 수 없다.
- 이러한 설계를 관계형 DB에 맞춘 설계라한다.

<img width="435" alt="image" src="https://user-images.githubusercontent.com/49191949/163193830-852f80e4-1c39-4247-9dd4-4aeae97c4bf7.png">

- 이렇게 바로 접근할 수 있어야 객체지향적이다.
- 참조로 바로 찾기



### 위와 같은 데이터 중심 설계의 문제점

- 객체 설계가 아닌 테이블에 맞춘 설계방식
- 테이블의 외래키를 객채에 그대로 가져옴
- 객체 그래프 탐색이 불가능해진다.
  - order.getMember() 불가능..
- 참조가 없으므로 UML도 잘못됬다.
- 이것을 해결하려면 **연관관계 매핑**을 해야한다