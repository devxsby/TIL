# automaticDimension

## Declaration

```swift
class let automaticDimension: CGFloat
```

## Discussion

테이블 뷰에서 지정된 차원에 대한 기본값을 선택하려면 테이블 뷰의 위임 메서드에서 이 값을 반환합니다.

예를 들어 tableView(_:heightForHeaderInSection:) 또는 tableView(_:heightFooterInSection:)에서 이 상수를 반환할 경우 테이블 뷰는 tableView(_:titleForHeaderInSection:) 또는 tableView(_:titleFooterInSection:)에서 반환된 값에 맞는 높이를 사용합니다.

---

### 공식문서

https://developer.apple.com/documentation/uikit/uitableview/1614961-automaticdimension
