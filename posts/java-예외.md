# 예외
---

## item 69. 예외는 진짜 예외 상황에서만 사용하라
- 예외는 오직 예외 상황에서만 써야 한다. 절대로 일상적인 제어 흐름용으로 쓰여선 안 된다.
- '상태 검사' 메소드를 사용해서, 예외를 사용할 일이 없게 만들어도 좋다.

## item 70. 복구할 수 있는 상황에서는 `checked exception`을, 프로그래밍 오류에는 `unchecked exception`을 사용하라
- 호출하는 쪽에서 복구하리라 여겨지는 상황이라면 `checked exception`를 사용하라.
- `unchecked exception`은 프로그램에서 잡을 필요가 없거나 혹은 통상적으로 잡지 말아야 한다.
- 어떤 예외를 사용할 지 확신하기 어렵다면 `unchecked exception`을 선택하는 편이 낫다.

> **clean code 133p, unchecked Exception을 사용하라**
> 
> throws 경로에 위치하는 모든 함수가 최하위 함수에서 던지는 예외를 알아야 하므로 **캡슐화**가 깨진다.

## item 71. 필요없는 `checked exception` 사용을 피하라
- `checked exception`은 발생한 문제를 프로그래머가 처리하여 안정성을 높이게 해주지만, 남용하면 쓰기 고통스러운 API를 낳는다.
- API 호출자가 예외 상황에서 복구할 방법이 없다면 `unchecked exception`을 던지자.
- 복구가 가능하고 호출자가 그 처리를 해주기 바란다면, 우선 빈 옵셔널을 반환하는 방식을 고려하자.

## item 72. 표준 예외를 사용하라
- `IllegalArgumentException`: 허용하지 않는 값이 인수로 건네졌을 때
- `IllegalStateException`: 객체가 메서드를 수행하기에 적절하지 않은 상태일 때
- ...

## item 75. 예외의 상세 메시지에 실패 관련 정보를 담아라
- 실패의 순간을 포착하려면 발생한 예외에 관련된 모든 매개변수와 필드의 값을 실패 메시지에 담아야한다.
- 단, 사용자에게 보여질 메시지와 예외의 상세 메시지를 구분할 필요가 있다. (방법은 ?)

### ⚠️ `e.printStackTrace()` 를 사용하지 말라는 이유
- `e.printStackTrace()`를 호출하면 스택 정보를 확인하는 과정을 거친다. => 스택 정보를 가져오기 위해 CPU 를 사용하고, 프린트 하기 위해 시간도 소요된다.
- 운영 시, 여러 스레드에서 콘솔에 로그를 프린트하면 데이터가 섞이기 때문에, 알아보기 어렵다. (보통 에러 추적을 위한 데이터와 함께 로그를 남기는 것이 바람직)
- 예외 스택 정보를 자바에서는 최대 100개까지 출력하기 때문에 서버 성능에도 좋지 않다.
- 톰캣에서 `e.printStackTrace()`로 콘솔에 찍힌 값은 catalina 로그에만 남는다. (에러 로그를 제어하기 힘들다.)

> **`e.printStackTrace()` 대신 로거를 사용하자.**
> 
> `log.error("유의미한 message", e);` 와 같이 유의미한 메세지와 함께 전체 에러 스택을 남기는 것이 좋다.
> 
> 로거를 사용하면 일관되게 로그 포맷에 영향을 주고, 원하는 위치, 파일에 저장할 수 있다.
> 운영 시 에러 원인을 파악하기 더 좋다.

## item 76. 가능 한 실패 원자적으로 만들라
- 가장 간단한 방법은 불변 객체를 설계하는 것이다.
- 예외가 발생하더라도 객체의 상태는 메서드 호출 전과 똑같이 유지돼야 한다는 의미이다.

## item 77. 예외를 무시하지 말라

## clean code 138p~ null을 반환/전달하지 말라
- `NullPointerException` 에서 벗어나지 못한다 !

## 참고
- effective java 3/E
- clean code
