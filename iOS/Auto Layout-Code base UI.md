# Auto Layout Code base로 UI 구현하기

Auto Layout을 SnapKit 라이브러리를 사용하여 코드로 구현하는 방법에 대해 알아보자.

`그 전에 왜 코드 베이스로 구현하는지에 대해 탐구해보자`

## 1. Storyboard에서 Auto Layout을 구현하는 방법과 Code로 구현하는 방법의 차이

처음 iOS 개발을 공부하면 대부분 Storyboard를 사용하여 UI를 구성한다.

직관적이고 간편하게 마우스 컨트롤을 통해 Auto Layout을 적용할 수 있다는 점이 매력적이다.

하지만, Storyboard로만으로 Auto Layout 구현하는 것에는 한계가 있고, 코드로 작성해야 할 때가 있다.

<br>

### 1. Storyboard는 **Index, Build 시간에 악영향**을 준다.

Storyboard로 오토 레이아웃을 구현하다보면, 초기 프로젝트 Index 시간과 Build 시간에 악영향을 주게 된다.

Index는 코드 자동 완성에 결정적인 역할을 하는 프로세스이기 때문에 Index 과정이 끝난 이후 코드 작성하는 것이 보다 효율적이다.

1-2개의 간단한 레이아웃이면 차이가 거의 없을 수 있으나 프로젝트 크기가 커질수록 이러한 문제를 더욱 더 부각된다.

<br>

### 2. **Xcode 자체 오류로 인해 Storyboard 내 View Controller가 보이지 않을 경우**

Xcode 자체 오류로 인해 종종 Storyboard에서 구현한 화면이 보이지 않을 경우가 있다. 이 때는 시스템 재시동 혹은 XCode 재시동 외에는 마땅한 방법이 없다. 

촉박한 일정의 압박 속에 효율적으로 업무를 진행하려면 IDE의 오류 현상을 최소화하면서 높은 품질의 코드를 생산해내야 하는데 Storyboard의 경우 언급한 문제점으로 인해 업무에 지장이 생길 수 있다.

<br>

### 3. Storyboard의 **Inspector에서 제공하지 않는 옵션의 경우**

layer.cornerRadius나 layer.borderWidth와 같은 속성 값은 Inspector에서 기본적으로 제공하지 않는다.

적용하고 싶을 경우 직접 코드로 IBOutlet 변수에 추가하거나 아니면 Identity Inspector에 있는 User Defined Runtime Attributes에 직접 추가해야 한다.

대표적인 속성이 view.layer.cornerRadius나 view.layer.masksToBound 등이 있다.

<br>

### 4. **Swift UI를 위해** 코드로 View를 구현하는 연습은 도움이 된다.

Apple은 Storyboard를 사용하지 않는 것을 권장하고 있다. iOS 13에 처음으로 발표한 Swift UI를 보면 100% Swift Code로 View Layout을 구현하는 방식을 취하고 있다.

이는 머릿 속에 혹은 미리 어느 정도 뷰를 설계한 후 코드로 레이아웃을 구성하는 연습을 해놓는 것이 도움이 될 수 있다는 것이다.

Auto Layout의 동작 방식의 미묘한 차이가 존재하지만 기본적인 사항들은 동일하며 View를 설계하는 방식에 있어서도 다를 점이 없기 때문에 Code로 구현하는 연습은 추후 Swift UI를 빠르게 학습하여 적용하는 데 도움이 된다.

---

## 2. Code로 Auto Layout 구현해보기

코듣로 오토레이아웃을 구현하는 방법은 3가지가 있다.

- 앵커(Anchor)를 통해서 구현

- NSLayoutConstraint 인스턴스로 구현

- Visual Format Language 를 통해 구현

여기서는 2번 **NSLayoutConstraint 인스턴스로 구현**으로 실습해보겠다.

뷰를 그릴 때에는 다음의 순서를 지켜줘야 한다.

1. UI 요소들 정의하기
2. addSubView로 뷰 추가해주기
3. translatesAutoResizeingMaskIntoConstraints를 false로 설정하기
4. 제약조건(constraints) 설정하기





---

## 3. SnapKit으로 Auto Layout 구현해보기

> SnapKit이란?
> 
> SnapKit은 iOS 및 OS X에서 Auto Layout을 쉽게 하기 위한 DSL(Domain-specific Language)이다.
> Cocoapods으로 사용할 수도 있고 Swift Package Manager (SPM)으로 사용할 수도 있다.

ㅇ

---

### 참고자료

[NSLayoutConstraint](https://developer.apple.com/documentation/uikit/nslayoutconstraint)

[Programmatically Creating Constraints](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html#//apple_ref/doc/uid/TP40010853-CH16-SW1)

[[iOS] Code로 AutoLayout 다루기](https://stickode.tistory.com/359)

[[ios] Auto Layout Programmatically - 1](https://baked-corn.tistory.com/36)

[[iOS - UI Custom] 11. Auto layout (programmatically)](https://ios-development.tistory.com/67)

[Auto Layout을 코드로 구현해봅시다](https://terry-some.tistory.com/122)

[[Swift] AutoLayout 코드로 그리기🖌 (Code base UI 그리기)](https://macgongmon.club/31)