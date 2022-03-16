# 프로토콜 (Protocol)

## 1. 프로토콜이란?

프로토콜이란 특정 객체가 갖추어야 할 기능이나 속성에 대한 설계도라고 할 수 있다. 협업하여 프로그램을 개발할 때 다른 프로그래머에게 일정 부분의 개발을 맡길 경우 무작정 맡기지 않고 '이러 이러한 것들이 필요하다'라는 설계도를 넘겨줄 때 사용한다. 이렇게 프로토콜을 사용해야만 추후 프로그램을 합치는 과정에서 문제가 생기지 않는다.

예를 들어 다른 프로그래머에게 클래스가 필요한데 이런 기능을 하는 함수여야 한다며 프로토콜로 만들어 전달하면, 프로그래머는 그 프로토콜을 상속받아 코딩하면 된다. 이때 상속받은 프로토콜에서 정의된 내용을 빠뜨리면 에러가 발생하므로 누락되지 않도록 해야 한다. 

프로토콜은 실질적으로 아무런 내용이 없다. 단순히 선언 형태만을 정의한다. 실질적인 내용은 이 프로토콜을 사용하는 객체에서 정의한다.

* 키워드 : protocol

* **프로토콜 명명법** : 클래스, 구조체와 마찬가지로 첫글자를 대문자로 시작하는 `UpperCamelCase`를 사용한다. 
  
```swift
protocol 프로토콜이름 {
    프로토콜 정의
}
```

* 구조체, 클래스, 열거형 등에서 프로토콜을 채택하려면 타입 뒤에 콜론(:)을 붙여준 후 채택할 프로토콜 이름을 쉼표(,)로 구분하여 명시한다. 

```swift
struct SomeStruct: AProtocol, AnotherProtocol {
    // 구조체 정의
}

class SomeClass: AProtocol, AnotherProtocol {
    // 클래스 정의
}

enum SomeEnum: AProtocol, AnotherProtocol {
    // 열거형 정의
}
```

* 위 코드의 각 타입은 AProtocol과 AnotherProtocol을 채택한 것이다. 만약, 클래스가 다른 클래스를 상속받는다면 상속받을 클래스 이름 다음에 채택할 프로토콜을 나열한다. 

```swift
class SomeClass: SuperClass, AProtocol, AnotherProtocol {
    // 클래스 정의
}
```

* 위의 SomeClass는 SuperClass를 상속받았으며 동시에 AProtocol과 AnotherProtocol 프로토콜을 채택한 클래스이다. 

`예시`

계산기를 만들어 볼 때, 덧셈과 뺄셈이 꼭 있어야 한다면 아래처럼 프로토콜을 만들 수 있다.

```swift
protocol CalculatorProtocol
    func add(op1 : Int, op2 : Int) -> Int
    func sub(op1 : Int, op2 : Int) -> Int
```

이렇게 계산기 프로토콜을 만들었다면 이 계산기 프로토콜을 상속받은 클래스는 반드시 add 함수와 sub 함수를 만들어야 한다. 그렇지 않으면 에러가 발생한다.

```swift
class SimpleCalculator : CalculatorProtocol {
    func add(op1 : Int, op2 : Int) -> Int {
        return op1 + op2
    }
    func sub(op1 : Int, op2 : Int) -> Int {
        return op1 - op2
    }
}
```

<br>

## 2. 프로토콜 요구사항

`업데이트 예정`

<br>

--- 
### 참고 자료

[Swift – 프로토콜, 익스텐션](https://blog.yagom.net/529/)

[Swift – 프로토콜 지향 프로그래밍](https://blog.yagom.net/531/)

[도서 : The 친절한 Swift 프로그래밍 Zero(V 4.0)](http://www.kyobobook.co.kr/product/detailViewKor.laf?barcode=9791195418909)