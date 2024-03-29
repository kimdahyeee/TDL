## 개발자가 반드시 정복해야 할 객체 지향과 디자인 패턴

> '개발자가 반드시 정복해야 할 객체 지향과 디자인 패턴' 책 읽고 정리

### chapter1. 들어가기
> 변화 가능한 유연한 구조를 만들어주는 핵심 기법 중 하나가 **객체 지향**이다.
> 변화에 잘 대응할 수 있도록 소프트웨어의 구조를 만들고 더 빠른 시간에 더 적은 노력을 들여서 수정하는 것이 더 낫다.

**수정하기 좋은 코드란?**
- 새로운 기능이 추가되었을 때, 기존 코드가 영향 받지 않음
  - 서로 다른 이유로 변경되는 코드가 한 메서드에 섞여 있으면 향후, 유지보수 하기 어려워진다.
- 특정 기능을 수정할 때, 하나의 클래스만 보면 되고 코드 분석/수정이 용이함

### chapter2. 객체지향
> 객체 지향적으로 만든 코드에서는 객체의 데이터를 변경하더라도 해당 객체로만 변화가 집중되고 다른 객체에는 영향을 주지 않기 때문에,
> 변화된 요구 사항을 빠르게 반영할 수 있다.

- **단일 책임 원칙** : 객체는 단 하나의 책임만을 가져야 한다.
  - 객체가 갖는 책임의 크기는 작아질수록 **변경의 유연함**을 얻을 수 있다.
- **캡슐화**
  > **캡슐화**는 내부 구현이 변경되더라도, 그 기능을 사용하는 코드가 영향받지 않도록 만들어준다. (의존 관계의 객체가 수정되지 않음)
  - tell, don't ask (묻지말고 시켜라) : 데이터를 물어보지 않고, 기능을 실행해 달라고 말하는 규칙
  - 데미테르의 법칙 : 규칙을 지키다보면 자연스럽게 캡슐화 향상!
    - 메서드에서 생성한 객체의 메서드만 호출
    - 파라미터로 받은 객체의 메서드만 호출
    - 필드로 참조하는 객체의 메서드만 호출
    ```
      member.getDate().getTime(); // 규칙 어김
      member.someMethod(); // ok!
    ```
    > 즉, **연속된 get 메소드를 호출**하거나, **임시 변수의 get 호출이 많은 경우** 법칙을 위반한 것이라고 볼 수 있다.
    
### chapter3. 다형성과 추상 타입
> **추상화**를 통해 유연함을 얻을 수 있다.

**추상화를 가능하게 해주는 다형성**

다형성을 구현하기 위해 **인터페이스 상속**과 **구현 상속**을 사용할 수 있다.

**추상화를 통해 우리가 얻을 수 있는 이점**
1. 구현 교체의 유연함
  ```
  ByteSource source = new FileDataReader();
  ByteSource source = new SocketDataReader();
  ```
  위의 코드와 같이 **구현 교체**가 용이해진다. 나아가서, 런타임 시점에도 객체 생성을 제어할 수 있다. (ex. 팩토리 클래스)

2. 테스트 용이
  - mock 객체를 정의하여 임의 테스트 가능 (해당 인터페이스를 구현하는 mock 객체 구현)
  
**인터페이스는 변경 가능성이 높은 경우에 한해서만 사용하자**

불필요하게 프로그램 복잡도만 증가시킬 수 있고, 유연함의 휴과는 누릴 수 없게 된다.

### chapter4. 재사용: 상속보단 조립
**상속을 통한 재사용의 단점**
1. 상위 클래스 변경을 어렵게 만든다.
  - 클래스를 상속받는 다는 것은 그 클래스를 의존한다는 뜻
  - 의존하는 클래스의 코드가 변경되면 영향받을 수 있다는 뜻
2. 다중 상속이 불가하기 때문에 클래스의 불필요하게 증가할 수 있다.
  - 클래스들의 조합으로 기능을 별도로 구현해야하기 때문에 클래스가 증가
3. 상속을 잘못된 사용으로, 부필요한 책임을 갖게 된다.
  - 상위 클래스의 메서드를 잘 못 사용할 수 있다는 뜻
  - `IS-A` 관계가 성립할 때에만 사용해야 한다. (상위 클래스의 메서드를 모두 사용할 수 있는 관계)

**상속 보다는 조립을 사용하자**
- 클래스 증가 방지
- 런타임에 조립 대상 객체를 교체할 수 있다.

ex) 위임(delegation) = 멤버 변수로 조립 대상의 class 를 가지는 것

