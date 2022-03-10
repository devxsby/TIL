# Optionals (옵셔널) 이란?

* 옵셔널은 [스위프트의 언어적 특성](./Swift/../스위프트의%20언어적%20특성.md)에서 언급한 특징 중 하나인 안전성(Safe)을 문법으로 담보하는 기능이다.
  
* C언어 또는 Objective-C에서는 찾아볼 수 없는 콘셉트이다.

* 옵셔널은 단어 뜻 그대로 '선택적인', 즉 값이 '있을 수도, 없을 수도 있음'을 나타내는 표현이다. 이는 변수나 상수 등에 꼭 값이 있다는 것을 보장할 수 없다. 즉, 변수 또는 상수의 값이 nil일 수도 있다는 것을 의미한다.
  
* 스위프트에서는 옵셔널 하나만으로도 옵셔널이 아닌 값과 철저히 다른 타입으로 인식하기 때문에 컴파틸할 때 바로 오류를 걸러낼 수 있다.
  
<br>

# 옵셔널 사용

그 전에 기본적으로 데이터 타입 중 nil과 그 친구들에 대해 간단히 알아보겠다.

* **Any, AnyObject와 nil 그리고 Never**
  - **Any**는 스위프트의 모든 데이터 타입을 사용할 수 있다는 뜻이다. 변수 또는 상수의 데이터 타입이 Any로 지정되어 있다면 그 변수 또는 상수에는 어떤 종류의 데이터 타입이든지 상관없이 할당할 수 있다.
  - **AnyObject**는 Any보다는 조금 한정된 의미로 클래스의 인스턴스만 할당할 수 있다. `클래스에 대한 내용은 구조체와 클래스에서 추후 업데이트`
  - **nil**은 특정 타입이 아니라 '없음'을 나타내는 스위프트의 키워드이다. 즉, 변수 또는 상수에 값이 들어있지 않고 비어있음을 나타내는 데 사용한다. 
  - **Never**는 특정 함수의 반환타입으로 사용될 수 있다. `종료되지 않는 함수에서 추후 업데이트`

* 변수 또는 상수에 값이 없는 경우, 즉 nil이면 해당 변수 또는 상수에 접근했을 때 잘못된 메모리 접근(memory access)로 런타임 오류가 발생한다. 아래는 오류가 발생하는 nil할당의 예시이다. nil은 옵셔널로 선언된 곳에만 사용할 수 있다. 
  
  ```swift
  var myName: String = "Ava"
  myName = nil  // 컴파일 에러 발생 
  ```

* 옵셔널 변수 또는 상수 등은 데이터 타입 뒤에 물음표(?)를 붙여 표현한다.
  
  ```swift
  var myName: String? = "Ava"
  print(myName)  // Ava

  myName = nil
  print(myName)  // nil
  ```
---
<br>

## 그렇다면 옵셔널은 어떤 경우에 사용할까?

* 옵셔널을 유용하게 사용할 수 있는 예로는 우리가 만든 함수에 전달되는 전달인자의 값이 잘못된 경우일 경우 제대로 처리하지 못했음을 nil로 반환하여 표현하는 것을 들 수 있다.

* 아래는 switch 구문을 통한 옵셔널 값의 확인 예제이다.


```swift
func checkOptionalValue(value optionalValue: Any?) {
    switch optionalValue {
        case .none:
        print("This Optional value is nil.")
        case .some(let value):
        print("Value is \(value).")
    }
}
var myName: String? = "Ava"
checkOptionalValue(value: myName)  // Value is Ava.

myName = nil
checkOptionalValue(value: myName)  // This Optional value is nil. 
```

<br>

# 옵셔널 추출

* 옵셔널을 값을 가지고 오고 싶을 때는 어떻게 해야 할까?
* 열거형의 some 케이스로 꼭꼭 숨어있는 **옵셔널의 값을 옵셔널이 아닌 값으로 추출**하는 옵셔널 추출(Optional Unsrapping)방법에 알아보자.

<br>

## 1. 강제 추출

* 옵셔널 강제 추출(Forced Unwrapping)방식은 옵셔널의 값을 추출하는 가장 간단하지만 **가장 위험한 방법**이다. 런타임 오류가 일어날 가능성이 가장 높으며, 옵셔널을 만든 의미가 사라지기 때문이다.

* 옵셔널의 값을 강제 추출하려면 옵셔널 값의 뒤에 느낌표(!)를 붙여주면 된다. 강제 추출 시 옵셔널에 값이 없다면, 즉 nil이면 런타임 오류가 발생한다.
  
