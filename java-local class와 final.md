
### 로컬 클래스와 final

[로컬 클래스에 대한 설명을 보려면 클릭하자.](./%20java-중첩클래스.md)

#### 로컬 클래스에는 접근 제한자를 붙일 수 없다.
여기에서 접근 제한자는 `private`, `public`등이고, static 도 붙일 수 없다.
로컬클래스는 메소드 내부에서만 사용되므로 접근을 제한할 필요가 없기 때문이다.

따라서, 로컬 클래스는 메소드가 실행될 때 메소드 내에서 객체를 생성하고 사용해야 한다.
주로 비동기 처리를 위해 스레드 객체를 만들 때에 사용된다.

#### 로컬 클래스와 final

로컬클래스의 객체는 메소드 실행이 끝나도 **힙메모리**에 존재해서 계속 사용할 수 있다.
매개 변수나 로컬 변수는 메소드 실행이 끝나면 **스택 메모리**에서 사라진다. (로컬 객체에서 사용할 경우 문제가 된다.)
이 때문에 자바는 컴파일 시 로컬 클래스에서 사용하는 매개 변수나 로컬 변수의 값을 로컬 클래스 내부에 복사해 두고 사용한다.
그리고 매개 변수나 로컬 변수가 수정되어 값이 변경되면 로컬 클래스에 복사해 둔 값과 달라지는 문제를 해결하기 위해
매개변수나 로컬 변수를 `final`로 선언해서 수정을 막는다.

> 즉, 로컬 클래스에서 사용 가능한 것은 `final`로 선언된 매개 변수와 로컬 변수뿐이라는 것이다.

- 자바 7 이전
    `final` 키워드를 꼭! 선언해 줬어야 했다. (컴파일 에러)
- 자바 8 ~
    `final`선언을 하지 않아도 된다.
    하지만, `final` 선언을 하지 않아도 `final` 특성을 가진다는 것을 알아두자.
    
    `final` 키워드가 붙은 필드는 로컬 변수로 복사되고,
    `final` 키워드가 붙지 않은 필드는 지역 변수로 복사된다.
```
// 소스 코드
void outMethod(final int arg1, int arg2) {
    final int var1 = 1;
    int var2 = 2;
    class LocalClass {
        void method() {
            int result = arg1 + arg2 + var1 + var2;
        }
    }
}

// 컴파일 후
class LocalClass {
    int arg2 = 매개값;
    int var2 = 2;
    void method() {
        int arg1 = 매개값;
        int var1 = 1;
        int result = arg1 + arg2 + var1 + var2;
    }
}
```

😮 참고
이것이 자바다
[https://www.slipp.net/questions/278](https://www.slipp.net/questions/278)
[https://docs.oracle.com/javase/tutorial/java/javaOO/localclasses.html](https://docs.oracle.com/javase/tutorial/java/javaOO/localclasses.html)