> 상속은 재사용이라는 관점이 아닌 **기능의 확장**의 관점에서 적용하자.

### chapter5. 설계 원칙: SOLID
> 객체 지향적으로 설계하는 데 기본이 되는 설계 원칙 `SOLID`
> 
> 사용자 관점에서의 설계를 지향한다.
1. 단일 책임 원칙 (Single responsibility principle)
    - 클래스는 **단 한개의 책임**을 가져야 한다.
      - 클래스는 한 개의 이유로만 변경되어야 한다.
      - 책임의 개수가 많아질수록 한 책임의 기능 변화가 다른 책임에 주는 영향은 비례해서 증가한다.
    - 단일 책임 원칙을 지킬 수 있는 방법
      - 메서드를 실행하는 것이 누구인지 확인해보기 👉 클래스의 사용자들이 서로 다른 메서드들을 사용한다면, 각각 다른 책임에 속할 가능성이 높다. 👉 책임 분리 후보가 된다.
  
2. 개방 폐쇄 원칙 (open-closed principle)
    - 확장에는 열려 있어야 하고, 변경에는 닫혀 있어야 한다.
      - 기능을 변경하거나 확장할 수 있으면서
      - 그 기능을 **사용**하는 코드는 수정하지 않는다는 말
    - 개방 폐쇄 원칙이 깨질 때 주요 증상
      - 다운 캐스팅을 한다 (ex. `instanceof` 와 같은 타입 확인 연산자 사용)
      - 비슷한 `if-else` 블록이 존재한다 
    > 개방 폐쇄 원칙은 변화가 예상되는 것을 **추상화**해서 변경의 유연함을 얻도록 해준다.
   
3. 리스코프 치환 원칙 (Liskov substitution principle)
    - **상위 타입의 객체**를 **하위 타입의 객체로** 치환해도 상위 타입을 사용하는 프로그램은 정상적으로 동작해야한다.
    - 리스코프 원칙이 깨질 때 주요 증상
      - 명시된 명세에서 **벗어난 값** 리턴
      - 명시된 명세에서 **벗어난 exception** 리턴
      - 명시된 명세에서 **벗어난 기능** 수행
    > 리스코프 원칙은 **확장**에 대한 것, 리스코프 치환 원칙을 지키지 않으면 개방 폐쇄 원칙을 지킬 수 없게 된다.
   
4. 인터페이스 분리 원칙 (Interface segregation principle)
    - 인터페이스는 그 인터페이스를 사용하는 **클라이언트**를 기준으로 분리하자
      - 클라이언트로부터 발생하는 인터페이스 변경의 여파가 다른 클라이언트에 미치는 영향을 최소화하자
  
5. 의존 역전 원칙 (Dependency inversion principle)
    - 고수준 모듈은 저수준 모둘의 구현에 의존해서는 안된다. = 저수준 모듈이 고수준 모듈에서 정의한 **추상 타입에 의존**해야 한다.
      - 고수준 모듈은 어떤 의미 있는 단일 기능을 제공하는 모듈
      - 저수준 모듈은 실제 구현하는 모듈
    - 저수준 모듈이 고수준 모듈을 의존하게 구현하자 = 추상화하자

### chapter6. DI(Dependency Injection) 와 서비스 로케이터
**DI 란?**

필요한 객체를 직접 생성하거나 찾지 않고, **외부에서 넣어 주는** 방식

**DI 주입 방식**
- 생성자 방식 - recommend
  - 생성 시점에 필요한 모든 의존 객체를 준비 가능 👉 한 번 객체가 생성되면 객체가 정상적으로 동작함을 보장
- 메서드 방식 (setter)

**DI 와 테스트**

DI 는 의존 객체를 Mock 객체로 쉽게 대체할 수 있게 함으로써 테스트를 할 수 있게 돕는다.

(= 의존 객체가 구현되지 않았더라도, mock 객체를 이용해서 테스트를 할 수 있단 말)

**서비스 로케이터**

서비스 로케이터란 ?
- 어플리케이션에서 필요로하는 객체를 제공하는 책임을 갖는 class
- 클라이언트는 로케이터를 통해 필요로하는 객체를 직접 찾아야 한다.

서비스 로케이터의 단점과 DI
- 동일 타입의 객체가 다수 필요할 경우, 각 객체 별로 제공 메서드를 만들어 주어야 한다.
    - 다른 객체로 변경하려면, 개방 폐쇄 원칙에 어긋남
