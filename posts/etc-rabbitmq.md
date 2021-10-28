## rabbitMQ
메시지 브로커 소프트웨어 오픈소스

> `AMQP(Advanced Message Queing Protocol)`의 구현체 

### 메시지 브로커란 ?
publisher 로 부터 전달받은 메시지를 subscriber 에게 전달해주는 중간 역할

이 때, 메시지가 적재되는 공간을 message queue 라고 한다.

### rabbiMQ 와 같은 메시지 브로커를 사용하는 이유 ?
- 대용량 메시지 처리
- 어플리케이션 간 결합도를 낮춤 (rabbitMQ 가 죽어도 서비스는 유지된다.)

> 대규모 시스템에서는 많은 데이터 교환 엔드포인트가 생성된다. 메시지 큐를 사용함으로써 통합된 protocol, data format 을 사용할 수 있다.

### rabbitMQ 동작
```
                          Broker
Producers -> [Exchange -- Binding --> Queue] -> Consumers
```
- `producers` : 메시지 발행
- `exchange` : producer 가 전달한 메시지를 `queue` / 다른 `exchange` 에 전달하는 역할
- `broker binding` : 설정된 bindings 라는 규칙 (원하는 queue 로 전달하기 위한 규칙)
- `consumers` : 메시지를 받는 역할

### `exchange` 메시지 전달 방식
1. direct exchange : 메시지에 포함된 `routing key` 를 기반으로 queue 에 메시지 전달
2. Fanout Exchange
3. Topic Exchange
4. Headers Exchange

### rabbitMQ 와 High Available