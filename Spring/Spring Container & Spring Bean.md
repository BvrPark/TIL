> 이 글은 김영한님의 [스프링 핵심 원리- 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8/dashboard) 강의의 일부분에서 요약한 것입니다.


# Spring Container & Spring Bean

## 스프링 컨테이너
```Java
public class AppConfig {

    public MemberService memberService(){
        return new MemberServiceImpl(memberRepository());
    }

    public OrderService orderService(){
        return new OrderServicImpl(memberRepository(), discountPolicy());
    }

    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }

    public DiscountPolicy discountPolicy(){
        return new RateDiscountPolicy();
    }
}
// 이 코드는 AppConfig를 사용해서 객체를 직접 사용하고 DI를 한 것
```
- 기존에는 개발자가 이렇게 AppConfig를 사용해서 직접 객체를 생성하고 DI를 했지만, 밑의 코드는 스프링 컨테이너를 통해서 사용한다.
</br></br></br>

```Java
@Configuration
public class AppConfig {

    @Bean
    public MemberService memberService(){
        return new MemberServiceImpl(memberRepository());
    }

    @Bean
    public OrderService orderService(){
        return new OrderServicImpl(memberRepository(), discountPolicy());
    }

    @Bean
    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }

    @Bean
    public DiscountPolicy discountPolicy(){
        return new RateDiscountPolicy();
    }
}
// 이 코드는 스프링 빈과 컨테이너를 활용한 것
```
- 스프링 컨테이너는 `@Configuration`을 붙은 AppConfig를 설정 정보로 사용한다.

- `@Bean`이라 적힌 메서드를 모두 호출해서 반환된 객체를 스프링 컨테이너에 등록한다.</br>
이렇게 스프링 컨테이너에 등록된 객체를 스프링 빈이라고 한다.

- 스프링 빈은 `@Bean`이 붙은 메서드 명을 스프링 빈의 이름으로 사용한다.</br>
ex) **memberService**, **orderService**

- 이전에는 개발자가 필요한 객체를 `AppConfig`를 사용해서 직접 조회했지만,</br>
지금부터는 스프링 컨테이너를 통해서 필요한 스프링 빈(객체)를 찾아야 한다.

```Java
// *다른 클래스
public static void main(String[] args) {

        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
        MemberService memberService = applicationContext.getBean("memberService", MemberService.class);
        OrderService orderService = applicationContext.getBean("orderService", OrderService.class);

// 이 코드는 스프링 컨테이너를 활용해서 객체를 찾는 것
// ApplicationContext를 스프링 컨테이너라고 한다.
```
- `ApplicationContext`를 스프링 컨테이너라고 한다.

- 스프링 빈은 `applicationContext.getBean("메서드 이름", 반환타입)`메서드를 사용해서 찾을 수 있다.

- 기존에는 개발자가 직접 자바코드로 모든 것을 했다면 지금부터는 스프링 컨테이너에 객체를 스프링 빈으로 등록하고, 스프링 컨테이너에서 스프링 빈을 찾아서 사용하도록 변경되었다.
</br></br></br></br></br>

## 스프링 컨테이너 생성과정
```Java
ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
```
- `ApplicationContext`를 스프링 컨테이너라 부르고 인터페이스이다.

- 스프링 컨테이너는 XML을 기반으로 만들 수 있고,</br>
애노테이션 기반의 자바 설정 클래스로 만들 수 있다.
- 위의 제일 첫 번째 코드에서 AppConfig를 사용했던 방식이 애노테이션 기반의 자바 설정 클래스로 스프링 컨테이너를 만든 것이다.
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
</br></br></br></br></br>

## 컨테이너에 등록된 빈 조회

### 스프링 컨테이너에 실제 스프링 빈들이 잘 등록되었는지 확인
```java
AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

@Test
@DisplayName("모든 빈 출력")
void 메서드(){
    String[] beanDefinitionNames = ac.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            Object bean = ac.getBean(beanDefinitionName);
            System.out.println("name = " + beanDefinitionName + "object = "+bean);
}
```
- 내가 등록한 빈 외에도 스프링 자체에 있는 빈들도 다 출력됨.
- `ac.getBeanDefinitionNames()` : 스프링에 등록된 모든 빈 이름을 조회
- `ac.getBean()` : 빈(Bean) 이름으로 빈(Bean) 객체를 조회
</br></br>

