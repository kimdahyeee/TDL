### JVM, JRE, JDK

---

![](../images/java-jvm-jre-jdk.jpg)

- 참고로 `java 11` 에서 부터는 `JDK`만 제공한다. (`JRE`를 따로 제공하지 않음)

### JVM 구조
- java source (.java 파일)은 **java compiler**에 의해 java byte code(.class 파일) 이 생성된다.
- 이 과정을 통해 생성된 바이트 코드와 class library 를 기반으로 **class loader** 가 클래스 로드한다.
![](../images/java-jvm.jpg)

### runtime data area
JVM 메모리 영역 (JVM 이 운영체제 위에서 실행되는 시점에 메모리 영역 할당!)
![](../images/java-runtime-data-area.jpg)

### heap 영역과 garbage collector
garbage collector 는 동적으로 할당한 메모리 영역 중 사용하지 않는 영역을 탐지하고 해제하는 역할을 수행

**garbage collector 과정**
- statck 의 모든 변수를 스캔하면서 각각 어떤 객체를 참조하고 있는 지 찾아서 마킹
- reachable object 가 참조하고 있는 객체도 찾아서 마킹
- 사용하지 않는 객체 삭제

![](../images/java-jvm-heap.jpg)
**minor GC 발생**
- eden 영역이 꽉 차면, minor GC 발생
- eden 영역의 reachable 객체는 survivor 0 으로 옮겨진다.
- eden 영역의 unreachable 객체는 메모리에서 해제
- 반~복 (eden 이 차면 -> survivor 0 이 채워짐)
- survivor 0 이 다 차게됨
- survivor 0 영역의 객체가 survivor 1 로 이동 (이동한 객체는 age 증가)
- survivor 1 이 다 채워졌을 때, survivor 0 으로 이동시키며, 사용하지 않는 메모리 해제 (**바로 old 로 가는게 아님!**)
    - 이 때, 한 영역(survivor 0~1 중 한 영역)은 비워진 상태로 유지
    - survivor 1 <-> survivor 0 옮기면서 age 가 증가된다.
- age 가 특정 값 이상이 되면 **old generation** 으로 옮겨진다. (promotion 과정)
**major GC 발생**
- old generation 이 꽉 차면, **full GC (major GC)** 발생

**OutOfMemory(OOM)**
- garbage collector 가 새로운 object 를 유지하기 위해 새로운 공간을 확보하지 못할 때 발생
  - heap size 부족한 경우
  - permanent area 가 가득 차면 outOfMemory 발생
  - memory leak 에 의해

**java8 에서 perm 이 사라지고 metaspace 가 추가된 이유 ?**
perm 영역은 JVM heap 에서 관리되는 영역이었기 때문에 메모리의 한계가 존재 => 개발자의 실수로 OOM 이 발생할 수 있음

metaspace 로 변경되며 OS 레벨에서 관리되는 native 영역으로 이동 => 영역에 대한 사이즈가 perm 영역보다 훨씬 크다.

### 참고
- [https://hoonmaro.tistory.com/19](https://hoonmaro.tistory.com/19)
- [outOfMemoryError 관련 포스팅](https://www.nextree.co.kr/p3878/)
- [https://changrea.io/java/oom-issue/](https://changrea.io/java/oom-issue/) :: 읽어보기!