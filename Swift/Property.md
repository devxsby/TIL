# 프로퍼티(Property)와 메서드(Method)

## 1. 프로퍼티 (Property)

클래스와 구조체에서 값을 **제공**하는 프로퍼티(Property)에 대해 알아보자. 

프로퍼티를 한글로 '속성'이라는 뜻으로, 값을 저장하기 위한 목적으로 클래스와 구조체, 열거형 내에서 정의된 변수나 상수이다.

그러나 이는 프로퍼티의 역할 중 일부에 불과하고, 프로퍼티가 클래스, 구조체, 열거형에서 하는 정확한 역할은 값을 제공하는 것이다.

프로퍼티(Property)는 값에 대한 저장 여부를 기준으로 두 가지 종류로 나눌 수 있다.

`저장 프로퍼티`

- 입력된 값을 저장하거나 저장된 값을 제공하는 역할
- 상수 및 변수를 사용해서 정의 가능
- 클래스와 구조체에서는 사용 가능하나, 열거형에서는 사용 불가

`연산 프로퍼티`

- 특정 연산을 통해 값을 만들어 제공하는 역할
- 변수만 사용해서 정의 가능
- 클래스, 구조체, 열거형 모두에서 사용 가능

저장 프로퍼티와 연산 프로퍼티는 대체로 클래스나 구조체를 바탕으로 만들어진 개별 인스턴스에 소속되어 있는 값을 저장하거나 연산 처리하는 역할을 한다.

따라서 프로퍼티를 사용하려면 인스턴스가 필요하다. 인스턴스를 생성한 후 이 인스턴스를 통해 프로퍼티를 참조하거나 값을 할당해야 한다. 이렇게 인스턴스에 소속되는 프로퍼티를 **인스턴스 프로퍼티**라고 한다.

예외적으로 일부 프로퍼티는 클래스와 구조체 자체에 소속되어 있는 값을 가지기도 한다. 이런 프로퍼티들을 **타입 프로퍼티(Type Property)**라고 한다. 타입 프로퍼티는 인스턴스를 생성하지 않아도 사용 가능하다.

프로퍼티를 작성할 때에는 위치가 중요하다. 클래스 정의 구문 내부에 작성되어야 하지만, 메소드 내부에 작성되면 안되기 때문이다. 메소드 내부에서도 변수나 상수를 사용하여 값을 저장할 수 있는데, 이는 프로퍼티가 아닌 지역변수에 불과하다. **따라서 프로퍼티는 클래스의 내부에, 그리고 메소드의 외부에 정의해야 한다.**

더불어 스위프트에서는 프로퍼티 값을 모니터링하기 위해 **프로퍼티 옵저버(Property Observer)**를 정의하여 사용자가 정의한 특정 액션과 반응하도록 처리할 수 있다. 즉, 프로퍼티의 값이 변하는 것을 감시하고, 프로퍼티의 값이 변할 때 값의 변화에 따른 특정 작업을 실행한다. 프로퍼티 옵저버는 우리가 직접 정의한 프로퍼티에 추가할 수 있으며 부모 클래스로 부터 상속받을 수도 있다.

<br>

### 1.1 저장 프로퍼티 (Stored Property)

**저장 프로퍼티(Stored Property)**는 클래스 또는 구조체의 인스턴스와 연관된 값을 저장하는 가장 단순한 개념의 프로퍼티이다.

저장 프로퍼티는 `var` 키워드를 사용하면 변수 저장 프로퍼티, `let`키워드를 사용하면 상수 저장 프로퍼티가 된다.

일반 변수나 상수를 선언할 때 초기값을 할당할 수 있는 것처럼 저장 프로퍼티를 선언할때에도 초기값을 할당할 수 있다. 하지만 반드시 선언하는 시점에서 초기값을 할당해야 하는 것은 아니고, 초기화 구문을 통해 설정해도 된다. 구조체의 멤버와이즈 구문이 이같은 역할을 한다.

