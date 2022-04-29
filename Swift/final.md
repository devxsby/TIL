# final

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


### 참고자료

[]()