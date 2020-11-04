## 빈 주입 시 인터페이스가 있다면 인터페이스를 쓰자

> 인터페이스가 있음에도 불구하고 클래스로 빈을 주입 받는다면,
> 특별한 경우에 예외 case 를 만날 수 있다.

### 스프링 기본 AOP 설정
spring 은 AOP로 proxy를 만들 때, `class` 기반으로 proxy를 생성한다.
(spring은 class의 자식 클래스를 만들어서 proxy를 생성하게 된다.)

### 클래스로 빈을 주입받을 때, 우리가 만날 수 있는 예외 🤢
임의로 `class`기반 proxy 생성 아닌 `interface`기반으로 proxy를 생성하도록 변경하는 경우를 생각해보자.

**우선, `class` 기반 proxy 생성과 `interface` 기반 proxy 생성시 빈 주입의 차이에서 확인할 수 있다.🙂**
- 아래와 같이 `interface`와 그 `interface`를 구현한 `TestService`라는 클래스가 존재하는 경우에
    ```
    public interface TestInterface {
        public void doSomething();
    }
    
    @Component
    public class TestService implements TestInterface {
        @Override
        public void doSomething() {
            ...
        }
    }
    ```
    - `class` 기반 proxy 생성 (스프링 부트 기본 사용)
       
        👉 `TestService ---> TestClassProxy (클래스의 자식 클래스)` 와 같이, TestService 에 대한 proxy 객체가 생성되고, `TestClassProxy`는 `TestService` 클래스를 참조한다.
        
        👉 `TestInterface ---> TestService ---> TestClassProxy` 와 같이 연결~연결~ 참조가 가능하다.
        
        > `@Autowired TestService testService` 도, `@Autowired TestInterface testService` 도 빈 주입이 가능하다. spring boot 에서 interface 든, 타겟 객체든 빈을 주입받을 수 있는 이유가 여기에 있다.

    - `interface` 기반 proxy 생성
    
        👉 `TestInterface ---> TestInterfaceProxy` 와 같이, interface에 대해 proxy 객체가 생성된다.
        
        👉 `TestInterface ---> TestService` = `TestService`는 `TestInterface` 타입이며
        `TestInterface ---> TestInterfaceProxy` = `TestInterfaceProxy` 는 `TestInterface` 타입이다.
        
        👉 하지만, `TestInterfaceProxy` 는 `TestService`의 타입이 아니다.
        
        > 따라서, `@Autowired TestService testService`와 같이 `TestService` 타입으로 빈을 주입할 수 **없다.**
        
### 결론
proxy를 생성할 때, `class`기반이던, `interface` 기반이던
빈 주입을 `interface` 기반으로 한다면, 문제될 게 없다.

`interface` 기반 빈 주입을 하자 😊
        
        
### 참고
- [백기선님 youtube - 인터페이스가 있을 땐 그걸 써야 하는 이유 (퀴즈)](https://www.youtube.com/watch?v=zfl_3Ell03g)
- [백기선님 youtube - 인터페이스가 있을 땐 그걸 써야 하는 이유 (해설)](https://www.youtube.com/watch?v=C6nsjqrCJq4)