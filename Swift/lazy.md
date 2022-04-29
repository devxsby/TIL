# lazy 키워드

- 지연 저장 프로퍼티 (Lazy Stored Properties)

- 이 프로퍼티가 호출되기 전까지는 초기화되지 않다가, 호출될 때 초기화 됨

- 복잡한 계산, 부하가 많이 걸리는 작업에서 지연 프로퍼티를 사용하면 불필요한 성능저하, 공간 낭비를 줄일 수 있음

- 예시 : `lazy var` 

  > **주의** `lazy let`은 틀린 표현


## 예제

```swift
struct ColorSet {
    var red: Int = 255
    var green: Int = 255
    var blue: Int = 255
}

class ArtWork {
    // 지연 저장 프로퍼티
    lazy var backgroundColor: ColorSet = ColorSet()
    let name: String
    
    init(name: String) {
        self.name = name
    }
}

let myFavoriteArtWork = ArtWork(name: "Composition II with Red Blue and Yellow")

print(myFavoriteArtWork.backgroundColor) // 이때 ColorSet 생성
```

## 결과

```swift
ColorSet(red: 255, green: 255, blue: 255)
```

### 참고자료

[]()