하지만 클래스에서는 프로퍼티를 선언할 때 초기값을 함께 함께 할당해주지 않으면 신경써야 할 것들이 있어 주의해야 한다. 프로퍼티 선언시 초기값이 할당되지 않은 저장 프로퍼티는 반드시 **옵셔널 타입**으로 선언해 주어야 한다. 이는 스위프트에서 클래스의 프로퍼티에 값이 비어있으면 인스턴스를 생성할 때 무조건 nil값으로 초기화하기 때문이다.

옵셔널 타입으로 프로퍼티를 선언할 때에는 일반 옵셔널 타입과 묵시적 옵셔널 해제 타입 중에서 선택해서 정의할 수 있다. 묵시적 옵셔널 타입으로 지정해두면 이 값을 사용할 때 옵셔널 해제 처리를 할 필요 없이 일반 변수처럼 쓸 수 있다.

아래는 아주 기본적인 저장 프로퍼티의 선언과 인스턴스 초기화 방법의 예시이다.

```swift
// 좌표
struct CoordinatePoint {
    var x: Int      // 저장 프로퍼티
    var y: Int      // 저장 프로퍼티
}

// 구조체는 기본적으로 저장 프로퍼티를 매개변수로 가지는 이니셜라이저가 있다.
let averyPoint: CoordinatePoint = CoordinatePoint(x: 20, y: 10)


// 사람의 위치정보
class Position {
    var point: CoordinatePoint  // 저장 프로퍼티 (변수) – 위치(point)는 변경될 수 있음.
    let name: String            // 저장 프로퍼티 (상수)
    
    // 프로퍼티 기본값을 지정해주지 않으면 이니셜라이저를 따로 정의해주어야 한다.
    init(name: String, currentPoint: CoordinatePoint) {
        self.name = name
        self.point = currentPoint
    }
}

// 사용자정의 이니셜라이저를 호출해야 한다.
// 그렇지 않으면 프로퍼티 초깃값을 할당할 수 없기 때문에 인스턴스 생성이 불가능하다.
let averyPosition: Position = Position(name: "avery", currentPoint: averyPoint)
```

위에서 구조체는 프로퍼티에 맞는 이니셜라이저를 자동으로 제공한다고 했지만 클래스는 초기값을 따로 지정해야 나중에 사용자 정의 이니셜라이저를 구현하지 않아도 된다고 했다. 아래는 그 예시 코드이다.

```swift
// 좌표
struct CoordinatePoint {
    var x: Int = 0      // 저장 프로퍼티
    var y: Int = 0      // 저장 프로퍼티
}

// 프로퍼티의 초깃값을 할당했다면 굳이 전달인자로 초깃값을 넘길 필요가 없다.
let averyPoint: CoordinatePoint = CoordinatePoint()
// 물론 기존에 초깃값을 할당할 수 있는 이니셜라이저도 사용 가능합니다.
let oliviaPoint: CoordinatePoint = CoordinatePoint(x: 30, y: 15)

print("avery's point : \(averyPoint.x), \(averyPoint.y)")       // avery's point : 0, 0
print("olivia's point : \(oliviaPoint.x), \(oliviaPoint.y)") // wizplan's point : 30, 15


// 사람의 위치정보
class Position {
    var point: CoordinatePoint = CoordinatePoint()  // 저장 프로퍼티
    var name: String = "Unknown"                    // 저장 프로퍼티
}

// 초깃값을 지정해줬다면 사용자정의 이니셜라이저를 사용하지 않아도 된다.
let averyPosition: Position = Position()

averyPosition.point = averyPoint
averyPosition.name = "avery"
```

다음은 옵셔널과 사용자 정의 이니셜라이저를 혼합한 예제이다. 이렇게하면 다른 프로그래머가 사용할 때, 내가 처음 의도한 대로 구조체와 클래스를 사용하도록 유도할 수 있다.

