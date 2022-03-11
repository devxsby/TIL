# Git 커밋 메시지 컨벤션

커밋 메시지를 잘 작성하는 것은 협업하는 데에 있어 중요하다.

why?
> 커밋 메시지를 잘 작성하면
> 
> 1. 커밋 로그의 가독성이 높아지고
> 
> 2. 협업과 리뷰를 위한 소통이 원활해지며
> 
> 3. 프로젝트 유지보수성이 높아진다.

유다시티(Udacity)에서는 **Git Commit Message Style Guide**를 제공하고 있다.

<br>

## 커밋 메시지의 구조
```
<type> : subject // 필수

<body>

<footer>
```

<br>

### Type (타입)

어떤 의도로 커밋했는지 type에 명시한다.

유다시티에서는 다음 7가지를 권장한다.

* Feat: 새로운 기능 추가

* Fix: 버그 및 오류 수정

* Docs: 문서 수정 (ex: README.md)

* Style: 코드 변화와 관련없는 포맷팅(공백, 세미콜론 누락 수정 등)

* Refactor: 새로운 기능이나 버그 수정 없이 코드 리팩토링

* Test: 테스트 코드 추가

* Chore: 빌드 수정, 패키지 매니저 수정

<br>

### Subject (제목)

* 대문자로 시작하는 명령형 문장으로 쓴다. (반대는 Don't)

* 영문 기준 50자를 넘지 않도록 한다.

* 제목 행 끝에 마침표(.)를 넣지 않는다.

<br>

### Body (본문)

* How(어떻게)보다는 What(무엇)과 Why(왜)를 설명한다.

* 72자가 넘어가면 개행한다.

* 현재 시제를 사용한다.

<br>

### Footer (꼬리말, 푸터)

* issue tracker ID를 참조할 때 쓴다.

* 어떤 이슈와 관련된 부분인지, 참고할 다른 이슈는 어떤 것이 있는 지 작성할 수 있다.

<br>

`다음은 위 규칙을 따르는 커밋 예시다`

```
Fix: 상품 정보 입력시 필수 정보인 상품 사진 url 예외처리

- 필수 입력 값인 image url이 body에 담겨있지 않을 때 key error 오류 처리

- Return되는 오류 메시지 수정 (500 error -> 400 key error with 'image_url')
```

만약 이슈 트래커를 사용한다면 다음처럼 내용 하단에 참조를 추가한다.

```
Feat: 추가 로그인 함수

- 로그인 API 개발

해결: #123
참고: #456, #789
관련: #48, #49
```

개인의 사용 빈도에 따라 적절하게 Add나 Remove 등 타입을 추가해서 쓰자.

```
Add: User app 생성 및 회원가입 엔드포인트 추가

- 유저 앱을 만들고, 유저 모델 클래스 생성

- 회원가입 엔드포인트 구현
```

```
Remove: data csv 파일 삭제

- 크롤링 결과 저장한 csv이 git에 잘못 올라와 해당 파일 삭제
```

추가로

1. 검토자가 알고 있는 것을 가정하지 말고 확실하게 설명하자.

2. 자신 코드가 직관적으로 바로 파악될 수 있다고 착각하지 말자.

3.  하나의 커밋에는 하나의 수정사항, 하나의 이슈를 해결한 내용만 남긴다.

4.  팀에서 정한 Commit 규칙을 따르자

<br>

---

<br>

아래는 Harry's diary님의 블로그에서 가져온 템플릿이다.

이렇게 미리 만들어놓고 복붙해서 쓰면 좋겠다 !

<br>

![템플릿](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FodS0l%2FbtrcrNCg6gV%2F3QvtTdnaLnN7CpcbKdNgU1%2Fimg.png)

---

## 참고 자료
[Git 커밋 메시지 작성법](https://blog.weirdx.io/post/33832)

[<Git> 커밋 메시지 컨벤션 : 유다시티](https://haesoo9410.tistory.com/300)

[Git&Github 깃 터미널 명령어와 커밋 메시지 작성법](https://velog.io/@palza4dev/TIL-28.-GitGithub-커밋-메시지-작성법)
