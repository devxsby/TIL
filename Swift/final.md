# final 키워드

- 오버라이드 방지 (재정의 방지)

- 재정의, 상속을 방지하고 싶은 것들 앞에 `final`을 붙여주면 된다.

- 예시 : `final var`, `final func`, `final class` 등


## 예제

```swift
class Person {
    final var name: String = ""
    
    final func speak() {
        print("가나다라마바사")
    }
}

final class Student: Person {
    override var name: String {
        set {
            super.name = newValue
        }
        get {
            return "학생"
        }
    } // Person의 name은 final을 사용했기 때문에 재정의할 수 없다.
    
    override func speak() {
        print("학생입니다.")
    } // Person의 speak은 final을 사용했기 때문에 재정의할 수 없다.
}

class UniversityStudent: Student {} // Student는 final을 사용했기 때문에 상속받을 수 없다.
```

## 성능적 이점 자세히...

런타임 시기에 성능적 이점을 가질 수 있으며, Static Dispatch와 Dynamic Dispatch의 차이를 알아야 하지만 이것들을 알기 전에 vTable을 먼저 알아야 한다.

### vTable이란?

Swift는 클래스마다 vTable을 가지고 있는데, 이는 클래스 내부의 함수들 중 어떠한 함수를 호출해야 할지 결정하는 테이블을 이야기한다.

따라서 함수가 호출되면 vTable에서 지금 class가 어떠한 함수를 호출해야 하는지 조회를 하고 해당 함수를 호출하는 방식을 갖게된다.

왜냐하면 함수가 오버라이딩 될 수 있는 가능성이 있기 때문에 상속받은 함수를 호출해야 할지, 오버라이딩 한 함수를 호출해야 할지 누군가는 알려주어야 하기 때문이다.

이러한 과정이 런타임 때 이루어지며, 이는 당연히 성능 측면에서 불리할 수 밖에 없다..
 
### Dynamic Dispatch

이름에서 알 수 있듯이 유동적으로 어떤 메소드와 프로퍼티를 호출할지 결정하는 방법이다. 

즉, 런타임에 결정된다는 말이다! (Swift는 기본적으로 해당 방법을 사용하고 있다)

### Static Dispatch

Dynamic Dispatch와는 반대로 컴파일 시기에 이미 어떠한 메소드와 프로퍼티를 호출할지 결정한다.

그러므로 Dynamic Dispatch처럼 결정하는 과정이 없으니 성능상으로 이점이 있을 것이다..!


> ⭐️ 추가적으로 `final` 키워드 뿐만 아니라 `private` 또한 다른 곳에서 override를 하지 않으면 컴파일러가 알아서 Static Dispatch처럼 동작한다.
> 
> 접근제한자의 역할 뿐만 아니라 이러한 이유에서라도 private를 습관화 하는게 좋다.


---

### 참고자료

[[iOS][Swift] Final 키워드에 관한 문법적 의미와 성능적 관점](https://itllbegone.tistory.com/10)