```swift
// 좌표
struct CoordinatePoint {
    // 위치는 x, y 값을 모두 가져야 하므로 옵셔널이면 안된다.
    var x: Int
    var y: Int
}

// 사람의 위치정보
class Position {
    var point: CoordinatePoint?     // 현재 사람의 위치를 모를 수도 있다 -> 옵셔널
    let name: String
    
    init(name: String) {
        self.name = name
    }
}

// 이름은 필수지만 위치는 모를 수 있다.
let averyPosition: Position = Position(name: "avery")

// 위치를 알게되면 그 때 위치 값을 할당한다.
averyPosition.point = CoordinatePoint(x: 20, y: 10)
```

<br>

#### 1.1.1 지연 저장 프로퍼티 (Lazy Stored Property)

일반적으로 저장 프로퍼티는 클래스 인스턴스가 처음 생성될 때 함께 초기화되지만, 저장 프로퍼티 정의 앞에 Lazy라는 키워드가 붙으면 예외가 된다. 이 키워드는 저장 프로퍼티의 초기화를 지연시킨다.

클래스 인스턴스가 생성되어 모든 저장 프로퍼티가 만들어지더라도 Lazy 키워드가 붙은 프로퍼티는 선언만 될 뿐 초기화되지 않고 계속 대기하고 있다가 프로퍼티가 호출되는 순간에 초기화된다. 

상수는 인스턴스가 완전히 생성되기 전에 초기화해야 하므로 필요할 때 값을 할당하는 지연 저장 프로퍼티와는 맞지 않는다. 따라서 지연 저장 프로퍼티는 `var` 키워드를 사용하여 변수로 정의한다.

지연 저장 프로퍼티는 주로 복잡한 클래스나 구조체를 구현할 때 많이 사용된다.

```swift
struct CoordinatePoint {
    var x: Int = 0
    var y: Int = 0
}

class Position {
    lazy var point: CoordinatePoint = CoordinatePoint()
    let name: String
    
    init(name: String) {
        self.name = name
    }
}

let averyPosition: Position = Position(name: "avery")

// 이 코드를 통해 point 프로퍼티로 처음 접근할 때 point 프로퍼티의 CoordinatePoint가 생성된다.
print(averyPosition.point)  // x: 0, y: 0
```

<br>

### 1.2 연산 프로퍼티 (Compute Property)

연산 프로퍼티Compute Property)는 실제 값을 저장하는 프로퍼티가 아니라, 특정 상태에 따른 값을 연산하는 프로퍼티이다. 

연산 프로퍼티는 필요한 값을 제공한다는 점에서 저장 프로퍼티와 같지만, 실제 값을 저장했다가 반환하지는 않고 대신 다른 프로퍼티의 값을 연산 처리하여 간접적으로 값을 제공한다.

연산 프로퍼티의 정의 형식은 저장 프로퍼티의 형식보다는 오히려 함수와 비슷하다. 연산 프로퍼티를 정의할 때에는 프로퍼티 이름과 타입에 이어서 중괄호로 된 실행 블록을 덧붙이고, 실행 블록 내부에는 접근자로 `get구문`(프로퍼티의 값을 참조할 때)과 설정자로 `set구문`(프로퍼티의 값을 할당하거나 변경하고자 할 때)을 작성한다. 경우에 따라 set구문은 생략할 수 있다. 그리고 내부적으로 return 키워드를 사용해서 값을 반환한다.

연산 프로퍼티는 항상 클래스나 구조체 또는 열거형 내부에서만 사용할 수 있다.

연산 프로퍼티를 사용하면 하나의 프로퍼티에 접근자와 설정자가 모두 모여있고, 해당 프로퍼티가 어떤 역할을 하는 지 좀 더 명확하게 표현할 수 있다.

