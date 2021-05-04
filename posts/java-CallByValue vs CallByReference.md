## `call by value` vs `call by reference`

> 찾아보자고 써놓고 드디어 찾아봤다.....

### call by value
말 그대로 "값에 의한 호출"이다.
- 함수 호출 시 인자로 전달되는 변수의 값을 **복사**하여 함수의 인자로 전달
- 복사된 인자는 지역적으로 사용되는 local value 의 특성을 가진다.

즉, **함수 안에서 인자의 값이 변경되어도, 외부의 변수는 변경되지 않음!**

### call by reference
말 그대로 "참조에 의한 호출"
- 함수 호출 시 인자로 변수의 레퍼런스**를** 전달

즉, **인자의 값이 변경되면, 인자로 전달된 변수의 값도 함께 변경!!**

### java 는 call by value !
기본 자료형의 경우 변수의 값을 **복사**해서 전달하고,
참조 자료형의 경우 변수가 가지는 값이 레퍼런스이므로 
인자를 넘길 때 변수가 가지는 레퍼런스가 **복사**되어 전달된다. (이 때, 레퍼런스가 전달되는 것이 아님!)

코드로 이해하자.

```
public class Main {
    public static void main(String[] args) {
        String hello = "Hello";
        bye(hello);
        System.out.println(hello); // hello 출력
    }

    public static void bye(String param) {
        param = "bye";
    }
}
```

변수의 값만 복사해서 넘기기 때문에 변수 `hello` 의 값은 변경되지 않는다.



### 참고
- [https://github.com/WeareSoft/tech-interview/blob/master/contents/java.md#call-by-reference%EC%99%80-call-by-value%EC%9D%98-%EC%B0%A8%EC%9D%B4](https://github.com/WeareSoft/tech-interview/blob/master/contents/java.md#call-by-reference%EC%99%80-call-by-value%EC%9D%98-%EC%B0%A8%EC%9D%B4)