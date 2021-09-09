## rabbitMQ
메시지 브로커 소프트웨어 오픈소스

### 메시지 브로커란 ?
publisher 로 부터 전달받은 메시지를 subscriber 에게 전달해주는 중간 역할

이 때, 메시지가 적재되는 공간을 message queue 라고 한다.

### rabbitMQ 동작
```
                          Broker
Producers -> [Exchange -- Binding --> Queue] -> Consumers
```
- `producers` : 메시지 발행
- `exchange` : producer 가 전달한 메시지를 queue 에 전달하는 역할
- `broker binding` : 설정된 bindings 라는 규칙 (원하는 queue 로 전달하기 위한 규칙)
- `consumers` : 메시지를 받는 역할

### `exchange` 메시지 전달 방식
1. direct exchange : 메시지에 포함된 `routing key` 를 기반으로 queue 에 메시지 전달
2. Fanout Exchange
3. Topic Exchange
4. Headers Exchange

### rabbitMQ 와 High Available