```swift
struct CoordinatePoint {
    var x: Int  // 저장 프로퍼티
    var y: Int  // 저장 프로퍼티
    
    // 대칭 좌표
    var oppositePoint: CoordinatePoint {    // 연산 프로퍼티
        // 접근자
        get {
            return CoordinatePoint(x: -x, y: -y)
        }
        
        // 설정자
        set(opposite) {
            x = -opposite.x
            y = -opposite.y
        }
    }
}

var averyPosition: CoordinatePoint = CoordinatePoint(x: 10, y: 20)

// 현재 좌표
print(averyPosition)    // 10, 20

// 대칭 좌표
print(averyPosition.oppositePoint)  // -10, -20

// 대칭 좌표를 (15, 10)으로 설정하면
averyPosition.oppositePoint = CoordinatePoint(x: 15, y: 10)

// 현재 좌표는 -15, -10가 된다.
print(averyPosition)    // -15, -10
```

연산 프로퍼티는 사실 메서드 형식으로도 표현할 수 있다. `메서드 설명은 밑에 있음`

인스턴스 외부에서 메서드를 통해 인스턴스 내부 값에 접근하려면 메서드를 두 개(접근자, 설정자) 구현해야 한다. 또한 이를 감수하고 메서드로 구현한다 해도 두 메서드가 분산 구현되어 코드의 가독성이 나빠질 위험이 있다. 타인의 코드를 보는 프로그래머의 입장에서는 프로퍼티가 메서드 형식보다 더 간편하고 직관적으로 느끼게 된다.

그러나 연산 프로퍼티는 접근자인 get 메서드만 구현해둔 것처럼 읽기 전용 상태로 구현하기 쉽지만, 쓰기 전용 상태로 구현할 수 없다는 단점이 있다. 메서드로는 설정자 메서드만 구현하여 쓰기 전용 상태로 구현할 수 있지만 연산 프로퍼티는 이가 불가능하다.

<br>

### 1.3 프로퍼티 옵저버 (Property Observer)

특정 프로퍼티를 계속 관찰하고 있다 프로퍼티의 값이 변경되면 이를 알아차리고 호출되는 구문이 있다. 이를 **프로퍼티 옵저버 (Property Observer)**라 한다.

프로퍼티 옵저버는 우리가 프로퍼티의 값을 직접 변경하거나 시스템에 의해 자동으로 변경하는 경우에 상관없이 일단 프로퍼티에 새 값이 설정(set)되면 무조건 호출된다. 심지어 프로퍼티에 현재와 동일한 값이 재할당되더라도 어김없이 호출된다. 

저장 프로퍼티에 값을 대입하는 구문이 수행되거나 연산 프로퍼티에 set구문이 실행되는 모든 경우에 프로퍼티 옵저버가 호출된다.

프로퍼티 옵저버에는 두가지 종류가 있다.

- `willSet` : 프로퍼티의 값이 변경되기 직전에 호출되는 옵저버
- `didSet` : 프로퍼티의 값이 변경된 직후에 호출되는 옵저버

`willSet`에서는 새 값의 파라미터를 지정할 수 있는데, 지정하지 않으면 기본 값으로 `newValue`를 사용한다. `didSet`에서는 바뀌기 전의 값의 파라미터를 지정할 수 있는데, 지정하지 않으면 기본 값으로 `oldValue`를 사용한다.

아래는 저장 프로퍼티에 프로퍼티 옵저버를 구현한 코드 예시이다.

```swift
  class Account {
    var credit: Int = 0 {
        willSet {
            print("잔액이 \(credit)원에서 \(newValue)원으로 변경될 예정입니다.")
        }
        
        didSet {
            print("잔액이 \(oldValue)원에서 \(credit)원으로 변경되었습니다.")
        }
    }
}

let myAccount: Account = Account()
// 잔액이 0원에서 1000원으로 변경될 예정입니다.
myAccount.credit = 1000
// 잔액이 0원에서 1000원으로 변경되었습니다.
```

<br>

### 1.4 타입 프로퍼티 (Type Property)

앞에서 본 저장 프로퍼티나 연산 프로퍼티는 클래스 또는 구조체 인스턴스를 생성한 후 사용할 수 있는 인스턴스 프로퍼티(Instance Property)이다.

인스턴스 프로퍼티는 인스턴스를 새로 생성할 때마다 초깃값에 해당하는 값이 프로퍼티의 값이 되고, 인스턴스마다 다른 값을 지닐 수 있다.

