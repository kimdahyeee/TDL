### 변수와 상수

- `let` - 모던한 변수 선언 키워드
- `var` - 오래된 변수 선언 키워드
- `const` - 상수 선언 키워드. 변수의 값을 변경할 수 없음

#### 대문자 상수 올바르게 사용하기

```
const BIRTHDAY = '1994.11.15';
const age = chageAge(BIRTHDAY); 
```
위와 같이 대문자 상수 사용하자.

- 하드 코딩된 값인 경우
- 실행 전에 이미 값을 알고 있고, 코드에서 직접 사용하는 경우

#### `var`를 지양하고, `let`, `const`를 사용하자
> 근래에는 `var`를 사용하지 않는다. 단, 오래된 스크립트에서 `var`를 `let`으로 수정할 때에 조심해야한다. 
>
> `var` 과 `let` 함수의 차이를 잘 알아둘 것 !

**1. `var`은 함수 수준의 스코프, `let`은 블록 스코프이다.**
- `var`
    ```
    if (true) {
      var test = true; // 'let' 대신 'var'를 사용했습니다.
    }
    
    alert(test); // true(if 문이 끝났어도 변수에 여전히 접근할 수 있음)
    ```

- `let`
    ```
    if (true) {
      let test = true; // 'let'으로 변수를 선언함
    }
    
    alert(test); // Error: test is not defined
    ```

**2. `var`는 재선언이 가능하다.**
이미 선언된 변수도, 재선언이 가능하기 때문에 코딩 실수에 의해 데이터가 변할 수 있다.

- `var`
    ```
    var user = "Pete";
    
    var user = "John"; // this "var" does nothing (already declared)
    // ...it doesn't trigger an error
    
    alert(user); // John
    ```
- `let`
    ```
    let user;
    let user; // SyntaxError: 'user' has already been declared
    ```
  
**3. 선언하기 전에 사용할 수 있는 `var` (변수 호이스팅)**
> `var` 선언은 함수가 시작되는 시점 (전역 공간에서는 스크립트가 시작되는 시점)에서 처리된다.

#### 참고
- [ko.javascript.info - 변수와 상수](https://ko.javascript.info/variables)
- [ko.javascript.info - 오래된 'var'](https://ko.javascript.info/var)