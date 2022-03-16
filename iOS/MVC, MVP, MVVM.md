# 디자인 패턴 MVC, MVP, MVVM 비교

## 1. MVC
* MVC = Model + View + Controller
  
  ### 1. 구조
   
   <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F7IE8f%2FbtqBRvw9sFF%2FAGLRdsOLuvNZ9okmGOlkx1%2Fimg.png"  width="300" height="300"/>

  - Model : 어플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 부분
  - View : 사용자에서 보여지는 UI 부분
  - Controller : 사용자의 입력(Action)을 받고 처리하는 부분
  
  <br>

  ### 2. 동작
  - 사용자의 Action들이 Controller에 들어오게 됨
  - Controller는 사용자의 Action을 확인하고 Model을 업데이트함
  - Controller는 Model을 나타내 줄 View를 선택함
  - View는 Model을 이용하여 화면에 나타냄
    
  <br>

  ### 3. 특징
  - Controller는 여러 개의 View를 선택할 수 있는 1:n 구조
  - Controller는 View를 선택할 뿐 직접 업데이트 하지는 않음
  
  <br>

  ### 4. 장점
  - 가장 널리 사용되고 있는 패턴이고 가장 단순함

  <br>

  ### 5. 단점
  - View와 Model 사이의 의존성이 높기 때문에 어플리케이션이 커질 경우 복잡해지고 유지보수가 어려움
   
<br>

## 2. MVP
* MVP = Model + View + Presenter
  
  ### 1. 구조
 
   <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FclZlsT%2FbtqBTLzeUCL%2FIDA8Ga6Yarndgr88g9Nkhk%2Fimg.png"  width="300" height="300"/>

  - Model : 어플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 부분
  - View : 사용자에서 보여지는 UI 부분
  - Presenter : View에서 요청한 정보로 Model을 가공하여 View에 전달해주는 부분
  
  <br>

  ### 2. 동작
  - 사용자의 Action들이 View를 통해 들어옴
  - View는 데이터를 Presenter에 요청함
  - Presenter는 Model에게 데이터를 요청함
  - Model은 Presenter에서 요청받은 데이터를 응답함
  - Presenter는 View에게 데이터를 응답함
  - View는 Presenter가 응답한 데이터를 이용하여 화면을 나타냄
    
  <br>

  ### 3. 특징
  - Presenter는 View와 Model의 인스턴스를 가지고 있어 둘을 연결하는 접착제 역할을 함
  - Presenter와 View는 1:1 관계
  
  <br>

  ### 4. 장점
  - View와 Model의 의존성이 없음 (Presenter를 통해서만 데이터를 전달받기 때문)

  <br>

  ### 5. 단점
  - 어플리케이션이 복잡해질 경우 View와 Presenter 사이의 의존성이 높아짐

<br>

## 3. MVVM
* MVVM = Model + View + View Model
  
  ### 1. 구조
   <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FCiXz0%2FbtqBQ1iMiVT%2FstaXr7UO95opKgXEU01EY0%2Fimg.png"  width="300" height="300"/>

  - Model : 어플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 부분
  - View : 사용자에서 보여지는 UI 부분
  - View Model : View를 표현하기 위해 만든 View를 위한 Model. View를 나타내기 위한 Model이자 데이터 처리를 하는 부분
  
  <br>

  ### 2. 동작
  - 사용자의 Action들이 View를 통해 들어옴
  - View에 Action이 들어오면, Command 패턴으로 View Model에 Action을 전달함
  - View Model은 Model에게 데이터를 요청함
  - Model은 View Model에게 요청받은 데이터를 응답함
  - View Model은 응답받은 데이터를 가공하여 저장함
  - View는 View Model과 Data Binding하여 화면을 나타냄

  <br>

  ### 3. 특징
  - Command 패턴과 Data Binding 두 가지 패턴을 사용하여 구현됨
  - Command 패턴과 Data Binding을 이용하여 View와 View Model 사이의 의존성을 없앰
  - View Mode과 View는 1:n 관계
  
  <br>

  ### 4. 장점
  - View와 View Model 사이의 의존성을 없고, Command 패턴과 Data Binding을 이용하여 View와 View Model 사이의 의존성 또한 없앤 디자인 패턴
  - 각각의 부분은 독립적이기 때문에 모듈화하여 개발할 수 있음

  <br>

  ### 5. 단점
  - View Model의 설계가 쉽지 않음
   
<br>

---
## 참고 자료
[MVC, MVP, MVVM 비교](https://beomy.tistory.com/43)

[MVC, MVP, MVVM, MMVI](https://brunch.co.kr/@oemilk/113)