# 뷰 계층구조(View Hierarchy)

## 1. view의 계층 구조는 superView, subView, siblingView로 특정되며 이는 drawing 순서를 결정 짓습니다.

- superView와 subView의 관계에서는 superView가 우선해서 그려진다.

- 동일한 superView 내부에 여러 siblingView가 있다면 먼저 addSubview가 된 순으로 drawing 된다.

- 두 sibilingView가 겹쳐 있다면 먼저 drawing된 View가 가려진다.

<p align="center"><img src= "https://velog.velcdn.com/images%2Fyongchul%2Fpost%2F91ce470e-caf1-4e88-a3e9-baf7b5eb853e%2Fimage.png " width="80%" ></p>

---

## 2. SuperView와 SubView의 계층구조에 따른 몇가지 특징

- superView를 제거하면 subView도 함께 제거됩니다.

- superView의 투명도는 subView에도 적용됩니다.

- superView의 size가 변하면 subView의 size도 함께 변합니다.

- superView는 subViews를 array로 관리합니다.

---

## 3. subView 관리를 위한 methods

- addSubView(_:)로 subView를 추가하고 removeFromSuperView()를 통해 제거할 수 있습니다.

- subView에는 tag를 붙일 수 있고 viewWithTag(_ :)메서드를 통해 subView를 사용할 수 있습니다.

```
subView.tag = 1
let assignSubView = superView.viewWithTag(1) // assignSubView는 subView를 가리킴
```

- isDescendant(of:)를 사용하여 특정 view 가 superView인지 확인할 수 있습니다.

```
let superView = UIView()
let subView = UIView()

superView.addSubview(subView)

let isSuperView: Bool = subView.isDescendant(of: superView)
print(isSuperView) // true
```

- willRemoveSubview(:), didAddSubview(:) willMove(toSuperview:), didMoveToSuperview willMove(toWindow:), didMoveToWindow를 사용하여 view를 동적으로 관리할 수 있습니다.

- insertSubview(:at:) insertSubview(:belowSubview:), insertSubview(_:aboveSubview:) 를 사용하면 원하는 위치에 subView를 추가할 수 있습니다.

- subView를 한 번에 모두 제거하는 method는 없지만 아래 code를 사용하면 한 번에 제거할 수 있습니다.

```
 superView.subviews.forEach {$0.removeFromSuperview()}
```

<br>

---

### 참고자료

[[iOS]View의 계층구조에 따른 특징과 subView관리를 위한 Method](https://velog.io/@yongchul/iOSView의-계층구조)