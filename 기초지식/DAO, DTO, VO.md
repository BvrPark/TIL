# DAO, DTO, VO

## DAO(Data Access Object)
- DAO는 DB의 data에 접근하기 위한 객체이다. 
- 주로, DB에 접근하기 위한 로직과 비지니스 로직을 분리하기 위해 사용한다.
- 직접 DB에 접근하여 data를 CRUD할 수 있는 기능을 수행한다.
- **JPA**에서는 `Repository` 객체들, **MVC 패턴**에서는 `Model`이 `DAO`라고 볼 수 있다.

</br>

## DTO(Data Transfer Object)
- DTO는 계층 간 데이터 교환을 위한 객체를 의미한다.
- DTO는 로직을 가지지 않는 데이터 객체이다.(Getter/Setter만 가진 클래스)
- 일반적으로 DB에서 꺼낸 값을 DTO에서 임의로 조작할 필요가 없다.</br> 따라서, **DTO**에는 `Setter`를 만들 필요가 **없고** `생성자`에서 값을 할당한다.

</br>

## VO(Value Object)
- VO는 값 오브젝트로써 값을 위해 쓰인다. 
- Read-Only 특징(사용하는 도중에 변경 불가능하며 오직 읽기만 가능)을 가진다. 

</br>

## DTO vs VO
- DTO와 유사하지만 VO는 getter 기능만 존재한다.
- DTO는 인스턴스 개념이고 VO는 리터럴 값 개념이다.
- **`VO`** 는 데이터들에 대해 Read-Only를 보장해줘야 신뢰성이 확보된다.</br>**-> 데이터 그 자체에 의미가 있다.**
- **`DTO`** 에게 데이터는 보존하고 전달되어야 되는 대상이다.</br>
**-> 데이터를 계층간 교환하는데 의미가 있다.**
