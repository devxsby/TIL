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


> 가이드 원칙
 
Swift에서는 아래와 같은 접근레벨 가이드 원칙 (Guiding Principle of Access Levels)을 제공하고 있다.
 
1. **public 변수는 다른 internal, file-private 혹은 private 타입에서 정의될 수 없습니다.** 왜냐하면 그 타입은 public 변수가 사용되는 모든 곳에서 사용될 수 없을 것이기 때문입니다.

2. **함수는 그 함수의 파라미터 타입이나 리턴 값 타입보다 더 높은 접근 레벨을 갖을 수 없습니다.** 왜냐하면 함수에는 접근 가능하지만 파라미터에 접근이 불가능 하거나 혹은 반환 값 타입보다 접근 레벨이 낮아 함수를 사용하는 관련 코드에서 이용할 수 없을 수 있기 때문입니다.

3. 아무런 **접근 레벨을 명시하지 않은 경우 internal을 갖게 됩니다.**

4. **단일 타겟 앱에서는 특별히 접근레벨을 명시할 필요가 없지만** 필요에 따라 **fileprivate, private등을 사용해 앱내에서 구현 세부사항을 숨길 수 있습니다.**

5. **기본적으로 open이나 public으로 지정된 엔티티만 다른 모듈에서 접근 가능합니다.** 하지만 유닛테스트를 하는 경우 모듈을 **import할때 import앞에 @testable이라는 에트리뷰트를 붙여주면 해당 모듈을 테스트**가 가능한 모듈로 컴파일해 사용합니다.

<br>

---

### 참고자료 

[[iOS - swift 공식 문서] 26. Access Control (접근제한)](https://ios-development.tistory.com/644?category=989689)


[[Swift] Access Control : 접근제어(open, public, internal, fileprivate, private)](https://joycestudios.tistory.com/15)

[https://velog.io/@zooneon/Swift-접근제어에-대해-알아보자](https://velog.io/@zooneon/Swift-접근제어에-대해-알아보자)
