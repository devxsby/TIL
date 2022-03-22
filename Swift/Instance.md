# 인스턴스(Instance) 생성 및 소멸

## 1. 인스턴스 생성

* 초기화 과정 : 새로운 인스턴스 사용 준비를 위해 저장 프로퍼티의 초기값을 설정

* 초기화 구현 : **이니셜라이저(Initializer)** 정의
  - 구현된 이니셜라이저는 새로운 인스턴스를 생성할 수 있는 특별한 메서드가 됨
  - 스위프트의 이니셜라이저는 반환값이 없음
  - just 인스턴스의 첫 사용을 위해 초기화 하는 것 !
  - `init` 키워드를 사용

`클래스, 구조체, 열거형의 기본적 형태의 이니셜라이저 예시`

```swift
class SomeClass {
    init() {
        // 초기화 시 필요한 코드
    }
}

struct SomeStruct {
    init() {
        // 초기화 시 필요한 코드
    }
}

enum SomeEnum {
    case someCase
    
    init() {
        // 열거형은 초기화 시 반드시 케이스중 하나가 되어야 한다.
        self = .someCase
        // 초기화 시 필요한 코드
    }
}
```

<br>

### 1.1 프로퍼티 기본값

* 구조체, 클래스 인스턴스는 처음 생성할 때 옵셔널 제외하고 다 **초깃값(Initial Value)**을 할당해야 함

* 프로퍼티를 정의할 때 프로퍼티 **기본값(Default Value)**을 할당하면 init()에서 초깃값을 설정하지 않아도 프로퍼티 기본값으로 자동 초기화 됨

```swift
// 초깃값을 설정하는 방법 1 : init 이니셜라이저
struct Area {
    var squareMeter1: Double
    
    init() {
    	squareMeter1 = 0.0 
    }
    
 // 초깃값을 설정하는 방법 2 : 프로퍼티 기본값 지정
  
    var squareMeter2: Double = 0.0

}

// 호출부
let room: Area = Area()
print(room.squareMeter1) // 0.0
print(room.squareMeter2) // 0.0
```

<br>

### 1.2 이니셜라이저 매개변수

* 함수나 메서드처럼 이니셜라이저도 매개변수를 가질 수 있음

* 즉, 인스턴스를 초기화하는 과정에 필요한 값을 전달받을 수 있음

```swift
struct Area {
    var squareMeter: Double
    
    init(fromPy py: Double) { // 첫 번째 이니셜라이저
        squareMeter = py * 3.3058 
    }
    
    init(fromSquareMeter squareMeter: Double) { // 두 번째 이니셜라이저
        self.squareMeter = squareMeter
    }
    
    init(value: Double) { // 세 번째 이니셜라이저
        squareMeter = value
    }
    
    init(_ value: Double) { // 네 번째 이니셜라이저
        squareMeter = value
    }
}

let roomOne: Area = Area(fromPy: 15.0)
print(roomOne.squareMeter) // 49.587

let roomTwo: Area = Area(fromSquareMeter: 33.06)
print(roomTwo.squareMeter) // 33.06

let roomThree: Area = Area(value: 30.0)
print(roomThree.squareMeter) // 30.0

let roomFour: Area = Area(55.0)
print(roomFour.squareMeter) // 55.0

Area() // 오류 발생 !
```
* 사용자 정의 이니셜라이저를 만들면 기존의 기본 이니셜라이저(`init()`)은 별도로 구현하지 않는 이상 사용할 수 없음

* 두 번째 이니셜라이저는 self 프로퍼티를 사용함

* 세 번째 이니셜라이저는 따로 전달인자 레이블을 사용하지 않음

* 네 번째 이니셜라이저는 와일드카드 식별자(`_`)를 사용하여 전달인자 레이블을 없앰

<br>

### 1.3 옵셔널 프로퍼티 타입

* 인스턴스가 사용되는 동안에 꼭 갖지 않아도 되는 저장 프로퍼티가 있거나 초기화 과정에서 값을 지정해주기 어려운 경우, 해당 프로퍼티를 옵셔널로 선언할 수 있음

* 옵셔널로 선언한 저장 프로퍼티는 초기화 과정에서 값을 할당해주지 않으면 자동으로 `nil` 할당됨

```swift
class Person {
    var name: String
    var age: Int? // 옵셔널 선언
    
    init(name: String) {
        self.name = name
    }
}

let avery: Person = Person(name: "Avery")
print(avery.name) // Avery
print(avery.age)  // 이니셜 라이저에서 초기화 안했으므로 자동 nil할당

avery.age = 20   // 값 입력하면
print(avery.age) // Optional(20)

if let age = avery.age {
    print(age) // 20
}

avery.name = "Ava" // 이름 변경
print(avery.name)  // Ava
```

<br>

### 1.4 상수 프로퍼티


<br>

### 1.5 기본 이니셜라이저와 멤버와이즈 이니셜라이저
### 1.6 초기화 위임
### 1.6 실패 가능한 이니셜라이저
### 함수를 사용한 프로퍼티 기본값 설정

---

## 2. 인스턴스 소멸

<br>

--- 
### 참고 자료

[인스턴스의 생성과 소멸](https://yagom.github.io/swift_basic/contents/15_init_deinit/)
