# iOS 앱의 구조와 라이프 사이클

## 앱의 프로세스 구조

### 1. 엔트리 포인트

* 기본적으로 만들어지는 main()함수에서 UIApplicationMain을 호출
* 시스템으로부터 전달받은 두 개의 인자값과 AppDelegate 클래스를 이용하여 UIApplicationMain()함수 호출
* 결과로 UIApplication 객체를 반환
* 생성된 UIApplication 객체는 UIKit 프레임워크에 속해있으므로 이후 앱 제어권은 UIKit 프레임워크로 이동

```swift
#import <UIKit/UIKit.h>
#import "AppDelegate.h"

int main(int argc, char *argv[]){
    @autoreleasepool {
        return UIApplicationMain(argc, argv, nil, NSStringFormClass) [AppDelegate class]))
    }
}
```
---
* UIApplicationMain() 함수
  - iOS 앱에 속하는 부분의 엔트리 포인트
  - 앱의 핵심 객체를 생성하는 프로세스를 핸들링
  - storyboard 파일 -> 앱의 유저 인터페이스를 읽어들임 & 커스텀 코드 호출 -> 앱 생성 초기에 필요한 설정 구현
  - 이벤트를 입력받기 위한 이벤트 루프 실행

<br>

* UIApplication을 생성
  - 앱의 본체라고 할 수 있는 객체
  - 커스텀 코드, 객체, 앱의 기능 모두 UIApplication에 포함되어 있는 하위 객체임
  - 모바일 디바이스에 설치된 앱을 실행하면 초기 구동 과정을 거쳐 앱 프로세스가 메모리에 등록되는데, 이 때의 앱 프로세스가 곧 UIApplication 객체라고 보아도 됨
  
<br>

* UIApplication 객체의 역할
  - 이벤트 루프, 다른 높은 수준의 앱 동작을 관리
  - 푸시 알림과 같은 특수한 이벤트를 커스텀 객체(Delegate)에게 알려주기도 함
  - 보통 서브 클래싱 없이 그대로 사용하는데, 서브 클래싱이 필요한 경우 UIApplication 객체는 AppDelegate를 대리 객체로 내세우고 커스텀 코드를 처리할 수 있도록 약간의 권한을 부여함

<br>

* AppDelegate 객체
  - iOS 앱 내에서 오직 하나의 인스턴스만 생성되도록 시스템적으로 보장받음
  - 앱 전체의 생명 주기를 함께 함
  - **AppDelegate 객체에 데이터를 저장하면 앱을 종료할 때까지 계속 데이터를 유지**
  - 앱의 초기 데이터 구조를 설정하기 위해 사용되기도 함

<br>

---

### 2. iOS 12 이전 앱의 라이프 사이클 : App-Based Life-Cycle Events

iOS 12 이하 버전에서는 scene을 지원하지 않기 때문에 UIKit는 모든 life cycle 관련 이벤트들을 **UIApplicationDelegate** 객체에게 전달한다.

App delegate는 앱에 존재하는 모든 window들을 관리하는데, 결과적으로 앱의 상태 변화는 외부 디스플레이의 content를 포함한 전체 UI에 영항을 미친다.

<p align="center"><img src="https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F404406%2F0c50ed72-c71f-f14d-e964-cdc97d52cada.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&w=1400&fit=max&s=eb88c65dfba4ea78061ac805e41642ab" width = "400px"></p>

#### 1. Not Running(Terminated)
 
- 앱이 **실행되지 않았거나, 완전히 종료**된 상태

- 실행되고 난 후, 시스템은 UI가 화면에 보여야한다면 앱을 inactive 상태에, 아니라면 background 상태에 둔다.

- foreground로 시작할 때 시스템은 자동으로 앱을 active상태로 전환한다.
 
#### 2. Inactive (Foreground)
 
- 앱이 foreground에서 **실행중이지만 아무런 이벤트를 받지 않고 있는** 상태

