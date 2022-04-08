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


