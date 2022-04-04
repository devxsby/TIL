# 오토 레이아웃(Auto Layout)

다음은 애플 공식 홈페이지에서 언급한 것이다.

> **iPhone 및 iPad용 적응형 사용자 인터페이스 구축**
> > 앱은 디스플레이 크기 또는 종횡비에 상관없이 모든 iPhone 및 iPad 모델에서 근사하게 보여야 합니다. Xcode storyboard 및 Auto Layout과 같은 기능을 사용할 경우, 앱의 인터페이스 요소와 레이아웃이 자동으로 화면에 맞춰집니다. WWDC19에서 공지한 바와 같이, 2020년 4월부터 App Store에 제출되는 모든 앱은 Xcode storyboard를 사용하여 앱의 실행 화면을 제공해야 하며, 모든 디스플레이 크기를 지원하는 인터페이스를 가지고 있어야 합니다.

이처럼 앱을 만들 때, 어느 디바이스에서 보든 동일한 화면이 적용되게 디자인해야한다.

오토 레이아웃의 Visual Design에 관련된 내용은 `Apple폴더 -> HIG폴더 -> Adaptivity and Layout.md에 올리도록 하겠다.` `추후 업데이트 예정`. 이 곳에서는 디자인적 요소는 살펴보지 않겠다.

<br>

## 1. 오토 레이아웃(Auto Layout)이란?

오토 레이아웃(Auto Layout)이란, 제약조건(Constraints)에 따라 뷰 계층 구조에 있는 모든 뷰의 **위치**와 **크기**를 **동적**으로 지정하는 것이다. 그렇기 때문에 디스플레이의 크기와 관계없이 동일한 레이아웃을 구현할 수 있다.

오토 레이아웃이 아닌 자기 자신의 좌표계 기반인 Bounds나 원점에서부터 거리와 크기를 기반으로 한 Frame-Based Layout로 화면을 구성할 시 현재 화면의 크기가 어떻든 간에 해상도가 다른 화면에서는 여백이 남거나, 반대로 화면이 잘리게 된다. 

그렇기 때문에 우리는 오토 레이아웃 기능을 사용해서 디바이스가 바뀌어도 상하좌우 여백이 알맞게 설정되도록 해야한다. 

<br>

## 2. 오토 레이아웃(Auto Layout) 설정하기

오토 레이아웃은 각 객체마다 제약 조건(constraints)을 설정하여 사용한다. 제약 조건이란 각 객체가 가질 수 있는 여백, 정렬 방법, 다른 객체와의 간격 등을 의미하며 제약 조건은 스토리보드 하단의 **`정렬 조건(Align)`** 아이콘 및 **`제약 조건(Pin)`** 아이콘을 클릭하여 설정할 수 있다. 

<p align="center"><img src="https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/Art/Auto_Layout_Tools_2x.png" width="150px" height="150px"></p>


`정렬 조건(Align)`

<p align="center"><img src="https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/Art/Align_Tool_Popup_2x.png" width="50%" height="50%"></p>


`제약 조건(Pin)`

<p align="center"><img src= "https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/Art/Pin_Popover_View_2x.png" width="50%" height="50%">

설정한 제약 조건은 도큐먼트 아웃라인 영역 또는 오른쪽 인스펙터 영역의 삼각자모양 아이콘 'Size Inspector' 버튼을 클릭한 후 확인할 수 있다.

<p align="center"><img src= https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/Art/attributes_2x.png  width="50%" height="50%">

Top, Bottom은 위쪽, 아래쪽의 제약을 설정하는 것이다. Left, Right 또한 왼쪽, 오른쪽 제약을 설정하는 것이다.

그러나 실제 마진을 설정할 때 Left / Right 대신 Leading / Trailing을 왼쪽 오른쪽처럼 자주 사용한다. 여기서 Leading은 Text가 시작되는 지점, Trailing은 Text가 끝나는 지점을 의미한다.

`왜일까?`

우리가 흔히 사용하는 한글이나 영어로 글을 쓸 때 왼쪽에서 오른쪽 방향으로 쓰기 때문에 Leading이 왼쪽, Trailing이 오른쪽이 되지만 아랍권에서는 오른쪽에서 왼쪽 방향을 쓰기 때문에 Leading이 왼쪽, Trailing이 오른쪽이 되어버린다. 