- Active 상태로 넘어가기 전에 반드시 이 상태를 거쳐야 한다.
 
#### 3. Active (Foreground)
 
- 앱이 foreground에서 실행중이며 **이벤트를 받을 수 있는** 상태
 
#### 4. Background
 
- 앱 사용 중에 **다른 앱을 실행하거나 홈화면으로 나갔을 때**의 상태

- Background에서 동작하는 코드를 추가하면 suspended 상태로 넘어가지 않고 Background 상태를 유지할 수 있다. (ex. 음악을 실행하고 홈 화면으로 나가도 음악이 재생 중인 상태)
 
#### 5. Suspended
 
- 앱이 background에 있지만 실행되는 코드가 없는 상태

- 메모리가 부족한 상황이 오면 iOS System은 foreground에 있는 앱의 여유 메모리 공간 확보를 위해 이 상태에 있는 앱들을 특별한 알림없이 정리할 수도 있다. 코드는 실행되지 않으며 앱을 다시 실행할 경우 **빠른 실행을 위해 메모리에만** 올라가 있다. **메모리가 부족한 상황**이 되면 iOS는 suspended 상태에 있는 앱들을 메모리에서 **해제**시켜 메모리 공간을 확보한다.

<br>

---

### 3. iOS 13 이상 앱의 라이프 사이클 : Scene-Based Life-Cycle

`Scene이란?`

* Scene은 기기에서 실행 중인 앱 UI의 한 인스턴스를 의미(사용자는 한 앱을 동시에 여러 개 실행 가능)

* 유저는 각 앱에 대해 여러 Scene을 만들고 각 Scene마다 다른 상태가 될 수 있음

* 하나의 앱은 여러 개의 Scene을 갖을 수 있으며, 새로운 Scene을 만들어 달라고 요청하면 unattached상태로 만듦

* Scene은 멀티윈도우때문에 생겼다..!

<p align="center"><img src="https://developer.apple.com/design/human-interface-guidelines/ios/images/multitasking/app-switcher-iphone_2x.png" width = "200px"></p>

iOS 13 이상에서 사용자는 앱UI의 여러 복사본을 만들어 App Switcher(인디케이터바 위로 올리면 나오는 것 혹은 홈 버튼 더블클릭)에서 전환할 수 있게 되었는데, 앱 UI의 각 사본마다 Scene Object를 사용하여 화면에 표시되는 window, views, view controllers를 관리한다.

사용자가 새로운 Scene을 요청하면 UIKit은 Scene Object를 만들고 초기설정을 처리한다. 앱은 지원하는 Scene type과 이 Scene들을 관리하는 데 사용하는 object들을 반드시 선언해야 한다. 선언은 info.plist에서 정적으로 하거나 런타임에서 동적으로 할 수 있다.

Scene을 사용하지 않을 수도 있는데 ... 멀티윈도우를 사용하고 싶다면 쓰는게 좋다. (**Apple 권장**)

각 Scene은 자신만의 라이프 사이클이 있기 때문에 다른 실행 상태에 있을 수 있다. 예를 들어 한 Scene이 foreground에 있는 동안 다른 Scene은 background에 있을 수 있다.

<p align="center"><img src="https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F404406%2Ffeaf6a1d-03db-9bd9-a33e-cdc636176433.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&w=1400&fit=max&s=a5e49840ffc6adc14abfd32a3df0d215" width = "600px"></p>

위의 App-Based Life-Cycle Events과 겹치므로 간단히 설명하겠다.

#### 1. Unattached
 
- Scene은 만들어졌으나 아직 앱에 연결되지 않은 상태
 
#### 2. Foreground Inactive
 
- 현재 UI에 보이는 상태이나 event를 수신할 수 없는 상태
 
#### 3. Foregroud Active
 
- 현재 UI에 보이는 상태이며 event를 수신 가능한 상태
 
#### 4. Background
 
- 현재 UI에 보이지 않는 상태

