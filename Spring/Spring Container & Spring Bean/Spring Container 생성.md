> 이 글은 김영한님의 [스프링 핵심 원리- 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8/dashboard) 강의의 일부분에서 요약한 것입니다.

# 스프링 컨테이너 생성

## 스프링 컨테이너 생성과정
```Java
ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
```
- `ApplicationContext`를 스프링 컨테이너라 부르고 인터페이스이다.

- 스프링 컨테이너는 XML을 기반으로 만들 수 있고,</br>
애노테이션 기반의 자바 설정 클래스로 만들 수 있다.
- 이전의 [SpringContainer 기본](https://github.com/crupy/TIL/blob/master/Spring/Spring%20Container%20%26%20Spring%20Bean/SpringContainer%20%EA%B8%B0%EB%B3%B8.md)에서 AppConfig를 사용했던 방식이 애노테이션 기반의 자바 설정 클래스로 스프링 컨테이너를 만든 것이다.
- `new AnnotationConfigApplicationContext(AppConfig.class);`는</br>
`ApplicationContext` 인터페이스의 구현체이다.
</br></br>

### 1. 스프링 컨테이너 생성
![컨테이너 생성](https://user-images.githubusercontent.com/84119178/154633478-a6aadf8d-29f4-462a-9edf-39f0b369eade.png)
- 스프링 컨테이너를 생성할 때는 구성 정보를 지정해주어야 한다.
- 여기서는 `AppConfig.class`를 구성 정보로 지정
</br></br>

![스프링빈 등록](https://user-images.githubusercontent.com/84119178/154634553-1762c6e4-c474-4687-a587-dcaed03e0806.png)
### 스프링 빈 이름
- 스프링 빈 이름은 일반적으로 메서드 이름을 사용한다.
- 스프링 빈 이름을 직접 부여할 수도 있다.
    - `@Bean(name="memberService2")`
</br></br>

### 3. 스프링 빈 의존관계 설정
![의존관계 설정](https://user-images.githubusercontent.com/84119178/154635042-d4700b2c-468c-46db-9433-74d327b094b5.png)
- 스프링 컨테이너는 설정 정보를 참고해서 의존관계를 주입(DI)한다.
- 스프링은 빈을 생성하고, 의존관계를 주입하는 단계가 나누어져 있다.</br>
그런데 이렇게 자바 코드로 스프링 빈을 등록하면 생성자를 호출하면서 의존관계 주입(DI)도 한번에 처리된다.
