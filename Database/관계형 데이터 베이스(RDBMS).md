# 관계형 데이터 베이스(RDBMS)</br>`R`elational `D`ata`B`ase `M`anagement `S`ystem</br>

테이블을 사용하여 데이터를 저장,관리하는 데이터베이스 관리 시스템

**특징**
- 데이터의 분류, 정렬, 탐색 속도가 빠름.
- 신뢰성이 높고, 어떤 상황에서도 데이터의 무결성을 보장.
- 기존에 작성된 스키마를 수정하기가 어려움.
- 데이터베이스의 부하를 분석하는 것이 어려움.

**✔ 스키마란?**
- 간단히 설명하면 테이블을 디자인하기 위한 청사진이다.
- 자세한 설명은 [링크]()

</br></br>

## 테이블의 구조

<img src = "https://user-images.githubusercontent.com/84119178/196906590-bb0522d8-2752-46f7-9905-9b20c1bd7a4d.png" width = "600">

### 테이블(table)
위 그림에서 <span style = "color:red">빨간 테두리</span> 부분
- 데이터 베이스안에 실제 데이터가 저장되는 형태
- 사전에 정의된 열의 데이터 타입대로 작성된 데이터가 행으로 축적

### 레코드(record || tuple || row)
위 그림에서 <span style = "color:blue">파란 테두리</span> 부분
- 테이블에서 가로줄
- 관계된 데이터의 묶음을 의미
- 저장하려는 하나의 객체를 구성하는 여러 값

### 열(column || field || attribute)
위 그림에서 <span style = "color:green">초록 테두리</span> 부분
- 테이블에서 세로줄
- 각각의 열은 유일한 이름을 가지고, 각각 자신만의 타입을 가짐.
- 즉, 저장하려는 데이터를 대표하는 이름을 가지고,</br> 
그 데이터들의 공통 특성도 있음.

### 값(value)
- 테이블은 각각의 행과 열에 대응하는 값을 가짐.
- 값은 열의 타입(공통 특성)에 맞아야 된다.

### 키(key)
- 하나의 테이블을 구성하는 열(column) 중에 특별한 의미를 가진 열(column)
- **기본 키(PK : Primary Key)**
    - 키 중 가장 중요한 키
    - PK에 저장된 데이터들은 중복되지 않는 유일한 값만 가짐.
    - NULL을 가질 수 없다.

- **외래 키(FK : Foreign Key)**
    - 다른 테이블에서 PK로 지정된 키

</br></br>

## 연관 관계 종류
- **1 : 1 관계**
- **1 : N 관계**
- **N : M 관계**

</br>

### 1 : 1 관계

<img src = "https://user-images.githubusercontent.com/84119178/197564432-a05c395a-2a12-467d-9eb8-17ce0a22c9c8.png">

- 하나의 레코드가 다른 테이블의 레코드 한 개와 연결된 경우이다.
- 다음과 같이 Users 테이블과 Phonebook 테이블이 있다고 가정한다.
- Users 테이블은 ID, name, phone_id를 가지고 있다.
- 이 중 phone_id는 외래키(foreign key)로써, Phonebook 테이블의 ID 와 연결되어 있다.
- Phonebook 테이블은 ID와 phone_number를 가지고 있다.
- 각 전화번호가 단 한 명의 유저와 연결되어 있고, 그 반대도 동일하다면, Users 테이블과 Phonebook 테이블은 1:1 관계(One-to-one relationship)이다.
- 그러나 1:1 관계는 자주 사용하지 않는다.
- 1:1로 나타낼 수 있는 관계라면 Users 테이블에 phone_id를 대신해 phone_number를 직접 저장하는 게 나을 수 있다.

</br>

### 1 : N 관계

<img src = "https://user-images.githubusercontent.com/84119178/197564969-be6cbb55-bf5e-4d8f-9d4d-b88ebbc78e55.png">

- 하나의 레코드가 서로 다른 여러 개의 레코드와 연결된 경우이다.
- Users 테이블과 Phonebook 테이블의 관계를 다음과 같이 가정한다.
- 이 구조에서는 한 명의 유저가 여러 전화번호를 가질 수 있다.
- 그러나 여러명의 유저가 하나의 전화번호를 가질 수는 없다.
- 이런 1:N(일대다) 관계는 관계형 데이터베이스에서 가장 많이 사용한다.

</br>

### N : M 관계

- 여러 개의 레코드가 다른 테이블의 여러 개의 레코드와 관계가 있는 경우이다.
- N:M(다대다) 관계를 위해 스키마를 디자인할 때에는, Join 테이블을 만들어 관리한다.
- 1:N(일대다) 관계와 비슷하지만, 양방향에서 다수의 레코드를 가질 수 있다.
- 다음과 같이 여행 상품을 관리하는 테이블이 있다고 가정한다.

<img src = "https://user-images.githubusercontent.com/84119178/197565118-0fcb3b58-a9b3-4eb4-a7d9-253dfcca5614.png">

- 여러 개의 여행 상품이 있고, 여러 명의 고객이 있다.
- 고객 한 명은 여러 개의 여행 상품을 구매할 수 있고, 여행 상품 하나는 여러 명의 고객이 구매할 수 있다.
- 이렇게 Customer 테이블과 Package table이 따로 존재한다면, N:M(다대다) 관계를 어떻게 표현할 수 있을까?
- 다대다 관계는 두 개의 일대다 관계와 그 모양이 같다.
- 두 개의 테이블과 1:N(일대다) 관계를 형성하는 새로운 테이블로 N:M(다대다) 관계를 나타낼 수 있다.
- 이렇게 다대다 관계를 위한 테이블을 조인 테이블이라고 한다.
- N:M(다대다) 관계를 그림으로 나타내면 다음과 같다.

<img src="https://user-images.githubusercontent.com/84119178/197566081-c9d1554b-6b68-44cf-93c1-ce4d7e4b411b.png">

</br></br></br></br></br>


## 참조
[hanamon.kr](https://hanamon.kr/%ea%b4%80%ea%b3%84%ed%98%95-%eb%8d%b0%ec%9d%b4%ed%84%b0%eb%b2%a0%ec%9d%b4%ec%8a%a4-%ec%84%a4%ea%b3%84-%ea%b4%80%ea%b3%84-%ec%a2%85%eb%a5%98/)
