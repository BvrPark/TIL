> 이 글은 김영한님의 [자바 ORM 표준 JPA 프로그래밍](https://www.inflearn.com/course/ORM-JPA-Basic/dashboard) 강의를 듣고 필요한 부분만 요약한 것입니다.

# JPQL 문법 정리
JPQL에 익숙해 지기 전까진 정리를 해두고 쓸 때 참고를 하는 것이 좋을 것 같아서 따로 정리를 한다.

## JPQL 특징
- JPQL은 **객체지향 쿼리언어**이므로 테이블 대신 **엔티티 객체를 대상으로 쿼리**한다.
- JPQL은 SQL을 추상화하기 때문에, 특정 DB SQL에 의존하지 않는다.</br> -> **결국 SQL로 변환**

## JPQL 문법 목차
- [JPQL 기본 문법](#jpql-기본-문법)
- [집합과 정렬](#집합과-정렬)
- [TypedQuery, Query](#typedquery-query)
- [결과 조회 API](#결과-조회-api)
- [파라미터 바인딩](#파라미터-바인딩)
- [프로젝션](#프로젝션)
- [페이징 API](#페이징-api)
- [JOIN](#join)
- [서브 쿼리](#서브-쿼리)
- [조건식](#조건식---case-식)
- [사용자 정의 함수](#사용자-정의-함수-호출-방법)

</br></br>
 
## JPQL 기본 문법
- 일반적인 select, from, where, groupby 등등 SQL과 문법은 거의 비슷하다.
- ### 예시)
    ```sql
    select m from Member as m where m.age > 18
    ```
    - 엔티티(**Member**)와 속성(**m.age**)은 대소문자를 구분한다.
    - JPQL 키워드(**select, FROM, where**)는 대소문자 구분을 하지 않는다.
    - 테이블 이름이 아닌 **엔티티 이름을 사용**한다.
    - **별칭(m)은 필수!!!**(as는 생략가능하다.)

</br>

## 집합과 정렬
```sql
select
    COUNT(m),   --> 회원수
    SUM(m.age), --> 나이 합
    AVG(m.age), --> 평균 나이
    MAX(m.age), --> 최대 나이
    MIN(m.age)  --> 최소 나이
from Member m
```
- **GROUP BY, HAVING, ORDER BY**도 위와 똑같이 사용하면 된다.

</br>

## TypedQuery, Query
- **`TypedQuery`** : 반환 타입이 명확할 때 사용
- **`Query`** : 반환 타입이 명확하지 않을 때 사용
```sql
TypedQuery<String> query1 
= em.createQuery("select m.username from Member m", String.class);
--> m.username은 반환 타입이 String이므로 TypedQuery 사용

Query query2 
= em.createQuery("select m.username, m.age from Member m", jpql.Member.class);
--> m.username, m.age로 반환 타입이 String, int이므로 반환타입이 명확하지 않으므로 Query 사용
```

</br>

## 결과 조회 API
- **`query.getResultList()`** : **결과가 하나 이상일 때**, 리스트 반환
    - 결과가 없으면 빈 리스트 반환</br>
    -> NullPointException이 안일어남!
- **`query.getSingleResult()`** : 결과가 **정확히 하나**, 단일 객체 반환
    - 결과가 없으면 : `javax.persistence.NoResultException`
    - 둘 이상이면 : `javax.persistence.NonUniqueResultException`</br>
    -> 결과가 정확히 하나가 아니면 무조건 Exception 반환!

</br>

## 파라미터 바인딩
- 이름 기준과 위치 기준이 있으나 위치 기준은 데이터의 변화에 따라 오류가 생길 수 있으므로 이름 기준만 다룬다.

- **이름 기준 파라미터 바인딩**
    ```sql
    Member result 
    = em.createQuery("select m from Member m where m.username = :username", Member.class)
                .setParameter("username", "member1")  --> :뒤의 ?("username")이 member1인 것을 찾을꺼다.
                .getSingleResult();
    --> 결과를 정확히 하나 반환한다.
    ```

</br>

## 프로젝션
- SELECT 절에 조회할 대상을 지정하는 것
- 프로젝션 대상은 엔티티, 임베디드 타입, 스칼라 타입 등이 있다.
- ### EX)
    ```sql
    SELECT m FROM Member m --> 엔티티 프로젝션(m)
    SELECT m.team FROM Member m --> 엔티티 프로젝션(m.team)
    SELECT m.address FROM Member m --> 임베디드 타입 프로젝션(m.address)
    SELECT m.username, m.age FROM Member m --> 스칼라 타입 프로젝션(m.username, m.age)
    SELECT 뒤에 DISTINCT로 중복 제거 가능
    --> 엔티티 타입을 제외하고는 join쿼리가 안 나간다.
    ```
- ### 프로젝션 - 여러 값 조회(필드가 다를 때)
    - ```sql
        SELECT m.username, m.age FROM Member m
        ```
    - 3가지 방법으로 조회를 할 수 있지만 **new 명령어로 조회**하는게 제일 깔끔하다.
    - ### new 명령어로 조회
        - MemberDTO클래스를 생성한 뒤, 생성자를 만든 후
        ```java
        em.createQuery("select new jpql.MemberDTO(m.username, m.age) from Member m", MemberDTO.class)
        // new뒤에 클래스 이름은 패키지 명을 포함한 전체 클래스 이름을 입력한다.
        ```

</br>

## 페이징 API
- JPA는 페이징을 2가지 API만 쓰면 된다.
- **setFirstResult**(int startPosition) : 조회 시작 위치(0부터 시작)
- **setMaxResults**(int maxResult) : 조회할 데이터 수

</br>

## JOIN
- 내부조인(둘 중 하나 null이면 null값이 생성)
    ```sql
    SELECT m FROM Member m (INNER)JOIN m.team t
    --> Member의 PK와 연결되어 있는 Team의 FK를 내부 조인
    --> INNER 생략 가능
    ```
- 외부조인(LEFT의 우측에 있는 값은 null가져도 됨)
    ```sql
    SELECT m FROM Member m LEFT(OUTER) JOIN m.team t
    --> Member의 PK와 연결되어 있는 Team의 FK를 외부 조인
    --> OUTER 생략 가능
    ```
- 세타조인(크로스 조인과 비슷하지만 같은 것은 아님)
    ```sql
    SELECT count(m) FROM Member m, Team t where m.username = t.name
    --> Member와 Team을 다 불러온 뒤, Member의 username과 Team의 name이 같은 값을 모두 count
    --> 서로 연관이 없는 엔티티들 끼리도 조인이 가능
    --> query는 cross join이 나간다.
    ```
- ### JOIN - ON을 사용하는 경우
    - **join 대상을 필터링하고 싶은 경우**
        - EX) 회원과 팀을 조인하면서, 팀 이름이 A인 팀만 조인
            ```sql
            SELECT m,t FROM Member m LEFT JOIN m.team t on t.name = 'A'
            ```
    - **연관관계 없는 엔티티 외부와 조인하고 싶은 경우**
        - EX) 회원 이름과 팀의 이름이 같은 대상을 외부 조인
            ```sql
            SELECT m,t FROM Member m LEFT JOIN Team t on m.username = t.name
            --> FK로 연결하지 않은 대상을 JOIN할 때도 쓸 수 있고
            --> 전혀 연관관계가 없는 외부 대상과 조인도 가능하다.
            ```

</br>

## 서브 쿼리
일반적으로 메인 쿼리랑 연관이 없을 수록 성능이 좋다.</br>
EX1) 나이가 평균보다 많은 회원
```sql
SELECT m FROM Member m
WHERE m.age > (SELECT avg(m2.age) FROM Member m2)
```

EX2) 한 건이라도 주문한 고객
```sql
SELECT m FROM Member m
WHERE (SELECT COUNT(o) FROM Order o WHERE m = o.member)>0
```
- ### 서브 쿼리 지원 함수
    - EXISTS(subquery) : 서브 쿼리에 결과가 존재하면 참
    - ALL(subquery) : 모두 만족하면 참
    - ANY, SOME(subquery) : 조건을 하나라도 만족하면 참
    - IN(subquery) : 서브 쿼리의 결과 중 하나라도 같은 것이 있으면 참
    - **EX)**
        ```sql
        SELECT m FROM Member m
        WHERE EXISTS(SELECT t FROM m.team t where t.name = '팀A')
        --> 팀A 소속인 회원

        SELECT o FROM Order o
        WHERE o.orderAmount > ALL(SELECT p.stockAmount FROM Product p)
        --> 전체 상품 각각의 재고보다 주문량이 많은 주문들

        SELECT m FROM Member m
        WHERE m.team = ANY(SELECT t FROM Team t)
        --> 어떤 팀이든 팀에 소속된 회원
        ```
    - **서브 쿼리의 한계**
        - JPA는 WHERE, HAVING, SELECT절(하이버네이트)에서 서브 쿼리 사용 가능
        - **FROM 절의 서브 쿼리는 현재 JPQL에서 불가능**
            - 대부분 조인으로 풀어서 해결
            - 그래도 안되면 쿼리를 나눠서 해결
            - 그래도 안되면 애플리케이션 조작을 통해 해결

</br>

## 조건식 - CASE 식
```sql
SELECT
    case when m.age <= 10 then '학생요금'
    case when m.age >= 60 then '경로요금'
         else '일반요금'
    end
FROM Member m
--> 이런식으로 표현하면 된다.

SELECT COALESCE(m.username, '이름 없는 회원') FROM Member m
--> 사용자 이름이 없으면 '이름 없는 회원'을 반환
```
### 사용자 정의 함수 호출 방법
- 사용전 Dialect에 추가해야 된다.
- 사용하는 DB방언을 상속받고 사용하고 싶은 함수를 등록하면 된다.
    - 따로 Dialect 클래스를 생성해서 자신이 쓰는 SQL Dialect를 상속받은 후, 사용한다.
    - 사용법은 extends 받은 인터페이스를 참고해서 함수를 등록하면 된다.
    - EX)
        ```sql
        SELECT function('함수 이름', i.name) FROM Item i
        --> 만약 하이버네이트를 사용한다면

        SELECT 함수 이름(i.name) FROM Item i
        --> 이렇게 Java 메서드처럼 사용해도 된다.
        ```
