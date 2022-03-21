# View Controller(뷰 컨트롤러)의 라이프 사이클

앱이 실행되면서 여러 event들에 의해 화면이 나오고 사라지는데, 이 과정에서 View Controller가 만들어지고 사라지며 화면을 띄우고 내리는 작업을 수행한다.

네비게이션 컨트롤러의 동작은 자료구조에서의 '스택'과 같다. `LIFO(Last In First Out) 방식 채택`

뷰 컨트롤러에는 자신만의 생명주기가 존재한다. 뷰가 화면에 보여지는 상태의 변화나 뷰의 레이아웃의 변화가 생기면 뷰 컨트롤러는 그에 맞는 메소드를 호출한다.

## 1. UIViewController 라이프 사이클의 흐름

<p align="center"><img src="https://docs-assets.developer.apple.com/published/f06f30fa63/UIViewController_Class_Reference_2x_ddcaa00c-87d8-4c85-961e-ccfb9fa4aac2.png" width = "400px"></p>

뷰의 상태변화와 그에 따라 호출되는 메서드이다.

아래에서 자세히 알아보자.

### 1. init()

- View Controller의 생성자 메서드이다.

- 스토리보드냐 xib/코드냐에 따라 다른 것이 호출된다.

<br>

### 2. loadView()

- 화면에 띄워 줄 뷰를 만드는 메서드로 뷰를 만들고 메모리에 로드한다.

