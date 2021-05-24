# ==, equals() 와 hashCode()

> `equals()` 를 재정의 할 때, 왜 `hashCode()` 도 재정의 하라고 할까?

## `==` 연산자와 `equals()`
`==` 연산자는 primitive type 은 **값**에 대해 비교하고, reference type 은 **참조하는 주소 값**을 비교한다.

`equals()` 는 어떻게 정의하느냐에 따라 다르겠지만, 보통 **내용**이 같은지 비교한다.

### `Object`의 `equals()`
아래와 같이 단순히 `==` 비교를 한다.
```
public boolean equals(Object obj) {
    return (this == obj);
}
```

### `String` 은 문자열 내용이 같다면 `equals()` 는 true 를 반환한다.
`String`은 아래와 같이 `equals()` 메서드를 재정의하여 Char 단위로 한 글자씩 문자열을 비교한다.
```
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    if (anObject instanceof String) {
        String aString = (String)anObject;
        if (!COMPACT_STRINGS || this.coder == aString.coder) {
            return StringLatin1.equals(value, aString.value);
        }
    }
    return false;
}

//StringLatin1.equals(value, aString.value);
@HotSpotIntrinsicCandidate
public static boolean equals(byte[] value, byte[] other) {
    if (value.length == other.length) {
        for (int i = 0; i < value.length; i++) {
            if (value[i] != other[i]) {
                return false;
            }
        }
        return true;
    }
    return false;
}
```

## `equals()` vs `hashCode()`
`equals()` 는 **내용**이 같은 지 확인하고, `hashCode()` 는 두 객체가 **같은** 객체인지 확인한다.

## `hashCode()`
객체의 해쉬코드란 객체를 식별할 하나의 정수 값을 의미한다.

`Object`의 `hashCode()` 메소드는 객체의 메모리 번지를 이용해서 해시코드를 만들어 리턴하기 때문에 객체마다 다른 값을 가진다.

## `hashCode()` 와 `hash` 관련 `Collections`
ex) `HashSet`, `HashMap`, `HashTable`

이러한 컬렉션들은 key 값이 hash code 기준으로 정해진다.

자세히 말하면 아래와 같은 순서로 두 객체가 동등한지 비교한다. (key 값이 존재하는 지 확인한다.)
- `hashCode()`를 수행하여 리턴된 해시코드 값이 같은 지 확인한다.
- 이 때, 다르면 다른 객체로 판단하고
- 해시코드 값이 같다면 `equals()` 메소드로 비교하여 같다면 동등한 객체로 판단한다.

> 따라서, `hashCode()`, `equals()` 중 하나라도 false 를 반환하면, 다른 객체라고 판단하여 key 가 의도하지 않은대로 중복되어 입력될 수 있다.
> 
> 즉, 객체의 동등 비교를 위해서는 `equals()` 만 재정의하지 말고, `hashCode()` 도 재정의해야한다.

## `String` 의 `hashCode()` 
`String` 에서는 `hashCode()` 를 재정의하여, 내용이 동일하면 동일한 `hashCode()` 를 반환하도록 설계되어 있다.
```
public int hashCode() {
    int h = hash;
    if (h == 0 && value.length > 0) {
        char val[] = value;

        for (int i = 0; i < value.length; i++) {
            h = 31 * h + val[i];
        }
        hash = h;
    }
    return h;
}
```

+a) jdk7~ `switch~case`문에서 `String` 을 비교할 때, `hashCode()` 의 int 값으로 구분하여 사용한다.

## 결론
`equals()` 를 재정의한다면, side effect 를 줄이기 위해, `hashCode()` 도 재정의하자.

## 참고
- [https://nesoy.github.io/articles/2018-06/Java-equals-hashcode](https://nesoy.github.io/articles/2018-06/Java-equals-hashcode)
- 이것이 자바다 - 신용권