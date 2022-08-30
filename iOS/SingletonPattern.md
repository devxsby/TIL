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

먼저 전역으로 저장될 것이기 때문에 static 키워드를 이용해 클래스 안에 인스턴스를 저장할 프로퍼티를 하나 생성한다.

싱글톤 패턴은 참조 타입만 가능하기 때문에 class를 이용해야 한다.

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


### 4. 싱글톤 패턴의 문제점

싱글톤 패턴은 전역 상태(Global State)로 이용할 수 있다는 장점이 있지만 다음과 같은 문제점을 가지고 있어 **안티 패턴**으로도 많이 불린다.

- private 생성자를 갖고 있어 상속이 불가능하다.
- 테스트하기 힘들다.
- 서버 환경에서는 싱글톤이 1개만 생성됨을 보장하지 못한다.
- 전역 상태를 만들 수 있기 때문에 바람직하지 못하다.

1. private 생성자를 갖고 있어 상속이 불가능하다.
싱글톤은 자신만이 객체를 생성할 수 있도록 생성자를 private으로 제한한다. 하지만 상속을 통해 다형성을 적용하기 위해서는 다른 기본생성자가 필요하므로 객체지향의 장점을 적용할 수 없다. 또한 싱글톤을 구현하기 위해서는 객체지향적이지 못한 static 필드와 static 메소드를 사용해야 한다.
 
 
2. 테스트하기 힘들다.
싱글톤은 테스트하기가 힘드며 테스트 방법에 따라 불가능할 수 있다. 싱글톤은 생성 방식이 제한적이기 때문에 Mock 객체로 대체하기가 어려우며, 동적으로 객체를 주입하기도 힘들다.
테스트는 개발의 핵심인데, 테스트 코드를 작성할 수 없다는 것은 큰 단점이 된다.
 
 
3. 서버 환경에서는 싱글톤이 1개만 생성됨을 보장하지 못한다.
서버에서 클래스 로더를 어떻게 구성하느냐에 따라 싱글톤 클래스임에도 불구하고 1개 이상의 객체가 만들어질 수 있다. 따라서 Java 언어를 이용한 싱글톤 기법은 서버 환경에서 싱글톤이 꼭 보장된다고 볼 수 없다. 또한 여러 개의 JVM에 분산돼서 설치되는 경우에도 독립적으로 객체가 생성된다.
 
 
4. 전역 상태를 만들 수 있기 때문에 바람직하지 못하다.
싱글톤의 스태틱 메소드를 이용하면 언제든지 해당 객체를 사용할 수 있고, 전역 상태(Global State)로 사용되기 쉽다. 아무 객체나 자유롭게 접근하고 수정하며 공유되는 전역 상태는 객체지향 프로그래밍에서 권장되지 않는다.
 
싱글톤 패턴은 객체를 1번 생성하고 재사용할 수 있다는 장점이 있으므로 싱글톤 패턴이 필요한 경우도 분명히 있다. 하지만 싱글톤 패턴을 직접 구현하면 다른 단점들이 너무 크게 부각되기 때문에 활용이 쉽지 않다. 만약 프레임워크에게 싱글톤을 위임할 수 있다면 위와 같은 단점들을 많이 해결할 수도 있다.

---

### 참고자료

[[Swift] 싱글톤 패턴(Singleton Pattern)](https://janechoi.tistory.com/4)

[[디자인 패턴] 싱글톤 패턴(Singleton pattern)](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Design%20Pattern/Singleton%20Pattern.md)

[Singleton Pattern](https://velog.io/@pcsoyeon/Singleton-Pattern)

[[디자인 패턴] 싱글톤이 안티 패턴이 될 수 있는 이유와 자바 싱글톤과 스프링 싱글톤의 차이](https://mangkyu.tistory.com/153)

[[Swift] Singleton Pattern(싱글턴 패턴)이란?](https://velog.io/@5n_tak/Swift-Singleton-Pattern싱글턴-패턴이란)

[Swift: Singleton, 싱글톤 패턴](https://medium.com/hcleedev/swift-singleton-싱글톤-패턴-b84cfe57c541)
