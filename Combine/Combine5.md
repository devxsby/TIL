# Combine 시작하기(5)-기타

## Cancellable

[Cancellable](https://developer.apple.com/documentation/combine/cancellable) 프로토콜은 특정 연산이 취소가 가능함을 의미한다. Subscriber를 직접 구현한다면 이 Cancellable을 채택해서 작업을 취소할 수 있도록 해줘야 한다. 이 구현은 Subscriber가 가지고 있는(취소를 위해서는 반드시 실행 연산이 진행되는 Subscription 객체를 Subscriber가 가지고 있어야 함) Subscription 객체를 대상으로 cancel() 호출하고, Subscriber가 가지고 있는 참조를 없애는 것으로 간단하게 구현할 수 있다. 이는 Subscription 프로토콜이 Cancellable 객체를 상속 받기 때문이다. 참고로 Apple이 제공하는 Subscriber인 Sink와 Assign은 이미 Cancellable이 구현되어 있다. 만약에 Subscription 마저 직접 구현해야 한다면, 연산 취소 기능은 Subscription에 직접 구현해야 한다.


## Subject

Subject는 Publisher의 일종인데, 외부에서 값을 주입할 수 있는 Publisher이다. Operator와 무엇이 다른가 하면, Operator 객체는 직접 만들 일이 없고 값을 가공하거나 필터링 하는 등의 기능적인 이유로 사용하는 데 반해, Subject는 다른 곳에서 만들어진 값을 Combine의 인터페이스로 사용할 수 있도록 만드는데 그 목적이 있다. Subject는 [send(_:)](https://developer.apple.com/documentation/combine/subject/send(_:))라는 메소드를 추가적으로 제공해주며, 이 메소드를 통해 값이나 완료, 에러 메시지를 보낼 수 있다. 그렇게 되면 이 subject를 구독하고 있는 모든 Subscriber에게 이 값이 전달되게 된다. Subject 역시 Publsiher의 일종이기 때문에 여러 Operator를 붙일 수 있다. Apple이 기본 제공해주는 subject로는 [PassthroughSubject](https://developer.apple.com/documentation/combine/passthroughsubject)와 [CurrentValueSubject](https://developer.apple.com/documentation/combine/currentvaluesubject)가 있으며, 이 두 가지의 차이는 가장 마지막에 내보낸 값을 내부에서 저장하고 있는지 여부이다. Passthrough는 말그대로 통과해버리기 때문에 저장하고 있지 않고, CurrentValue는 가장 마지막에 내보낸 값을 저장하고 있다. 만약 새로운 Subscriber가 CurrentValueSubject를 구독한다면, 구독하는 순간에 Subject가 가지고 있던 값을 즉시 받을 수 있다.


## ConnectablePublisher

Publisher에는 두 가지 종류가 있다. 구독이 일어나야만 실제로 동작하는 일반 Publsiher와 [connect()](https://developer.apple.com/documentation/combine/connectablepublisher/connect())을 명시적으로 호출해줘야 동작하는 [ConnectablePublisher](https://developer.apple.com/documentation/combine/connectablepublisher)이다. ConnectablePublisher는 구독 여부와 상관없이 자신의 동작을 수행할 수 있다. ConnectablePublisher는 구독자 여부에 관계없이 동작한다는 점 때문에 시간과 관련된 작업에서 종종 사용된다.


---

## 참고자료

https://jcsoohwancho.github.io/2020-01-23-Combine-시작하기(5)-기타/