```swift
var myName: String? = "Ava"
var yagom: String = myName!     // 옵셔널이 아닌 변수에는 옵셔널 값이 들어갈 수 없다.
// 추출해서 할당해주어야 한다.

myName = nil
yagom = myName!     // 런타임 오류

// if 구문 등 조건문을 이용해서 조금 더 안전하게 처리해 보자
if myName != nil {
    print("My name is \(myName!).")
} else {
    print("myName == nil")
}
// myName == nil
```
<br>

## 2. 옵셔널 바인딩

* 옵셔널 바인딩(Optional Binding)은 옵셔널에 값이 있는지 확인할 때 사용한다. 만약 옵셔널에 값이 있다면 옵셔널에서 추출한 값을 일정 블록 안에서 사용할 수 있는 상수나 변수로 할당해서 옵셔널이 아닌 형태로 사용할 수 있도록 해준다.
  
* 옵셔널 바인딩은 if 또는 while 구문 등과 결합하여 사용할 수 있다.
  
```swift
var myName: String? = "Ava"

// 옵셔널 바인딩을 통한 임시 상수 할당
if let name = myName {
    print("My name is \(name).")
} else {
    print("myName == nil")
}
// My name is Ava.

// 옵셔널 바인딩을 통한 임시 변수 할당
if var name = myName {
    name = "Eve"   // 변수이므로 내부에서 변경이 가능함
    print("My name is \(name).")
} else {
    print("myName == nil")
}
// My name is Eve.
```
* 옵셔널 바인딩을 통해 한 번에 여러 옵셔널의 값을 추출할 수도 있다. 쉼표(,)를 사용해 바인딩 할 옵셔널을 나열하면 된다.
  
* 단, 바인딩하려는 옵셔널 중 하나라도 값이 없다면 해당 블록 내부의 명령문은 실행되지 않는다.
  
```swift
myName = "Ava"
var yourName: String? = nil

// friend에 바인딩이 되지 않으므로 실행되지 않는다.
if let name = myName, let friend = yourName {
    print("We are friend!")
}

yourName = "Eve"

if let name = myName, let friend = yourName {
    print("We are friend! \(name) & \(friend).")
}
// We are friend! Ava & Eve.
```

* 옵셔널 바인딩은 옵셔널 체이닝과 환상적인 조합을 이룬다. `옵셔널 체이닝은 추후 업데이트`

<br>

## 3. 암시적 추출 옵셔널

* 때때로 nil을 할당하고 싶지만, 옵셔널 바인딩으로 매번 값을 추출하기 귀찮거나 등... nil을 할당해줄 수 있는 옵셔널이 아닌 변수나 상수가 있으면 좋을 것이다. 이때 사용하는 것이 암시적 추출 옵셔널(Implicitly Unwrapped Optionals)이다.
  
* 옵셔널을 표시하고자 타입 뒤에 물음표(?)를 사용했지만, 암시적 추출 옵셔널을 사용하려면 타입 뒤에 느낌표(!)를 붙이면 된다.
  
* 암시적 추출 옵셔널로 지정된 타입은 일반 값처럼 사용할 수 있으나, 여전히 옵셔널이기 때문에 nil로 할당해줄 수 있다. 그러나 nil이 할당되어 있을 때 사용하면 오류가 발생한다. 아래의 코드를 보자.

```swift
var myName: String! = "Ava"

print(myName)   // Ava

myName = nil

// 암시적 추출 옵셔널도 옵셔널이므로 당연히 바인딩을 사용할 수 있다.
if var name = myName {
    print("My name is \(name).")
} else {
    print("myName == nil")
}
// myName == nil

print(myName)   // 오류!!
```

결론 !
--- 
  옵셔널을 사용할 때는 강제 추출 또는 암시적 추출 옵셔널을 사용하기 보다는 옵셔널 바인딩, nil 병합 연산자를 비롯해 옵셔널 체이닝 등의 방법을 사용하는 것이 훨씬 안전하며, 스위프트 언어의 지향점에 부합한다.

<br>

--- 
<br>

### 참고 자료
[Optional](https://developer.apple.com/documentation/swift/optional)

[Optional Changing](https://docs.swift.org/swift-book/LanguageGuide/OptionalChaining.html)

[What is an "unwrapped value" in Swift?](https://stackoverflow.com/questions/24034483/what-is-an-unwrapped-value-in-swift)