하지만 경우에 따라서 인스턴스에 관련된 값이 아니라 클래스나 구조체, 또는 열거형과 같은 객체 자체에 관련된 값을 다루어야 할 때도 있다.

각각의 인스턴스를 생성하지 않고 클래스나 구조체 자체에 값을 저장하게 되며 타입 자체에 속하는 프로퍼티를 **타입 프로퍼티(Type Property)**라고 부른다.

아래는 타입 프로퍼티와 인스턴스 프로퍼티의 차이를 확인할 수 있는 예시 코드이다.
```swift
class AClass {
    
    // 저장 타입 프로퍼티
    static var typeProperty: Int = 0
    
    // 저장 인스턴스 프로퍼티
    var instanceProperty: Int = 0 {
        didSet {
            AClass.typeProperty = instanceProperty + 100
        }
    }
    
    // 연산 타입 프로퍼티
    static var typeComputedProperty: Int {
        get {
            return typeProperty
        }
        
        set {
            typeProperty = newValue
        }
    }
}

AClass.typeProperty = 123

let classInstance: AClass = AClass()
classInstance.instanceProperty = 100

print(AClass.typeProperty)  // 200
print(AClass.typeComputedProperty)  // 200
```

타입 프로퍼티는 특정 클래스나 구조체, 그리고 열거형에서 모든 인스턴스들이 공유해야 하는 값을 정의할 때 유용하다.

일반적으로 타입 프로퍼티를 선언하는 요령은 클래스와 구조체에서 모두 같다. 클래스나 구조체의 정의 블록 내에서 타입 프로퍼티로 사용할 프로퍼티 앞에 `static` 키워드를 추가해주면 된다. 이 키워드는 구조체나 클래스에 관계없이 저장 프로퍼티와 연산 프로퍼티에 모두 사용할 수 있다. 클래스 타입에서 선언된 연산 프로퍼티의 일부에는 `class`키워드를 사용한다.

저장 타입 프로퍼티는 변수 또는 상수로 선언할 수 있으며, 연산 타입 프로퍼티는 변수로만 선언할 수 있다.

```swift
class Account {
    
    static let dollarExchangeRate: Double = 1000.0  // 타입 상수
    
    var credit: Int = 0         // 저장 인스턴스 프로퍼티
    
    var dollarValue: Double {   // 연산 인스턴스 프로퍼티
        get {
            return Double(credit) / Self.dollarExchangeRate
            // Self.dollarExchangeRate는 Account.dollarExchangeRate와 같은 표현이다.
        }
        
        set {
            // Self.dollarExchangeRate는 Account.dollarExchangeRate와 같은 표현이다.
            credit = Int(newValue * Self.dollarExchangeRate)
            print("잔액을 \(newValue)달러로 변경 중입니다.")
        }
    }
}
```

<br>

## 2. 메서드 (Method)

메서드 혹은 메소드(Method)는 **특정 타입의 객체 내부에서 사용하는 함수**를 말한다. 즉, 클래스나 구조체, 열거형과 같은 객체 내에서 함수가 선언될 경우 이를 메서드라고 통칭한다.

함수와 메서드의 차이점은 구현 목적이 가지는 독립성과 연관성에 있다. 함수는 독립적인 기능을 구현하기 위해 만들어지는 것이지만, 메서드는 하나의 객체 내에 정의된 다른 메서드들과 서로 협력하여 함수적인 기능을 수행한다.

메서드는 크게 **인스턴스 메서드(Instance Method)**와 **타입 메서드 (Type Method)**로 구분되는데, 객체의 인스턴스를 생성해야 사용할 수 있는 메서드가 인스턴스 메서드, 객체의 인스턴스를 생성하지 않아도 사용할 수 있는 메서드가 타입 메서드이다.

<br>

### 2.1 인스턴스 메서드 (Instance Method)

**인스턴스 메서드 (Instance Method)**는 클래스, 구조체 또는 열거형과 같은 객체 타입이 만들어 내는 인스턴스에 소속된 함수이다.