- 일반적으로 사용자는 이 메서드를 직접 호출하면 안된다. [관련 설명](https://leehonghwa.github.io/blog/loadView/)

<br>

### 3. viewDidLoad()

- 뷰 컨트롤러가 뷰 계층을 메모리에 로드한 뒤(loadView() 직후) 시스템에 의해 자동으로 호출된다.

- 메모리가 처음 로드될 때(즉, 화면이 처음 만들어질 때) 한 번만 호출된다. (메모리 경고로 뷰가 사라질 경우 제외)

- 뷰와 관련된 추가적인 초기화 코드가 있을 경우 이 메서드 내부에 작성한다.

`viewDidLoad에서는 한 번 초기화 된 후 변하지 않는 내용에 관한 작업을 수행`

<br>

### 4. viewWillAppear() / viewDidAppear()

**viewWillAppear**
- 뷰 컨트롤러의 Root View가 로드된 이후 Window의 뷰 계층으로 더해지기 직전에 호출되는 메서드이다.

- 다른 뷰로 이동했다가 다시 돌아오면 재호출되므로 그 때 필요한 코드는 이 메서드 내부에 작성한다.

`viewWillAppear에서는 화면이 나타날 때마다 업데이트 해야 하는 내용에 관한 작업을 수행`

**viewDidAppear**
- Window의 Root View가 뷰 계층으로 더해진 직후 호출되는 메서드이다.

- 뷰가 나타났다는 것을 컨트롤러에게 알리는 역할을 하며, 화면에 적용될 애니메이션을 그려준다.

<br>

### 5. viewWillDisappear() / viewDidDisappear()

**viewWillDisappear**
- Window의 Root View가 뷰 계층에서 제거되기 직전 호출되는 메서드이다.

- 뷰가 삭제되려고 하는 것을 뷰 컨트롤러에게 통지하며, 작업한 내용을 되돌리는 작업과 최종 데이터 저장 작업 등을 수행한다.

**viewDidDisappear**
- Window의 Root View가 뷰 계층에서 제거된 직후 호출되는 메서드이다.

- 뷰가 제거되었음을 알려주며, 사라지는 것과 관련된 추가 작업, 마무리작업(리소스 해제 등)을 수행한다.

<br>

### 6. deinit()

- View가 메모리에서 해제된 뒤 호출된다.

<br>

--- 

## 2. View Controller 라이프 사이클 메서드 호출 순서 예시

FirstViewController(이하 A 뷰컨)가 있고 “두 번째 뷰컨트롤러로 이동” 버튼을 눌렀을 때 SecondViewController(이하 B 뷰컨) 로 이동하고 B 뷰컨트롤러의 “닫기” 버튼을 눌러 A 뷰컨트롤러로 이동할 때 ViewController Life Cycle 메서드 호출 순서를 살펴보자.

<p align="center"><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FpCkep%2Fbtq6BD6KQhQ%2F5Nlk0KuNAshr34meHMB9b1%2Fimg.png" width = "400px"></p>

`처음 앱을 실행했을 때`

1. A 뷰컨의 ViewDidLoad
2. A 뷰컨의 ViewWillAppear
3. A 뷰컨의 ViewDidAppear

`두 번째 뷰 컨트롤러로 이동 버튼을 누른 후`
1. B 뷰컨의 ViewDidLoad
2. A 뷰컨의 ViewWillDisappear
3. B 뷰컨의 ViewWillAppear
4. B 뷰컨의 ViewDidAppear
5. A 뷰컨의 ViewDidDisappear

`B 뷰 컨트롤러에 닫기 버튼을 누른 후`
1. B 뷰컨의 ViewWillDisappear
2. A 뷰컨의 ViewWillAppear
3. A 뷰컨의 ViewDidAppear
4. B 뷰컨의 ViewDidDisappear

<br>

---

## 3. NavigationController 가 있을 때 ViewController Life Cycle 메서드 호출 순서

NavigationController 에 RootViewController(이하 Root 뷰컨) 가 있고 TableView에 cell 을 눌러서 DetailViewController((이하 Detail 뷰컨)) 로 이동하고 NavigationBar 의 “Root View Controller” 버튼을 눌러 RootViewController 로 이동할 때의 ViewController Life Cycle 메서드 호출 순서를 살펴보자.

<p align="center"><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FLwJKq%2Fbtq6FtIMYG9%2F7ESi9gZCxEJvFRSxtmhhk0%2Fimg.png" width = "600px"></p>

`처음 앱을 실행했을 때`
1. NavigationController 의 ViewDidLoad
2. NavigationController 의 ViewWillAppear
3. Root 뷰컨의 ViewDidLoad
4. Root 뷰컨의 ViewWillAppear
5. Root 뷰컨의 ViewDidAppear
6. NavigationController 의 ViewDidAppear

`RootViewController 에 셀을 눌렀을 때`
1. Detail 뷰컨의 ViewDidLoad
2. Root 뷰컨의 ViewWillDisappear
3. Detail 뷰컨의 ViewWillAppear
4. Root 뷰컨의 ViewDidDisappear
5. Detail 뷰컨의 ViewDidAppear

`DetailViewController 의 뒤로가기 버튼을 눌렀을 때`
1. Detail 뷰컨의 ViewWillDisappear
2. Root 뷰컨의 ViewWillAppear
3. Detail 뷰컨의 ViewDidDisappear
4. Root 뷰컨의 ViewDidAppear

네비게이션 컨트롤러가 있을 때는 네비게이션 컨트롤러가 없을 때와 다르게 사라질 뷰가 먼저 ViewDidDisappear 가 호출되고 그 뒤에 나타날 뷰의 ViewDidAppear 가 호출되었다.

<br>

---

### 참고 자료


[UIViewController](https://developer.apple.com/documentation/uikit/uiviewcontroller)

[init(nibName:bundle:)](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621359-init)

[The Role of View Controllers](https://developer.apple.com/library/archive/featuredarticles/ViewControllerPGforiPhoneOS/index.html#//apple_ref/doc/uid/TP40007457-CH2-SW1)

[[ios] UIViewController Lifecycle](https://baked-corn.tistory.com/32)

[[ios] UIViewController LifeCycle - 추가](https://baked-corn.tistory.com/132)

[iOS ViewController Life Cycle](https://kiljh.tistory.com/239)

[UIViewController: UIViewController](https://velog.io/@yohanblessyou/작성중-View의-Life-Cycle)

[Understand the View Controller LifeCycle](https://velog.io/@delmasong/Understand-the-View-Controller-LifeCycle)

[View Controller의 생명주기(Life-Cycle)](https://zeddios.tistory.com/43)