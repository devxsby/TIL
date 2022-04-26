# tabBarController의 didSelect와 shouldSelect

## 1. tabBarController(_:didSelect:)

### 1-1. 선언

사용자가 탭 바에서 탭 바 아이템을 선택했음을 위임으로 알린다.

```swift
optional func tabBarController(_ tabBarController: UITabBarController, 
                     didSelect viewController: UIViewController)
```

### 1-2. 파라미터

**tabBarController**

뷰 컨트롤러를 포함하는 탭 바 컨트롤러

**viewController**

사용자가 선택한 뷰 컨트롤러. iOS v3.0 이상에서는 이미 선택된 뷰 컨트롤러와 동일한 뷰 컨트롤러가 될 수 있다.

---

## 2. tabBarController(_:shouldSelect:)

### 2-1. 선언

특정 뷰 컨트롤러를 활성화 할 지 여부를 위임으로 묻는다.

```swift
optional func tabBarController(_ tabBarController: UITabBarController, 
                  shouldSelect viewController: UIViewController) -> Bool
```

### 2-2. 파라미터

**tabBarController**

뷰 컨트롤러를 포함하는 탭 바 컨트롤러

**viewController**

사용자가 누른 탭에 속하는 뷰 컨트롤러

## 2-3. 반환 값

뷰 컨트롤러의 탭을 선택해야 하는 경우 true이고, 현재 탭을 활성 상태로 유지해야 하는 경우 false이다.

---

### 공식 문서

[tabBarController(_:didSelect:)](https://developer.apple.com/documentation/uikit/uitabbarcontrollerdelegate/1621173-tabbarcontroller)


[tabBarController(_:shouldSelect:)](https://developer.apple.com/documentation/uikit/uitabbarcontrollerdelegate/1621166-tabbarcontroller)