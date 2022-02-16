> 이 글은 김영한님의 [스프링 핵심 원리- 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8/dashboard) 강의의 일부분에서 요약한 것입니다.

</br></br>

# 스프링 컨테이너와 스프링 빈
</br>

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