WWDC 2015에서 오른쪽 / 왼쪽을 특정적으로 고정해야 하는 사항이 아니면, Leading과 Trailing을 사용해야 한다고 한다고 언급한 바 있다. <https://developer.apple.com/videos/play/wwdc2015/218/>

<br>

## 3. Safe Area란?

Xcode의 미리보기 기능으로 오토 레이아웃이 정상적으로 설정되었는지 확인해볼 수 있다. 보조 편집기 영역을 연 후 'Adjust Editor Options' 버튼을 클릭한 후 'Preview' 메뉴를 선택한다.

그러나 미리보기 결과에서 화면 크기가 다른 디바이스의 위, 아래 여백이 다르게 나타나는 경우를 볼 수 있는데 이는 디바이스끼리 Safe Area라는 영역이 서로 다르기 때문에 발생한 문제이다.

Safe Area는 노치가 적용된 iOS 11 기반의 아이폰x 시리즈부터 만들어졌다. 이전에는 회전했을 시 Top과 Bottom만 체크하면 됐지만, 노치가 생긴 후로는 Leading과 Trailing에 대한 마진도 필요하게 되었기 때문에 Safe Area가 등장하게 된 것이다.

이처럼 시스템에 의해 가려질 수 있는 부분의 마진을 자체적으로 가지는 것이 Safe Area이다.

`iPhone 8 vs iPhone X safe area (Portrait Orientation)`

<p align="center"><img src= "https://miro.medium.com/max/1114/1*XJro6ujERnKVXIcDVXy6kw.png" width="50%" height="50%">


`iPhone 8 vs iPhone X safe area (Landscape Orientation)`
<p align="center"><img src= "https://miro.medium.com/max/1400/1*rwWMirhEosAfaBnkWOwBpg.png" width="50%" height="50%">

`문제 발생시 해결법은 생략`

<br>

## 4. 스택 뷰(Stack View)란?

오토 레이아웃 기능을 사용하다 보면 객체가 몇 개 없는 간단한 레이아웃은 쉽게 설정할 수 있지만, 객체가 많은 레이아웃은 어렵고 복잡하다. 어떤 객체를 맞추면 다른 하나가 틀어지고, 디바이스를 변경하면 이상해지기도 한다.

이럴 때 쉽게 사용할 수 있는 객체가 바로 스택 뷰(Stack View)이다. 스택 뷰는 객체를 모아서 관리할 수 있는 뷰 컨테이너로, 별다른 제약 조건을 사용하지 않아도 내부 객체들을 원하는 모양으로 정렬할 수 있다.

스택 뷰는 다른 객체들과 마찬가지로 라이브러리(Library)에서 가져올 수 있으며, Horizontal Stack View(세로형)와 Vertical Stack View(가로형)가 있다.

스택 뷰끼리 중첩해서 표 모양의 레이아웃도 만들 수 있다.

`예시`

간단한 스택 뷰를 만들어보자.

<p align="center"><img src= "https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/Art/Simple_Stack_View_Screenshot_2x.png" width="50%" height="40%">

가로형 스택 뷰를 1개를 사용하여 label, image view, and button을 배치한다.

<p align="center"><img src= "https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/Art/simple_stack_2x.png" width="50%" height="40%">

스택 뷰 안의 뷰 간 간격을 주고 싶을 땐 Spacing에 값을 넣어주면 된다. 그 외에 Alignment와 Distribution, Axis, Value 등도 설정해준다.

 코드로 보면 아래와 같다.

```swift
1. Stack View.Leading = Superview.LeadingMargin
2. Stack View.Trailing = Superview.TrailingMargin
3. Stack View.Top = Top Layout Guide.Bottom + Standard
4. Bottom Layout Guide.Top = Stack View.Bottom + Standard
```


<br>

---
## 참고 자료

[Understanding Auto Layout](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/index.html)

[iOS Safe Area](https://medium.com/rosberryapps/ios-safe-area-ca10e919526f)

[[iOS] 오토레이아웃(AutoLayout)과 Layout 개념](https://jinshine.github.io/2018/06/07/iOS/오토레이아웃(AutoLayout)과%20Layout%20개념/)

[도서 : The 친절한 Swift 프로그래밍 Zero(V 4.0)](http://www.kyobobook.co.kr/product/detailViewKor.laf?barcode=9791195418909)