```java
@Test
    @DisplayName("애플리케이션 빈 출력하기")
    void 메서드(){
        String[] beanDefinitionNames = ac.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            BeanDefinition beanDefinition = ac.getBeanDefinition(beanDefinitionName);

            
            if(beanDefinition.getRole() ==BeanDefinition.ROLE_APPLICATION){
                Object bean = ac.getBean(beanDefinitionName);
                System.out.println("name = " + beanDefinitionName + "object = "+bean);
            }

        }
    }
```
- 내가 등록한 빈(Bean)들을 조회할 때
- `ac.getBeanDefinition()` : 빈(Bean)에 하나하나에 대한 메타 데이터 정보
- `if(beanDefinition.getRole() ==BeanDefinition.ROLE_APPLICATION)`
    - 내가 직접 등록한 빈
- `if(beanDefinition.getRole() ==BeanDefinition.ROLE_INFRASTRUCTURE)`
    - 스프링이 내부에서 사용하는 빈
</br></br>

```java
@Test
    @DisplayName("빈 이름으로 조회")
    void 메서드(){
        MemberService memberService = ac.getBean("memberService", MemberService.class);
        assertThat(memberService).isInstanceOf(MemberService.class);

    }
```
- 빈(Bean) 이름과 타입으로 조회를 하는 것
- `ac.getBean()`에서 이름 없이 타입으로만으로도 조회가 가능</br>
단, 같은 타입이 여러 개면 곤란해짐
- `assertThat(memberService).isInstanceOf(MemberService.class);`
    - 조회한 빈(Bean)의 객체가 MemberService.class가 맞는지 판별
</br></br>

```java
 @Test
    @DisplayName("빈 이름으로 조회X")
    void 메서드(){
        assertThrows(NoSuchBeanDefinitionException.class,
                ()-> ac.getBean("XXXX", MemberService.class));
    }
```
- **`assertThrows(NoSuchBeanDefinitionException.class,
                ()-> ac.getBean("XXXX", MemberService.class));`**
    - **`()-> ac.getBean("XXXX", MemberService.class)`**
    - 빈(Bean)이름에 없는 XXXX를 조회하는 `ac.getBean()`이 실행되었을 때,

    - **`NoSuchBeanDefinitionException.class`** : 이러한 형식의 예외가 나타나면

    - **`assertThrows()`** : 이것은 참이 된다.

- **`NoUniqueBeanDefinitionException.class`**
    - 이 예외는 타입으로 조회시 같은 타입이 둘 이상 있을 경우, 중복오류가 발생하는 예외이다.
    - 이 예외가 발생시, 빈(Bean) 이름과 타입을 지정해서 조회를 하면 된다.
</br></br>

```java
@Test
    @DisplayName("특정 타입을 모두 조회하기")
    void 메서드(){
        Map<String, MemberRepository> beansOfType = ac.getBeansOfType(MemberRepository.class);
        for (String key : beansOfType.keySet()) {
            System.out.println("key = " + key + "value = " + beansOfType.get(key));
        }
        System.out.println("beansOfType = " + beansOfType);
        assertThat(beansOfType.size()).isEqualTo(2);
    }
```
- 특정 타입을 조회할 때는 `getBeansOfType()`을 쓰면 된다.
- `assertThat(beansOfType.size()).isEqualTo(2);`
    - 특정 타입이 2개가 맞는지 판별하는 것
    - `isEqualTo(3)`이면 3개가 맞는지 판별하는 것
</br></br>

상속관계 일땐 부모 타입으로 조회를 하면 자식 타입도 함께 조회된다.</br>
그래서 Object타입으로 조회를 하면 모든 스프링 빈을 조회한다.
</br></br></br></br></br>


## 스프링 빈 설정 메타 정보 - BeanDefinition
**BeanDefinition**
- 빈(Bean) 설정 메타 정보
- 역할과 구현을 개념적으로 나눈 것.
- XML,자바코드를 읽어서 `BeanDefinition`을 만들 수 있다.
- 스프링 컨테이너는 자바코드인지 XML인지 구분을 못해도 된다.
- 자바코드에서 `@Bean`, XML에서 `<bean>`당 각각 하나의 메타정보가 생성
- 스프링 컨테이너(ApplicationContext)는 이 메타정보를 기반으로 스프링 빈을 생성

![BeanDefinition](https://user-images.githubusercontent.com/84119178/154791706-418a2aba-32fe-4e50-8752-33c3d511d824.png)
