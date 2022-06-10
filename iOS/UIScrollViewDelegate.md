# UIScrollViewDelegate 메소드

- 스크롤링, 줌잉, 스크롤의 감속, 스크롤 애니메이션같은 작업에 반응하는 메소드들

## Declaration

```swift
protocol UIScrollViewDelegate
```

## Responding to Scrolling and Dragging 함수들

`계속 추가 예정`

### 1. func scrollViewDidScroll

```swift
func scrollViewDidScroll(_ scrollView: UIScrollView) {
    // Tells the delegate when the user scrolls the content view within the receiver
}
```
> 특징: offset 값이 바뀔 때 마다 호출되므로 살짝만 스크롤해도 계속 호출됨

### 2. func scrollViewWillBeginDragging

```swift
func scrollViewWillBeginDragging(_ scrollView: UIScrollView) {
// called on start of dragging (may require some time and or distance to move)
}
```
> 특징: : 스크롤뷰에서 드래그하기 시작할 때 한 번만 호출됨

### 3. func scrollViewWillEndDragging

```swift
func scrollViewWillEndDragging(_ scrollView: UIScrollView, withVelocity velocity: CGPoint, targetContentOffset: UnsafeMutablePointer<CGPoint>) {
// called on finger up if the user dragged. velocity is in points/millisecond. targetContentOffset may be changed to adjust where the scroll view comes to rest
}
```
> 특징 : 손을 땠을 때 한 번 호출되고, 그 때의 속도(방향 포함)를 알 수 있다.

### 4. func scrollViewDidEndDragging

```swift
func scrollViewDidEndDragging(_ scrollView: UIScrollView, willDecelerate decelerate: Bool) {
// called on finger up if the user dragged. decelerate is true if it will continue moving afterwards
}
```
> 특징 : 손을 땠을 때 한 번 호출되고, 테이블 뷰의 스크롤 모션의 감속 여부를 알 수 있다.

---

### 공식문서

[UIScrollViewDelegate](https://developer.apple.com/documentation/uikit/uiscrollviewdelegate)

### 참고자료

[UIScrollViewDelegate](https://yagom.net/forums/topic/uitableview에서-주로-사용되는-uiscrollviewdelegate를-알아보자/)

[UIScrollViewDelegate 메소드 파헤치기](https://iamcho2.github.io/2020/12/30/uiscrollview-delegate-methods)