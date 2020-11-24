## 인터페이스 기반 빈 주입

> 인터페이스 기반 빈 주입 시 우선순위가 어떻게 되는 걸까?
>
> 알고있었던 개념이지만, 최근에 아래의 케이스를 만나고, 빈 주입에 대해 의문이 들었다.
> 이번 기회에 정확히 기억해두자.

### 빈 주입 우선순위
인터페이스 기반의 빈 주입 시, 빈을 찾는 순서는 아래와 같다.

1. 빈의 타입으로 검색하여 빈 주입
2. 빈의 이름으로 검색하여 빈 주입

1번의 과정으로 빈을 찾지 못한다면 2번의 과정을 거쳐, 빈을 찾게되고,
2번의 과정이 끝나도 빈을 찾지 못한다면, 컴파일 에러가 발생한다.

### 빈 주입 시 실수하기 좋은 케이스
1. interface 구현체가 1개일 때, 빈이 아닌 경우
- 에러 
```
public interface TeaBag {
}

// 빈이 아니기 때문에 에러 발생
public class Chamomile implements TeaBag {
}

@Component
public class Water {
    @Autowired
    private TeaBag chamomile;
}
```

- 정상
```
public interface TeaBag {
}

// 빈이기 때문에 정상 수행
@Component
public class Chamomile implements TeaBag {
}

@Component
public class Water {
    @Autowired
    private TeaBag chamomile;
}
```

2. `interface`의 구현체가 2개일 때, 주입 대상이 빈이 아닌 경우
- 정상수행되지만, 빈 주입을 확인해보면 `GreenTea`로 주입 (빈이 잘 못 주입되는 경우)

  👉 **빈의 타입**으로 검색하여 찾은 게 1개이기 때문에, 바로 주입된 경우 (이미 찾았기 때문에, 이름으로 찾지 않는다.)
```
public interface TeaBag {
}

public class Chamomile implements TeaBag {
}

@Component
public class GreenTea implements TeaBag {
}

@Component
public class Water {
    @Autowired
    private TeaBag chamomile;
}
```
- 정상

  👉 **빈의 타입**으로 검색하여 찾은 게 2개이기 때문에, 이름으로 재 검색하여 빈을 찾는다.
```
public interface TeaBag {
}

@Component
public class Chamomile implements TeaBag {
}

@Component
public class GreenTea implements TeaBag {
}

@Component
public class Water {
    @Autowired
    private TeaBag chamomile;
}
```

### 참고
- [테스트코드](https://github.com/kimdahyeee/blog-code/tree/master/spring-injection)
- [어노테이션이 달린 빈의 자동 스캔 (Without Spring Boot)](https://perfectacle.github.io/2019/06/23/auto-scanning-annotation-based-bean/)