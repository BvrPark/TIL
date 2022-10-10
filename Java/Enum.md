> 스프링과 JPA를 공부하다 보니 Enum의 개념이 나와서 알아보기 쉽게 정리를 해보았다.
# Enum(열거 타입)
- Enumeration의 앞 글자.
- 자바에서는 final로 기본 자료형(문자열, 숫자 등)의 값을 고정할 수 있음</br> 
-> 이렇게 고정한 값을 **상수**라고 한다. 
- 어떤 클래스가 상수로만 작성되어 있을 시, 반드시 class를 선언할 필요 ❌</br>
-> 이때, class로 선언된 부분에 `enum`이라고 대신 선언하면</br>
이 객체는 상수의 집합이라고 명시적으로 나타내는 뜻이 된다.

- ex) 원래 코드
```java
class MVC{
    public final static MVC Model = new MVC();
    public final static MVC View = new MVC();
    public final static MVC Controller = new MVC();
}
```
</br>
- ex) Enum 타입 코드

```java
enum MVC{
    MODEL, VIEW, CONTROLLER;
}
```
Enum 타입에서는 상수들을 다 대문자로 써야한다.(Model -> MODEL)</br>
Enum 타입에서 생성자는 무조건 private로 설정해야 한다.

