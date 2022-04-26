# 싱글톤 패턴 (Singleton Pattern)

## 1. 싱글톤 패턴(Singleton Pattern)이란?

`애플리케이션이 시작될 때, 어떤 클래스가 최초 한 번만 메모리를 할당(static)하고 해당 메모리에 인스턴스를 만들어 사용하는 패턴`

즉, 싱글톤 패턴은 '하나'의 인스턴스만 생성하여 사용하는 디자인 패턴이다.

생성자가 여러번 호출되도, 실제로 생성되는 객체는 하나이며 최초로 생성된 이후에 호출된 생성자는 이미 생성한 객체를 반환시키도록 만든다.

객체를 하나만 생성하여 여러 곳에서 공용으로 사용하고 싶을 때 주로 사용한다.

> 인스턴스가 필요할 때, 똑같은 인스턴스를 만들지 않고 기존의 인스턴스를 활용함!

## 2. 싱글톤 패턴을 사용하는 이유

예제를 통해 알아보도록 하자. 아래와 같이 유저의 정보를 저장하는 클래스가 있다.

```swift
class UserInfo {
    var id: String?
    var pw: String?
    var name: String
}
```
만약, A ViewController에서 name, B ViewController에서 id, C ViewController에서 pw 정보를 받아와 이 클래스(UserInfo)에 저장해야된다면 아래와 같이 코드를 작성할 수 있다.

```swift
// A ViewController
let userInfo = UserInfo()
userInfo.name = "Subin"

// B ViewController
let userInfo = UserInfo()
userInfo.id = "subin123"

// C ViewController
let userInfo = UserInfo()
userInfo.pw = "123456"
```

세 개의 Viewcontroller에서 UserInfo 객체를 각각 따로 생성하여 id, pw, name 정보를 저장하게된다.

이렇게 되면 각각의 인스턴스에 정보가 따로 저장되는데, 우리는 하나의 인스턴스에 모든 정보(id, pw, name)이 저장되는 것을 원한다고 하자.

이럴 때 싱글톤 패턴을 사용해서 클래스에 대한 인스턴스는 최초 생성시에 전역으로 저장해두고, 이후에 이 전역 인스턴스에 접근하도록 하는 방식을 사용할 수 있다.

이렇게 **한 인스턴스를 여러 곳에서 접근이 가능하도록** 하는 것이 바로 싱글톤이다.

싱글톤을 사용하면 생성시에 메모리를 사용하고, 사용하지 않을 때에는 메모리를 해제할 수 있기 때문에 메모리 낭비를 방지할 수 있다는 장점이 있다.

## 3. 싱글톤 패턴 적용하는 법

### 3-1. static 프로퍼티로 Instance 생성

먼저 전역으로 저장될 것이기 때문에 static 키워드를 이용해 클래스 안에 인스턴스를 저장할 프로퍼티를 하나 생선한다.

싱글톤 패턴은 참조 타입만 가능하기 때문에 class를 이용해야 합니다.

```swift
class UserInfo {
    static let shared = UserInfo()

    var id: String?
    var pw: String?
    var name: String?
}
```
### 3-2. init() private으로 접근제어

생성자인 init()을 private로 접근제어를 해준다.

외부의 접근을 차단시켜 UserInfo 클래스의 shared 인스턴스로만 접근 가능하도록 하는 것이 싱글톤 패턴이다.

```swift
class UserInfo {
    static let shared = UserInfo()

    var id: String?
    var pw: String?
    var name: String?

    private init() { }
}
```

### 3-3. Singleton Class 접근 방법

위에서 만든 싱글톤 객체를 외부에서 접근하기 위해서는 생성해준 static 프로퍼티를 이용한다. 

어느 클래스에서든 shared라는 static 프로퍼티로 접근하면, 하나의 인스턴스를 공유하는 것이기 때문에, 아래와 같이 다른 ViewController에서 동일한 인스턴스에 접근해 값을 정해줄 수도 있다.

```swift
// A ViewController
let userInfo = UserInfo.shared
userInfo.name = "Subin"

// B ViewController
let userInfo = UserInfo.shared
userInfo.id = "subin123"

// C ViewController
let userInfo = UserInfo.shared
userInfo.pw = "123456"
```

아래 코드처럼 저장된 값을 가져와 사용해줄 수도 있다.

```swift
// D ViewController
let userInfo = UserInfo.shared
print(userInfo.name ?? "")
```

---

### 참고자료

[[Swift] 싱글톤 패턴(Singleton Pattern)](https://janechoi.tistory.com/4)

[[디자인 패턴] 싱글톤 패턴(Singleton pattern)](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Design%20Pattern/Singleton%20Pattern.md)