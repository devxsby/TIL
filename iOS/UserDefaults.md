# UserDefaults

An interface to the user’s defaults database, where you store key-value pairs persistently across launches of your app.

> 기본적으로 제공되는 DB를 사용할 수 있는 인터페이스. key-value 쌍으로 유지됨.

<br>

## Declaration

```swift
class UserDefaults : NSObject
```

앱에서 사용되거나 필요한 데이터를 영구적으로 보관하기 위한 방법으로는 **네트워크 서버 이용, CoreData, UserDefaults** 등이 있다.

이 중 `UserDefaults`는 **런타임 환경에서 동작**하면서, 앱이 실행되는 동안 **기본 저장소 (default database)에 접근해 데이터를 기록하고, 가져오는 역할을 하는 인터페이스**이다.

UserDefaults는 **싱글톤 패턴으로 설계**되어 **앱 전체에서 단 하나의 인스턴스만 존재**한다. 즉, 앱 전체가 공유해서 사용하는 형태이다. UserDefaults 객체를 호출하면 각각의 호출에 응답하기 위한 인스턴스가 생성되는 것이 아니라 하나의 UserDefaults 객체가 모든 요청을 받아 처리하는 형태이다.

UserDefaults는 **float, double, integer, boolean** 과 같은 공통 유형에 액세스하기위한 메소드를 제공할 뿐만아니라 **NSData, NSString, NSNumber, NSDate, NSArray 또는 NSDictionary 유형**의 객체를 저장할 수도 있다.

사용자 정의 객체를 저장하거나 불러오고싶을 때는 NSKeyedArchiver(NSKeyedUnarchiver), codable을 활용해야 한다.

`간단정리💡`

- 앱의 기본 데이터베이스에 영구적으로 데이터를 저장할 수 있는 인터페이스
- **key - value 쌍**으로 저장
- **Singleton 객체**, 앱 전체에서 하나의 인스턴스로 사용
- `float`, `double`, `NSData`, `NSString` 등의 데이터 타입 저장, 불러오기

<br>

## 사용하기

UserDefaults 클래스는 Foundation 프레임워크 안에 내장된 클래스이기 때문에 먼저 Foundation을 가져온다.

### UserDefaults.standard

공유된 기본값 객체를 반환하는 함수이다. 현재 입력된 값이 없기 때문에 NSUserDefaults 값만 출력된다.

```swift
import Foundation
print(UserDefaults.standard)

// <NSUserDefaults: 0x121e13450>
```

### set(Any?, forKey: String)

set 명령어를 통해 UserDefaults 데이터베이스에 등록을 할 수 있다. 기본적인 형식이 Dictionary 구조이기 때문에 앞쪽에 value값을 넣고 forKey에는 key값을 지정해야한다.

데이터가 존재하는지 확인하기 위해서는 해당 값의 형식에 따라 object, url, array, dictionary, string, stringArray, data, bool, integer, float, double 등을 호출하고 해당 값의 키를 forKey 파라미터에 입력을 하면 된다.

value값이 String인 경우 값이 없을 수도 있는 경우가 있기 때문에 해당 값들을 사용할려면 ??나 guard 문을 사용하여 없을 때를 대비하는 것이 좋다.

기존의 사용하던 key의 value값을 바꿀 때도 set을 통해 똑같은 key값을 입력하면 변경이 가능하다.

```swift
import Foundation
print(UserDefaults.standard)
UserDefaults.standard.set("Daesiker", forKey: "name")
UserDefaults.standard.set(27, forKey: "age")
UserDefaults.standard.set(4.1, forKey: "grade")
print(UserDefaults.standard.string(forKey: "name") ?? "no name")
print(UserDefaults.standard.integer(forKey: "age"))
print(UserDefaults.standard.double(forKey: "grade"))

//<NSUserDefaults: 0x126613450>
//Daesiker
//27
//4.1
```

### removeObject(forKey: String)

해당 키에 데이터가 존재하면 삭제해주는 메서드이다.

여기서 주의할 점은 Bool, Int, Float, Double 타입의 value들은 해당 키가 없으면 기본값을 반환해준다. Bool은 false가 기본값이고 Int, Float, Double은 0을 반환한다.

```swift
import Foundation
print(UserDefaults.standard)
UserDefaults.standard.set("Daesiker", forKey: "name")
UserDefaults.standard.set(27, forKey: "age")
UserDefaults.standard.set(4.1, forKey: "grade")
print(UserDefaults.standard.string(forKey: "name") ?? "no name")
print(UserDefaults.standard.integer(forKey: "age"))
print(UserDefaults.standard.double(forKey: "grade"))

UserDefaults.standard.removeObject(forKey: "age")

print(UserDefaults.standard.string(forKey: "name") ?? "no name")
print(UserDefaults.standard.integer(forKey: "age"))
print(UserDefaults.standard.double(forKey: "grade"))
/*

<NSUserDefaults: 0x126613450>
Daesiker
27
4.1
Daesiker
0
4.1
*/
```

<br>

---

### 참고자료

[UserDefaults | Apple Developer Documentation](https:/developer.apple.com/documentation/foundation/userdefaults)

[[iOS] UserDefaults 사용해보기](https://junghun0.github.io/2019/05/21/ios-userdefaults/)

[[iOS] UserDefaults 사용하기](https://velog.io/@nnnyeong/iOS-UserDefaults-사용하기)

[[ios] userDefaults를 이용한structure 타입 Data 저장하기](https://velog.io/@cooo002/ios-userDefaults를-이용한structure-타입-Data-저장하기)

[iOS ) 왕초보를 위한 User Defaults사용해보기(switch)](https://zeddios.tistory.com/107)
