## java string pool
String 타입은 **불변성**이라는 성질을 가지고 있고, 같은 값에 대해서는 단 하나의 문자열만 생성하도록 설계되어 있다.
(메모리의 최적화를 위함 !)

JVM 의 heap 영역에는 **string pool** 이라는 특별한 영역에서 String 객체들을 관리한다.

### string pool
string pool 의 실체는 HashMap 이다.
문자열 리터럴을 사용하여 String 을 생성하면, `string pool` 에 기존에 같은 값을 가지는 String 객체가 있는지 검사하고
있으면 그 객체의 참조값을, 없으면 `string pool` 에 새 객체를 생성하고 그 참조값을 반환한다.

```
String a = "aaa"; // 리터럴 생성
String b = "aaa";
System.out.println(a == b); // true
```

### 리터럴을 이용한 String 생성과 new String() 을 이용한 생성의 차이
```
String a = "a";
String b = "a";
String c = new String("a");
String d = new String("a");
 
System.out.println(a == b); //true
System.out.println(a == c); //false
System.out.println(c == d); //false
System.out.println(a.equals(b)); //true
System.out.println(a.equals(c)); //true
System.out.println(c.equals(d)); //true
```
new 연산자를 통해 생성하면, `string pool` 영역이 아닌 `heap` 영역에 생성된다. (기존 값을 재활용하지 않고 늘 새로 생성!)

따라서, 리터럴 생성과 참조 값이 같지 않다.

**TMI**
java 6 버전까지는 `permGen` 영역에 저장되어, `String pool` 에 문자열 객체가 많이 생성되면 `outOfMemory` 에러가 발생
(`permGen` 영역은 runtime 중 메모리를 동적으로 늘릴 수 없음)

java7 부터는 `permGen` 영역이 아닌 `heap` 에 `string pool` 생성하여 `OOM` 이슈 위험성 적어짐

### 참고
- [https://dololak.tistory.com/718](https://dololak.tistory.com/718)
- [https://www.baeldung.com/java-string-pool](https://www.baeldung.com/java-string-pool)