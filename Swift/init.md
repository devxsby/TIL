# 초기화

## 1. init의 존재 이유

> 모든 저장 프로퍼티들은 초기화돼야 한다. `init` 키워드는 초기화 과정를 도와준다.

```swift
class Test{
    var a:Int! // nil로 초기화
    var b:Int? // nil로 초기화
    var c: Int // init으로 초기화
    var d: Int // error
    
    init(tmp: Int){
        self.c = tmp
    }
}
// d 변수가 초기돠되지 않아서 error
```

---

## 2. 초기화의 종류

### 2-1. 초기화 메소드의 델리게이션 : super.init()

- 연쇄적인 초기화과정

```swift
class A{
    var a: Int
    
    init(value: Int) {
        self.a = value
    }
}
 
class B: A{
    init(value1: Int, value2: Int) {
        super.init(value: value1)
    }
}
 
class C: B {
    init(value1: Int, value2: Int, value3: Int) {
        super.init(value1: value1, value2: value2)
    }
}
```

### 2-2. 지정 초기화 메소드 (Designated Initializer)

- 서브 클래싱 시에 필수적으로 초기화해주어야 하는 메소드 : `init(...)`으로 작성

- **주의** : `required init`은 서브클래스에서 super.init이 아닌 직접 구현해야 하는 초기화 메소드이다.

```swift
class A{
    var a: Int
    
    init(value: Int) {
        self.a = value
    }
    
    required init (val1: Int, val2: Int, val3: String){
        self.a = val1
    }
}
 
class B: A{
    required init(val1: Int, val2: Int, val3: String) {
        super.init(value: val1)
    }
}
```

### 2-3. 편의 초기화 메소드 (Convenience Initializer)

- 현재 정의되어 있는 초기화 메소드를 불러 추가 내용을 붙이는 것 : `convenience init(...)`으로 작성

- **주의** : `convenience`가 아닌 `init`키워드는 블록안에 self.init과 같이 다른 초기화 메소드를 부를 수 없다.

```swift
class A{
    var a: Int
    var name: String!
    
    init(val1: Int, val2: String) {
        self.a = val1
        self.name = val2
    }
    
    convenience init(newVal: Int) {
        self.init(val1: newVal, val2: "imustang")
    }
}
```

---

## 3. init의 오버로딩과 오버라이딩

- 서브클래싱 시, **수퍼 클래스에서 init키워드로 작성된 것은 반드시 super로 초기화 필요**

### 3-1. 오버로딩 : 같은 이름의 method, 다른 parameter 혹은 return 타입

```swift
class A{
    var a: Int
    
    init(inValue: Int) {
        self.a = inValue
    }
}

class B: A{
    var b: Int
    
    init(inValue: Int, inValue2: Int) {
        self.b = inValue2
        super.init(inValue: inValue)
        
    }
}
// 오버로딩 안할 시 a 변수가 초기화 되지 않은 상태로 남아있음
```

### 3-2. 오버라이딩 : method 이름, parameter나 return 타입이 모두 같아야 함

- 서브 클래싱 시, 수퍼 클래스와 **같은 초기화 이름으로 할 땐 override init**으로 선언


```swift
class A{
    var a: Int
    
    init(inValue: Int) {
        self.a = inValue
    }
}

class B: A{
    var b: Int
    
    override init(inValue: Int) {
        self.b = inValue
        super.init(inValue: inValue)
    }
}
```


---

### 참고자료

[[swift] 13. 초기화(init, convenience init, required init, override init)](https://ios-development.tistory.com/44?category=887218)