인스턴스 메서드는 인스턴스 프로퍼티에 접근하거나 수정하는 방법을 제공하거나 인스턴스의 생성 목적에 따른 함수적 관계성을 제공하는 등 객체의 인스턴스에 대한 기능적 측면을 제공한다.

인스턴스 메서드는 객체 타입 내부에 선언된다는 점을 제외하고는 일반 함수와 선언하는 형식이 완전히 동일하다.

하지만 함수와 달리, 인스턴스 메서드는 **해당 매서드가 속한 인스턴스를 통해서만 호출**될 수 있다.

다음은 클래스의 인스턴스 메서드를 정의하고 사용하는 코드 예시이다.


```swift
class LevelClass {
    // 현재 레벨을 저장하는 저장 프로퍼티
    var level: Int = 0 {
        // 프로퍼티 값이 변경되면 호출하는 프로퍼티 옵저버
        didSet {
            print("Level \(level)")
        }
    }

    // 레벨이 올랐을 때 호출할 메서드
    func levelUp() {
        print("Level Up!")
        level += 1
    }

    // 레벨이 감소했을 때 호출할 메서드
    func levelDown() {
        print("Level Down")
        level -= 1
        if level < 0 {
            reset()
        }
    }

    // 특정 레벨로 이동할 때 호출할 메서드
    func jumpLevel(to: Int) {
        print("Jump to \(to)")
        level = to
    }
    
    // 레벨을 초기화할 때 호출할 메서드
    func reset() {
        print("Reset!")
        level = 0
    }
}

var levelClassInstance: LevelClass = LevelClass()
levelClassInstance.levelUp()         // Level Up!
levelClassInstance.levelDown()       // Level Down
levelClassInstance.levelDown()       // Level Down
// Level -1
// Reset!
// Level 0
levelClassInstance.jumpLevel(to: 3)  // Jump to 3
// Level 3
```

인스턴스 메서드는 일반 함수와 선언 형식이 같지만 다음 세 가지 항목에서 차이가 있다.

1. 구조체와 클래스의 인스턴스에 소속된다는 점
2. 메서드 내에서 정의된 변수와 상수뿐만 아니라 클래스 범위에서 정의된 프로퍼티도 모두 참조할 수 있다는 점
3. **self 키워드**를 사용할 수 있다는 점

<br>

#### self 프로퍼티란?

```swift
self.프로퍼티
```

모든 인스턴스는 암시적으로 생성된 `self 프로퍼티`를 갖는다. `자바의 this와 비슷`하게 인스턴스 자기 자신을 가리키는 프로퍼티이다. self 프로퍼티는 인스턴스를 더 명확히 지칭하고 싶을 때 사용한다.

프로퍼티에 반드시 self 키워드를 붙이지 않더라도, 컴파일러는 이 변수가 프로퍼티의 것이라는 것을 대부분 잘 인식하고 사용하는 데 문제가 없도록 처리한다.

이 때문에 self 키워드를 생략하고 사용하는 경우도 많은데, **반드시 self 키워드를 붙여주어야 할 때**가 있다. 바로 **메서드 내부에 프로퍼티와 동일한 이름을 가진 변수나 상수가 선언되었을 때**이다.

**프로퍼티와 일반 변수의 이름이 충돌할 경우에는 프로퍼티 앞에 반드시 self 키워드를 붙여 주어야 한다.**

위의 인스턴스 메서드 예시 코드 처럼 level 변수를 사용할 때, 스위프트는 자동으로 메서드 내부에 선언된 지역변수를 먼저 사용하고, 그 다음 메서드 매개변수, 그 다음 인스턴스의 프로퍼티를 찾아 level이 무엇을 지칭하는지 유추한다.

메서드 매개변수의 이름이 level인데, 이 level 매개변수가 아닌 인스턴스 프로퍼티의 level을 지칭하고 싶다면 이 때 self 프로퍼티를 사용한다.

