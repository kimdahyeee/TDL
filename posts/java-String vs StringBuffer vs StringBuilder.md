# String vs StringBuffer vs StringBuilder

String 은 잘 못 사용한다면 **메모리**와, **성능**에 많은 영향을 끼칠 수 있다.

### `String` vs `StringBuffer`, `StringBuilder` 의 차이
> 객체를 append 하는 시점의 객체 생성의 차이

`String`은 매번 객체를 새로 생성한다. (매번 객체를 생성하기 때문에 메모리는 점점 증가하고, 더이상 사용하지 않는 객체를 GC를 통해 정리해야 하므로 CPU 사용이 증가하기 때문에, 성능도 좋지 않다.(시간이 오래 걸린다.))

`StringBuffer`, `StringBuilder` 는 기존의 객체의 크기를 증가시키면서 값을 더한다.


### `StringBuffer` vs `StringBuilder` 차이
> `thread-safe` 함의 차이

`StringBuffer`는 스레드에 안전하게 설계되어 있다.
```
@Override
// append 메서드에 synchronized 선언되어 있다.
public synchronized StringBuffer append(Object obj) {
    toStringCache = null;
    super.append(String.valueOf(obj));
    return this;
}
```

`StringBuilder`는 단일 스레드에서만 안정성을 보장한다.
즉, 여러 스레드에서 사용할 때 문제가 발생할 수 있다.

### jdk 버전에 따른 차이
jdk5.0 이상에서는 `+` 연산시 컴파일러가 `StringBuilder`로 append 하도록 변환해준다.
(반복 루프에서는 불가능하다. 개발자가 조심하자.)

- 자바 성능 튜닝 이야기 55p 참고

### 결론

`String`은 짧은 문자열을 더할 경우 사용한다.

반복 연산을 통해 문자열을 더할 경우에는 `StringBuffer`나 `StringBuilder` 를 사용하자. 

`StringBuffer`는 스레드에 안전한 프로그램이 필요할 때나, 개발 중인 시스템의 부분이 스레드에 안전한지 모를 경우 사용하자.
(ex. static 으로 선언한 문자열을 변경하거나, singleton 클래스에 선언된 문자열인 경우)

`StringBuilder`는 스레드에 안전한지의 여부와 전혀 관계없는 경우 사용하자.
(ex. 메서드 내에 변수를 선언한 경우 - 메서드 내에서만 해당 변수가 살아있으므로)

### 참고
- 자바 성능 튜닝 이야기