- Background 상태에서는 screen 밖에 있으므로 가능한 최소한의 작업만 하거나 아무것도 하지 말아야 한다.
 
#### 5. Suspended
 
- Background에 있다 특별한 작업을 하지 않는 경우 전환되는 상태

<br>

---

### 4. App-Based Life-Cycle 와 Scene-Based Life-Cycle의 차이점

1. Scene-based는 inactive, active라는 네이밍 앞에 foreground를 명시적으로 붙였다.
 
2. App-based는 not running과 suspended사이에 화살표가 있는데,
    Scene-based는 unattached와 suspended 사이 화살표가 없다
 
3. App-based는 suspended에서 inactive로 가는 화살표가 있었는데,  
    Scene-Based는 suspended에서 foreground inactive로 가는 화살표가 없다. 

<br>

---

### 5. App's Life Cycle 관련 Delegate 함수들

앱의 상태가 변경되면 UIKit는 적절한 delegate object의 method를 호출해서 우리에게 알려주는데, **iOS 12 이전** 버전에서는 **UIApplicationDelegate를 사용**하여 life cycle event에 응답한다. **iOS 13이상**에서는 **UISceneDelegate를 사용**하여 scene 기반 앱의 life cycle event에 응답한다. 

<br>

#### 1. App-Based Life-Cycle Events  (iOS 12 and earlier)

* configurationForConnecting
  - 새 scene을 만들 때 사용할 UIKit의 구성 데이터를 검색

* didDiscardSceneSessions
  - app switcher에서 하나 이상의 앱 장면을 닫았다고 알림

* applicationDidBecomeActive
  - 앱이 활성화되었음을 알림

* applicationWillResignActive
  - 앱이 곧 비활성화될 것임을 알림

* applicationDidEnterBackground
  - 앱이 이제 백그라운드에 있음을 알림

* applicationWillEnterForeground
  - 앱이 포그라운드로 진입하려고 한다는 것을 알림

* applicationWillTerminate
  - 앱이 종료되려고 할 때 대리자에게 알림

<br>

#### 2. Scene-Based Life-Cycle Events  (iOS 13 and later)

* scene(UIScene, willConnectTo: UISceneSession, options: UIScene.ConnectionOptions)
  - 앱에 장면 추가에 대해 알림

* sceneDidDisconnect
  - UIKit이 앱에서 scene을 제거했음을 알림

* sceneWillEnterForeground
  - scene이 Foreground에서 실행되기 시작하고 사용자에게 표시될 것임을 알림

* sceneDidBecomeActive
  - scene이 활성화되었으며 이제 사용자 이벤트에 응답하고 있음을 알림

* sceneWillResignActive
  - 장면이 활성 상태를 끝내고 사용자 이벤트에 대한 응답을 중지하려고 한다고 대리자에게 알림

* sceneDidEnterBackground
  - scene이 백그라운드에서 실행 중이며 더 이상 화면에 표시되지 않음을 알림

<br>

---

### 참고 자료

[UIKit](https://developer.apple.com/documentation/uikit)

[Cocoa Application Layer](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/OSX_Technology_Overview/CocoaApplicationLayer/CocoaApplicationLayer.html#//apple_ref/doc/uid/TP40001067-CH274-SW1)

[Managing Your App's Life Cycle](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle)

[Specifying the Scenes Your App Supports](https://developer.apple.com/documentation/uikit/app_and_environment/scenes/specifying_the_scenes_your_app_supports)

[iOSアプリのライフサイクル](https://qiita.com/KenNagami/items/766d5f95940c76a8c3cd)

[iOS13のScene-Basedライフサイクル(UISceneDelegate)](https://qiita.com/KenNagami/items/cbbe98b736fbdb24fef8)

[App LifeCycle (앱의 생명주기)](https://velog.io/@llghdud921/App-LifeCycle-앱의-생명주기)

[[App's Life Cycle] App-Based Life-Cycle과 Scene-Based Life-Cycle](https://eunjin3786.tistory.com/163)