위의 인스턴스 메서드 예시 코드의 jumpLevel(to:) 메서드를 조금 변경해서 알아보자.

```swift
class LevelClass {
    var level: Int = 0

    func jumpLevel(to level: Int) {
        print("Jump to \(level)")
        self.level = level // 여기 주목
    }
}
```

level = level이라고 적으면 컴파일러는 좌측의 level은 인스턴스 프로퍼티인 level이 아닌 매개변수로 넘어온 level을 지칭하는 것으로 판단하게 된다.

그럴 때 좌측에 level 대신 **self.level**이라고 적어주면 좌측의 **level이 인스턴스 프로퍼티**라는 것을 명확히 할 수 있다.

추가로, self 프로퍼티는 값 타입 인스턴스 자체의 값을 치환할 수 있다. 클래스의 인스턴스는 참조 타입이라 self 프로퍼티에 다른 참조를 할당할 수 없는데, 구조체나 열거형 등의 self 프로퍼티를 사용하여 자신 자체를 치환할 수도 있다. `예시 코드 생략`

<br>

### 2.2 타입 메서드 (Type Method)

타입 프로퍼티를 메서드로 옮긴 게 타입 메서드(Type Method)이다. 즉, 인스턴스를 생성하지 않고 객체 타입(클래스나 구조체) 자체에서 호출할 수 있는 메서드를 **타입 메서드 (Type Method)**라고 한다.

클래스, 구조체, 열거형 타입에서는 프로퍼티 선언 앞에 `static` 키워드를 붙여주되 하위 클래스에서 재정의 가능한 타입 메서드를 선언할 때에는 `class` 키워드를 사용한다.

이렇게 선언된 타입 메서드를 호출할 때에는 인스턴스 메서드와 마찬가지로 점 구문을 이용한다. 그러나 타입 메서드는 인스턴스 메서드와 달리 self 프로퍼티가 타입 그 자체를 가리킨다는 점이 다르다.

인스턴스 메서드에서는 self가 인스턴스를 가리킨다면 타입 메서드의 self는 타입을 가리킨다. 그래서 타입 메서드 내부에서 타입 이름과 self가 같은 뜻이라고 볼 수 있고, 타입 메서드에서 self 프로퍼티를 사용하면 타입 프로퍼티 및 타입 메서드를 호출할 수 있게 된다.

아래 예시를 살펴보자

```swift
// 시스템 음량은 한 기기에서 유일한 값이어야 합니다.
struct SystemVolume {
    // 타입 프로퍼티를 사용하면 언제나 유일한 값이 된다.
    static var volume: Int = 5

    // 타입 프로퍼티를 제어하기 위해 타입 메서드를 사용한다.
    static func mute() {
        self.volume = 0
        // SystemVolume.volume = 0 그리고 Self.volume = 0과 같은 표현이다.
    }
}

// 내비게이션 역할은 여러 인스턴스가 수행할 수 있다.
class Navigation {
    // 네비게이션 인스턴스마다 음량을 따로 설정할 수 있다.
    var volume: Int = 556
    
    // 길 안내 음성 재생
    func guideWay() {
        // 내비게이션 외 다른 재생원 음소거
        SystemVolume.mute()
    }

    // 길 안내 음성 종료
    func finishGuideWay () {
        // 기존 재생원 음량 복구
        SystemVolume.volume = self.volume
    }
}

SystemVolume.volume = 10

let myNavi: Navigation = Navigation()

myNavi.guideWay()
print(SystemVolume.volume)  // 0

myNavi.finishGuideWay()
print(SystemVolume.volume)  // 5
````

<br>

--- 
### 참고 자료

[도서 : 꼼꼼한 재은 씨의 Swift 문법편](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791186710234)

[프로퍼티 (Properties)](https://jusung.gitbook.io/the-swift-language-guide/language-guide/10-properties)

[매소드 (Methods)](https://jusung.gitbook.io/the-swift-language-guide/language-guide/11-methods)

[프로퍼티 – Property](https://blog.yagom.net/556/)