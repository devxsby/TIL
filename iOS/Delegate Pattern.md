# Delegation 이란?
* 딜리게이션이란 일부 클래스의 책임을 다른 클래스의 인스턴스에게 위임 또는 전달할 수 있는 디자인패턴을 의미한다.
  
* `예시` 
  
  당신과 내가 초콜릿 쿠키를 만들어 전달하는 팀의 일원이라고 상상해보자.
  
  당신이 쿠키 제빵을 담당하여 이를 위해 필요한 '쿠키 반죽 만들기' 일을 나에게 **위임(delegate)** 한다.

  내가 도우를 만들고 이를 당신에게 전달해주면 당신은 그것을 이용해서 쿠키를 만들 수 있다.

  이때 몇가지 핵심 사항에 대해 알아보자.

  - 당신은 쿠키를 만드는 작업을 맡았기에 나에게 '쿠키 도우 만들기' 업무를 위임한다.
  - 나는 위임된 작업(쿠키 반죽 도우 만들기)을 마치면 쿠키 반죽을 당신에게 제공한다.

---

## Delegation 예제 코드

* Cookie 구조체


```swift
struct Cookie {
    var size: Int = 5
    var hasChocoChips: Bool = false
}
```

* Bakery 클래스
```swift
class Bakery {
    func makeCooke() {
        cookie.size = 6
        cookie.hasChocoChips = true
    }
}
```

위 코드를 살펴보면 Bakery라는 클래스는 내부에서 Cookie 구조체를 사용하여 cookie를 생성하고 일부 속성을 설정하는 makeCookie()라는 함수를 가지고 있다.

이 시점에서 우리는 쿠키를 세가지 방식으로 판매하려고 한다.

- 오프라인 베이커리 샵에서 판매
- 온라인 베이커리 웹 사이트에서 판매
- 유통 업체 도매를 통한 판매

쿠키를 판매하는 것은 우리의 책임이 아니지만, 쿠키를 만들어 전달하는 것까지는 우리의 책임이다. 

따라서 쿠키를 만들고 나서 전달하는 방법이 필요하다.

여기서 **Delegation**개념이 이용된다.
---

첫 번째, 우리는 **Protocol**을 정의해야 한다. 프로토콜 내에는 쿠키를 판매처에 전달하는 책임의 내용들을 캡슐화한다.

BakeryDelegate 프로토콜 안에 cookieWasBaked라는 함수가 선언되고, 이 delegate 함수(=cookieWasBaked)는 쿠키가 구워질 때 마다 호출된다.

```swift
protocol BakeryDelegate {
    func cookieWasBaked(_ cookie: Cookie)
}
```

두 번째, 위 delegate를 클래스 내부에 통합한다.
```swift
class Bakery {
    var delegate: BakeryDelegate?

    func makeCooke() {
        var cookie = Cookie()
        cooke.size = 6
        cookie.hasChocoChips = true

        delegate?.cookieWasBaked(cookie)
    }
}
```

기존 Bakery 클래스에서 두 가지가 추가되었다.

1. BakeryDelegate 타입의 delegate 프로퍼티가 추가되었다. 
2. 함수 cookieWasBaked()가 makeCookie() 함수 내부 delegate에서 호출된다.

# Delegate Pattern 사용하는 이유는?
* iOS 개발 패턴 중 가장 중요한 패턴이다. Apple framework를 사용하기 위해서 반드시 알아야 할 패턴이다.

* iOS에서 프로토콜을 사용하여 delegation을 구현하는 것은 **클래스가 단일 상속만을 지원하기 때문**이다. 

* 즉, 하나의 부모 클래스를 상속받고 나면 더는 다른 클래스를 상속받을 수 없으므로 기능을 덧붙이기 제한적이다. 이를 극복하기 위해 구현 개수에 제한이 없는 프로토콜을 이용하여 필요한 단위별 객체를 작성한다.

* 결론적으로, 개발의 유연성이 증가한다 !!

---

### 참고 자료

[Using Delegates to Customize Object Behavior](https://developer.apple.com/documentation/swift/cocoa_design_patterns/using_delegates_to_customize_object_behavior)

[Delegation in Swift Explained](https://www.appypie.com/delegation-swift-how-to)