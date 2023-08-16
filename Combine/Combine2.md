# Combine 시작하기(2)-Publisher

## Publisher 프로토콜

모든 Publisher는 [Publisher 프로토콜](https://developer.apple.com/documentation/combine/publisher)을 따라야 한다. 이 Publishers는 3가지 요소를 필수적으로 정의해야 한다.

- associatedtype Output: Publisher가 내보내는 값의 타입이다.
- associatedtype Failure: Publisher가 내보내는 에러의 타입이다. Error 프로토콜을 채택한 타입이어야 한다.
- [receive(subscriber:)](https://developer.apple.com/documentation/combine/publisher/receive(subscriber:)): 해당 Publisher에 인자로 주어지는 Subscriber를 전달한다. Subscriber는 Publisher와 타입이 일치해야만 한다. receive라는 단어는 Publisher의 입장에서 쓰인 의미이기 때문에, 사용자가 호출할 때는 [subscribe(_:)](https://developer.apple.com/documentation/combine/publisher/subscribe(_:)-4u8kn)메소드를 통해서 사용하게 된다. <br> 내부적으로는 Publisher는 자신이 사용하는 [Subscription](https://developer.apple.com/documentation/combine/subscription) 객체를 만들어서 Subscriber에게 전달을 하게 되는데, 이 Subscription 객체가 실질적인 작업과 값 전달을 수행하게 된다. 즉, Publisher는 Subscription 객체의 팩토리 객체라고 볼 수 있다.


Publisher 프로토콜은 또한 익스텐션으로 많은 Operator들을 정의하고 있다.


## Publisher 만들기

Apple은 Publisher를 커스텀해서 만드는 것을 권장하지 않는다. 실제로 Publisher 프로토콜을 구현하기 위해서 신경쓸 부분도 있고, 그에 맞는 Subscription 타입 또한 구현을 제대로 하려면 신경 써야 할 부분이 있기 때문이다. 따라서 다음과 같은 방법을 권장한다.

- [Subject](https://developer.apple.com/documentation/combine/subject)의 구체 타입인 [PassthroughSubject](https://developer.apple.com/documentation/combine/passthroughsubject)와 [CurrentValueSubject](https://developer.apple.com/documentation/combine/currentvaluesubject)를 활용한다. 이 Subject는 외부에서 값을 손쉽게 주입할 수 있는 Publisher의 일종이다.
- 클래스의 Property인 경우, [@Published](https://developer.apple.com/documentation/combine/published)프로퍼티 래퍼를 적용하면, 값이 변화할 때 마다 변한 값이 Subscriber들에게 전파된다.



## Publisher 사용하기

Apple은 여러가지 간편하게 사용할 수 있는 Publisher 타입을 제공하고 있다.

- [Just](https://developer.apple.com/documentation/combine/just): 하나의 값을 즉시 동기적으로 발생시키고자 할 때 사용하는 Publisher이다.
- [Future](https://developer.apple.com/documentation/combine/future): 하나의 값을 비동기적으로 발생시키고자 할 때 사용하는 Publisher이다.
- [Deferred](https://developer.apple.com/documentation/combine/deferred): 구독이 일어나기 전까지 어떤 Publisher가 사용될 지 모르다가 구독이 일어나야지만 실제 사용될 Publisher가 결정되는 Publisher이다. 외부 조건에 의해 Publisher가 달라져야 할 때 유용하다.
- [Empty](https://developer.apple.com/documentation/combine/empty): 아무런 값을 발생시키지 않는 Publisher이다. 즉시 정상 종료할 수도 있고, 영원히 종료하지 않도록 설정할 수도 있다.
- [Fail](https://developer.apple.com/documentation/combine/fail): 즉시 에러를 발생시키고 종료되는 Publisher이다.
- [Record](https://developer.apple.com/documentation/combine/record): 구독할 때마다 이전에 발생했던 값들을 다시 내보내는 Publisher이다.

또한 Apple은 몇 가지 동기 혹은 비동기 인터페이스에 추가적인 Publisher 프로퍼티를 제공한다.

```swift
// 동기 인터페이스의 publisher 형태
let syncSub = [1,2,3,4]
          .publisher
          .sink { print($0) }

// 비동기 인터페이스의 publisher 형태
let asyncSub = NotificationCenter.default
  .publisher(for: NSControl.textDidChangeNotification, object: filterField)
  .sink(receiveCompletion: { print ($0) },
        receiveValue: { print ($0) })
```



---

## 참고자료

https://developer.apple.com/documentation/combine/publisher

https://developer.apple.com/documentation/combine/receiving-and-handling-events-with-combine

https://jcsoohwancho.github.io/2020-01-19-Combine-시작하기(2)-Publisher/