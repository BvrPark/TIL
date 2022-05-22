# MVC패턴

MVC는 **M**odel **V**iew **C**ontroller의 약자로 애플리케이션을 3가지 역할로 구분한 개발 방법론.
</br></br></br>

## Model

데이터를 가진 객체를 Model이라고 지칭.</br>
데이터베이스, 초기 상수, 변수 등과 이런 데이터와 정보들의 가공을 담당하는 컴포넌트.

**`Model`의 규칙**
- 사용자가 편집하길 원하는 모든 데이터를 가지고 있어야만 함.
- View나 Controller에 대해서 어떠한 정보도 알지 말아야 함.
- 변경이 일어나면, 변경 통지에 대한 처리방법을 구현해야 함.

## View
View는 사용자 인터페이스 요소를 나타내며, 데이터 및 객체의 입력과 보여주는 출력을 담당.</br>
데이터를 기반으로 사용자들이 볼 수 있는 화면.

**`View`의 규칙**
- Model이 가지고 있는 정보를 따로 저장해서는 안됨.
- Model이나 Controller와 같이 다른 구성요소를 몰라야 함.
- 변경이 일어나면, 변경 통지에 대한 처리방법을 구현해야 함.

## Controller
사용자가 접근한 URL에 따라 사용자의 요청사항을 파악한 후, 그 요청에 맞는 데이터를 Model에게 의뢰하고, 데이터를 View에 반영해서 사용자에게 알려줌.</br>

- Model에 명령을 보내서 View의 상태를 변경할 수 있음</br>
ex) 워드에서 문서 편집</br>
- Controller가 관련된 Model에 명령을 보내서 View의 표시 방법을 바꿀 수 있음</br>
ex) 문서를 스크롤하는 것

**`Controller`의 규칙**
- Model이나 View에 대해서 알고 있어야 함.
- Model이나 View의 변경을 모니터링해야 함.
</br></br></br>

## MVC패턴의 작동방식

![MVC패턴](https://user-images.githubusercontent.com/84119178/169681118-809e7a2d-021d-4cf8-9958-4a69287ce912.png)</br>
위 그림과 같이 작동을 하며 MVC패턴을 잘 이용하면, 사용자 인터페이스로부터 비즈니스 로직을 분리하여 애플리케이션의 시작적요소나 비즈니스 로직을 서로 영향없이 쉽게 고칠 수 있음.
</br></br>

**예시**
1. 사용자가 웹사이트에 접속(USER)
2. Controller는 사용자가 요청한 웹페이지를 서비스하기 위해서 모델을 호출(MANIPULATES)
3. Model은 데이터베이스나 파일과 같은 데이터 소스를 제어한 후 그 결과를 Return
4. Controller는 Model이 리턴한 결과를 View에 반영(UPDATES)
5. 데이터가 반영된 View는 사용자에게 보여짐(SEES)
</br></br></br>

## MVC패턴 방식
MVC패턴에는 2가지 방식이 있다.
- Model 1 방식 : JSP에서 출력과 로직을 전부 처리
- Model 2 방식 : JSP에서 출력만 처리

### **Model 1 방식**
![Model1방식](https://user-images.githubusercontent.com/84119178/169682585-c0c21bd3-53e7-4402-ae2a-105126070a4a.png)</br>
Model 1 방식은 Controller영역에 View영역을 같이 구현하는 방식이며, 사용자의 요청을 JSP가 전부 처리.</br>
요청을 받은 JSP는 Java Bean Service Class를 사용하여 웹브라우저 사용자가 요청한 작업을 처리하고 그 결과를 출력.
</br></br>

### **Model 2 방식**
![Model2방식](https://user-images.githubusercontent.com/84119178/169682824-2de41c78-d5cd-4761-a5a5-95599e0f531d.png)</br>
Model 2 방식은 웹브라우저 사용자의 요청을 서블릿이 받고 서블릿은 해당요청으로 View로 보여줄 것인지 Model로 보낼 것인지 판단하여 전송.</br>
Model 2 방식의 경우 HTML소스와 JAVA소스를 분리해놓았기 때문에 Model 1 방식에 비해 확장시키기도 쉽고 유지보수 또한 쉽다.
</br></br></br>


## MVC 패턴의 장단점

### 장점
- 비즈니스 로직과 UI로직을 분리하여 유지보수를 독립적으로 수행가능.
- Model과 View가 다른 컴포넌트들에 종속되지 않아 애플리케이션의 확장성, 유연성에 유리.
- 중복 코딩의 문제점 제거

### 단점
- Controller에 다수의 Model과 View가 복잡하게 연결되어 있는 상황이 발생할 수도 있다.

</br></br>


<h2>참고</h2>

[https://cocoon1787.tistory.com/704](https://cocoon1787.tistory.com/704)
