# 접근 제한자 (private, fileprivate, internal, public, open)

## 접근 제한자 형용 범위

- private : 같은 클래스

- fileprivate : 같은 소스 파일 (.swift)

- internal : 같은 모듈(framework) or 같은 프로젝트

- public : 모듈 외부까지 가능

- open : 모듈 외부, 상속 및 override 가능 (확장 가능)

**선언하지 않을시 디폴트는 internal 접근 제한자 !!!**

> 블록 내부에서의 접근제한자 예시

```swift
public class SomePublicClass {                  // explicitly public class
    public var somePublicProperty = 0            // explicitly public class member
    var someInternalProperty = 0                 // implicitly internal class member
    fileprivate func someFilePrivateMethod() {}  // explicitly file-private class member
    private func somePrivateMethod() {}          // explicitly private class member
}

class SomeInternalClass {                       // implicitly internal class
    var someInternalProperty = 0                 // implicitly internal class member
    fileprivate func someFilePrivateMethod() {}  // explicitly file-private class member
    private func somePrivateMethod() {}          // explicitly private class member
}

fileprivate class SomeFilePrivateClass {        // explicitly file-private class
    func someFilePrivateMethod() {}              // implicitly file-private class member
    private func somePrivateMethod() {}          // explicitly private class member
}

private class SomePrivateClass {                // explicitly private class
    func somePrivateMethod() {}                  // implicitly private class member
}
```

---

### 참고자료 

[[iOS - swift 공식 문서] 26. Access Control (접근제한)](https://ios-development.tistory.com/644?category=989689)
