## `null` vs `undefined`

### javascript 의 `null`과 다른 언어에서의 `null`
다른 언어에서의 `null`은 **존재하지 않는 객체에 대한 참조**나 **널 포인터**를 나타낼 때 사용한다.

javascript 의 `null`은 **존재하지 않는 값**, **비어 있는 값**, **알 수 없는 값**을 나타내는 데 사용한다.

💡 즉, 명시적으로 **존재하지 않는 값**, **비어 있는 값**이라고 나타낼 때 사용된다.

**tmi )**
`typeof null`(`null`의 타입)은 `object`를 반환한다. 
`null`은 객체가 아니지만, 하위 호환을 위해 남겨두고 있는 것이라고,, (초창기(?) 버그라고 한다..)

### `undefined` (vs `null`)

`undefined`와 `null`은 '타입'이다. (여기서 **타입**에는 문자열, 숫자 타입을 말할 때의 '타입'이다.)

`undefined`는 **값이 할당되지 않은 상태**를 나타낼 때 사용한다.
    
💡 즉, 변수는 선언했지만, 값을 할당하지 않은 `let a; //undefined`와 같은 경우이다.

**tmi 1 )**
`undefined`를 직접 할당하지 않아도 된다. `let a;` 선언과 동시에 `a`에는 `undefined`가 할당된다.

**tmi 2 )**
`null`은 식별자가 아닌 특별한 키워드이기 때문에 `null = 1` 과 같이 정의할 수 없지만,
`undefined`는 `undefined = 2`와 같이 선언할 수 있다.(느슨한 모드에서) **이렇게 사용하지 말자 ^^**

### 값이 없는(undefined) vs 선언되지 않은(undeclared)

보통 `undefined`는 '값이 없음'의 의미로 쓰인다.

개발하다보면 `b;` 라고 호출한다면 `Uncaught ReferenceError: b is not defined`와 같은 에러 메시지를 보게되는데,,
여기에서 `is not defined`는 undefined 의 의미가 아닌, 선언조차 되지 않음(undeclared)의 의미이다.

헷갈리기 쉬우니 알아둘 것 !


### 참고
- [https://ko.javascript.info/types](https://ko.javascript.info/types)
- YOU DON'T KNOW JS - 타입과 문법, 스코프와 클로저