- 인터페이스 분리 원칙 위배
    - 서비스 로케이터를 사용하는 코드 입장에서, 자신이 필요한 타입 뿐 아니라 서비스 로케이터가 제공하는 다른 타입에 대한 의존이 생김
    - 다른 의존 객체의 수정에 의해 영향 받을 수 있음

> 서비스 로케이터보다는 DI 를 사용하자.
> 
> 필요한 객체를 생성하거나 찾지 말고, 외부에서 넣어주는 방식(DI)를 사용하자.

### chapter7. 주요 디자인 패턴

#### 디자인 패턴
> 디자인 패턴은 특정 상황에 맞는 **해결책을 빠르게 찾을 수 있도록** 도움을 준다.

대표적으로 `GOF의 디자인 패턴`이 유명하다.

1. 전략 패턴

   > 변경되는 알고리즘을 추상화하고 있는 인터페이스를 **전략** 이라고 하며, 기능 자체의 책임을 갖고 있는 클래스를 **컨텍스트** 라고 한다.
   
    - 전략 패턴에서는 컨텍스트는 적용할 전략을 **직접 선택하지 않는다.**
        - 컨텍스트의 **클라이언트**가 사용할 전략을 **직접 전달**해 준다. (`DI` 사용하여 컨텍스트에 전략 전달)
    - 전략 패턴 적용 대상
        - 비슷한 코드를 실행하는 `if-else` 블록
        - 동일한 기능을 제공하지만 다른 알고리즘을 선택해야 하는 경우 (성능에 따라 다른 알고리즘 선택)

2. 템플릿 메서드 패턴

   > 템플릿 메서드 패턴을 사용하면, **동일한 실행 과정의 구현을 제공**하면서, 동시에 하위 타입에서 일부 단계를 구현할 수 있도록 한다. (**상속 이용**)
   > 
   > 코드의 중복을 방지할 수 있다.
   
    - 템플릿 메서드 패턴 구성
        - **실행 과정**을 구현한 **상위** 클래스
        - 실행 과정의 **일부 단계**를 구현한 하위 클래스
    - 상위 클래스는 모든 타입에게 동일하게 적용하는 실행 과정을 제공하기 때문에, 이 메서드를 **템플릿 메서드**라고 한다.
    
3. 템플릿 메서드 & 전략 패턴 조합
    
    > 템플릿 메서드와 전략 패턴을 함께 사용하면 **조립** 방식으로 템플릿 메서드 패턴을 활용할 수 있다.

   상속을 사용하지 않고, **파라미터로 콜백 함수를 전달**하여 유연한 설계를 할 수 있다.

    ```
        public <T> T execute(TransactionCallback<T> action) {
            ...
            action.doInTransaction(status);
            ...
        }    
    ```
   
4. 상태 패턴
    
    > 기능이 상태에 따라 **다르게 동작**해야 할 때 사용할 수 있는 패턴
    > 
    > 상태별 처리 코드를 상태로 분리함으로써 콘텍스트의 코드가 간결해지고 변경의 유연함을 얻게 된다.
   
    - 새로운 상태가 추가되더라도 콘텍스트 코드가 받는 영향은 최소화된다.
    - 상태에 따른 동작을 구현하는 코드가 각 상태 별로 구분되기 때문에 상태 별 동작을 수정하기가 쉽다.
    - 상태 변경은 누가 해야 할까? : 상황에 맞게 선택해야 한다.
        - 컨텍스트에서 하는 경우
            - 비교적 상태 개수가 적고, 상태 변경 규칙이 거의 바뀌지 않는 경우 유리
            - 지속적으로 상태 변경 규칙이 바뀐다면, 상태 변경 코드가 복잡해질 수 있음 > 유연함이 떨어짐
        - 상태 객체에서 하는 경우
            - 분산되어 있기 때문에, 상태 변경 규칙을 파악하기 어려울 수 있음
            - 컨텍스트에 영향 주지 않고, 상태 변경 규칙을 바꿀 수 있음

5. 데코레이터 패턴

    > **상속**이 아닌 **위임**을 하는 방식으로 기능을 **확장**한다.
   
    - 각 확장 기능들의 구현이 별도 클래스로 분리되기 때문에, 각 확장 기능 및 원래 기능을 서로 영향 없이 변경할 수 있다.
    - 각 데코레이터 객체들을 조합해서 사용할 수 있다. (중첩해서)
    
