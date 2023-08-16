# Combine 시작하기(1)-Overview

## Combine은 무엇인가?

Apple은 Combine에 대해 이렇게 정의했다.

> A Unified, declarative API for processing values over time 시간에 따른 값들을 처리하기 위한 통합적이고 선언적인 API

- Unified: Apple은 기존 비동기 인터페이스로 Target/action, Notification Center, URLSession, KVO, 콜백 등의 예시를 들면서 Combine이 이들을 대체하는 게 아니라, 이들을 같은 형태의 인터페이스로 사용할 수 있게 만들어준다는 점에서 통합적(Unified)라는 표현을 사용했다. (다만 Target/Action을 SwiftUI를 사용하지 않은 채로 Combine으로 대체할 수 있는진 아직 발견x)
- Declarative: 반복문, 조건문 등의 제어 흐름(Control Flow)을 직접 명시하지 않고 프로그램이 수행해야 할 로직들을 차례대로 연결하여 프로그래밍을 하는 방식이다. 여기서 제어 흐름은 map, filter 등의 기본 연산들(primitives)에 의해 가려지게 된다.
- Processing values over time: 동작에 따라 값을 한꺼번에 받지 않고, 시간 간격에 따라 비동기적으로 받게 된다는 뜻이다.

## Combine의 원리

Combine을 구성하는 요소들은 다음과 같다.

- Publisher: 값과 에러를 만들어내는 방법이 정의되어 있는 객체이다. Publisher는 프로토콜이지만, 이 프로토콜을 채택하는 타입은 강제하는 부분은 없지만 값타입(struct)이 적절하다.
- Subscriber: Publisher로부터 값을 받아서 사용하는 객체이다. Subscriber는 identity가 있어야 되기 때문에 참조 타입(class)으로 선언되어야 한다.
- Operator: Publisher의 일종으로, 다른 Publisher에서 나온 값을 받아 처리하는 데 특화되어 있는 Publisher이다. Operator는 직접 만드는 경우는 없고, Publisher의 인스턴스 메소드를 통해서 만들도록 캡슐화되어 있다. 이렇게 Operator를 여러 개 연결해서 원하는 로직을 처리하도록 만드는 게 Combine을 활용한 프로그램의 핵심이다.


Combine의 동작 과정은 다음과 같다.

<p align="center"><img src="https://miro.medium.com/v2/resize:fit:1400/0*BTHSzBkPQot2l2WP"></p>


1. Publisher와 Subscriber를 만들고, Subscriber는 Publisherd의 [Subscribe(_)](https://developer.apple.com/documentation/combine/publisher/subscribe(_:)-4u8kn) 메소드를 통해 Publisher를 구독한다.
2. Publisher는 연산 방법과 연산에 필요한 상태를 캡슐화한 Subscription 객체를 만들어서 Subscriber에 보낸다.
3. Subscriber는 Subscription에 자신이 원하는 이벤트의 수를 의미하는 Demand를 만들어서 Subscription에 보낸다.
4. Subscription은 Demand를 기반으로 적절한 양의 값 또는 완료 이벤트를 보낸다. Demand를 어떻게 사용하는 지는 완전히 Subscription의 재량으로, 정확한 수의 값을 보낼 수도 있고 그보다 적은 수, 혹은 많은 수를 보낼 수도 있다. (애플의 API는 요청량에 맞게 보내주고, 더 이상 보낼 수 없으면 완료 이벤트를 보내도록 되어)
5. Subscription은 필요한 때에 Subscriber에 완료 이벤트를 보낸다.
6. 완료 이벤트를 받은 Subscriber는 작업을 마무리하기 위한 추가 작업이 필요하면 한다.


---

## 참고자료

https://developer.apple.com/wwdc19/722

https://jcsoohwancho.github.io/2020-01-18-Combine-시작하기(1)-Overview/
