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