6. 프록시 패턴

    > 실제 객체를 대신하는 **프록시 객체**를 사용해서 실제 객체의 **생성이나 접근 등을 제어**할 수 있도록 해 주는 패턴
   
    - 우선 프록시 객체를 사용하고, 실제 사용 시점(필요한 시점)에 실제 객체 생성/접근 한다는 말
    - 위임 방식이 아닌 **상속**을 사용해서 프록시를 구현할 수 있다.
        - 특정 기능은 특정 클래스만 실행하도록 **보호 프록시**로 사용
        - 이 때, 차이는 상속을 했기 때문에 보호 프록시 사용 시점에 실제 객체가 생성되기 때문에, 가상 프록시로는 적합하지 않다.
    
7. 어댑터 패턴

    > 클라이언트가 요구하는 인터페이스와 재사용하려는 모듈의 인터페이스가 **일치하지 않을 때 사용**할 수 있는 패턴
   
    - 인터페이스를 맞추기 위해 **중간에** 요구하는 형식으로 변경하여, 기능을 실행하고, 기대 결과를 반환한다.
    - 어댑터 패턴이 적용된 `SLF4J` 라는 로깅 API 가 있다.
        - `SLF4J`는 동일 api 로 여러 로깅 프레임워크를 사용할 수 있도록 어댑터 패턴 적용
    
8. 옵저버 패턴

    > 한 객체의 상태 변화를 정해지지 않은 여러 다른 객체에 통지하고 싶을 때 사용되는 패턴
   
    - 옵저버 패턴은 **주제 객체**와 **옵저버 객체**로 구성된다.
        - **주제 객체** : 옵저버 목록을 관리하고, 옵저버 등록/제거할 수 있는 메서드를 제공한다. 상태의 변경이 발생되면 등록된 옵저버에게 변경 내역을 알린다.
        - **옵저버 객체** : 주제 객체가 호출하는 메서드에서 필요한 기능을 구현한다.
    - 옵저버 패턴의 장점
        - 주제 객체의 변경 없이 옵저버 객체를 추가할 수 있다.
    - 한 주제에 대한 다양한 구현 클래스가 존재한다면, 옵저버 패턴을 적용하여, 중복 코드를 방지할 수 있다.
    - 옵저버 패턴 구현의 고려 사항
        - 옵저버에서 어떤 상태에 따라 실행 여부를 결정하거나, 주제 객체에서 어떤 상태에 따라 실행 여부를 결정할 지 정해야한다.
            - 각 옵저버에서 분기 처리를 할 경우엔, 다른 옵저버의 변경이 누락될 위험이 있다. (개발자의 실수)
            - 모두 케이스가 다르다면, 각각의 옵저버에서 분기하는 것이 좋다.
        - 옵저버 인터페이스는 각 책임에 따라 분리하는 것이 좋다.
            - 불필요한 코드를 작성해야할 수 있다.
    
9. 미디에이터 패턴

    > 객체들이 직접 메시지를 주고받는 대신, 중간에 중계 역할을 수행하는 미디에이터 객체를 두고 미디에이터를 통해서 각 객체들이 간접적으로 메시지를 주고받도록 하는 패턴
   
    - 미디에이터 추상 클래스를 사용해서 미디에이터 자체의 재사용을 높일 수 있다.
    
10. 파사드 패턴

    > 코드 중복과 직접적인 의존을 해결하기 위해 사용된다.
    > 
    >  서브 시스템을 감춰 주는 상위 수준의 인터페이스를 제공하는 패턴
    
    - 클라이언트의 코드가 간결해진다.
    - 클라이언트와 서브 시스템의 간접적인 의존을 제거할 수 있다.
    
    > 중복된 코드에서 미세한 차이가 발생하면 이후 변경해 주어야 할 때 미세한 차이점을 누락할 가능성이 높아지고, 이는 결국 프로그램에 버그를 만드는 원인이 된다.
    
11. 추상 팩토리 패턴

    > 클라이언트로부터 **객체 생성 책임**을 분리 (추상화 사용)
    
    - 클라이언트에 영향을 주지 않으면서 사용할 객체를 변경할 수 있다.
    - 대표적으로 `JDBD API` 가 추상 팩토리 패턴 적용

12. 컴포지트 패턴

    > 부분과 전체를 **하나의 인터페이스**로 추상화
    
13. 널 객체 패턴

    > `null`대신 사용될 클래스를 구현한다. 이 클래스는 상위 타입을 상속받으며, 아무 기능도 수행하지 않는다.
    > `null`을 리턴하는 대신 `null`을 대체할 객체를 리턴한다.
    
    - `null` 검사 코드를 사용하지 않아도 된다. => 코드가 간결해지고, 가독상이 높아지며, 유지보수하기 좋아진다.