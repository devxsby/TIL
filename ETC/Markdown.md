# 1. 마크다운에 관하여

## 1.1. 마크다운이란?

[Markdown](http://whatismarkdown.com/)은 텍스트 기반의 마크업언어로 2004년 존 그루버와 에런 스워츠 와의 협업에 의해 만들어졌으며, 쉽게 쓰고 읽을 수 있으며 HTML로 변환이 가능하다.

특수기호와 문자를 이용한 매우 간단한 구조의 문법을 사용하여 웹에서도 보다 빠르게 컨텐츠를 작성하고 보다 직관적으로 인식할 수 있다.

깃헙의 저장소Repository에 관한 정보를 기록하는 README파일이나 온라인 문서 혹은 일반 텍스트 편집기로 문서 양식을 편집할 때 쓰인다.

<br>

## 1.2. 마크다운의 장단점

### 1.2.1. 장점

	1. 문법이 간결하고 쉽다.
	2. 별도의 도구없이 작성가능하다.
	3. 다양한 형태로 변환이 가능하다.
	4. 텍스트(Text)로 저장되기 때문에 용량이 적어 보관이 용이하다.
	5. 지원하는 프로그램과 플랫폼이 다양하다. (Github, Discord 등)

### 1.2.2. 단점

	1. 표준이 없다. (사용자마다 문법이 상이할 수 있음)
	2. 모든 HTML 문법을 대신하지 못한다.

<br>

# 2. 마크다운 문법

## 2.1. 헤더 (Headers)

* 제목과 부제목
    ```
    제목 
    ===
    부제
    ---
    ```

    제목 
    ===
    부제목
    ---

<br>

* 글머리 : #1개 ~ #6개까지만 지원하며 뒤로 갈수록 작아진다.
  ```
  # 가
  ## 나
  ### 다
  #### 라
  ##### 마
  ###### 바
  ####### 사 // 안됨
  ```
  # 가
  ## 나
  ### 다
  #### 라
  ##### 마
  ###### 바
  ####### 사

* **문자 중앙정렬** : center를 앞 뒤로 넣어 감싸주면 된다.
  ```
  <center>Hello, World!</center>
  ```

  <center>Hello, World!</center>

  <br>

  > 글머리랑 합쳐서 크기를 조정하고 정렬할 수도 있다.
  ```
  ## <center>Hello, World!</center>
  ```

  ## <center>Hello, World!</center>

<br>

## 2.2. 블럭 인용문 (Block Quote)

이메일에서 사용하는 ```>``` 블럭인용문자를 이용한다.

3개까지 사용 가능하다.
```
> 첫 번째 인용문.
>	> 두 번째 인용문.
>	>	> 세 번째 인용문.
```
> 첫 번째 인용문.
>	> 두 번째 인용문.
>	>	> 세 번째 인용문.

이 안에서는 다른 마크다운 요소를 포함할 수 있다.
> ### 대한
> * 민국
>	```
>	만세
>	```

<br>

## 2.3. 목록 (List)

### 순서대로(Ordered) 정렬

순서있는 목록은 숫자와 점을 사용한다.
```c
// 알아서 내림차순으로 해줌
1. 첫 번째
3. 두 번째
2. 세 번째
```
1. 첫 번째
3. 두 번째
2. 세 번째

<br>

### 순서없는(Unordered) 목록

글머리 기호 `*`, `+`, `-` 을 붙인다.
```
* apple
  * banana
    * cat
      * dog
        * elephant
```
* apple
  * banana
    * cat
      * dog
        * elephant

혼합해서 사용하는 것도 가능하다.

이렇게 사용하면 들여쓰기를 몇 번째로 한 건지 인식하기 편하다.

```
* 1단계
  - 2단계
    + 3단계
```

* 1단계
  - 2단계
    + 3단계

<br>

## 2.4. 코드 (Code)

### 2.4.1. 들여쓰기

```
This is a normal paragraph:

    This is a code block. \\ Space바 4번 혹은 Tab바 2번
```

<br>

### 2.4.2. 코드블럭

코드블럭은 다음과 같이 2가지 방식을 사용할 수 있다:

* 1번.  Back quote ( ` ) 또는 물결( ~ ) 3개(```)로 감싸는 방법
  
  `주의 : 숫자 1번키 옆의 표시 (```)이다. 작은따옴표(''') 아님`

<pre>
<code>
```
let name: String = "Yoon"
print("My name is \(name).")
```

~~~
This is code blocks.
~~~
</code>
</pre>

```
let name: String = "Yoon"
print("My name is \(name).")
```

~~~
This is code blocks.
~~~

**깃허브**에서는 코드블럭코드(```) 옆에 언어를 지정해주면 [Syntax color](https://docs.github.com/en/github/writing-on-github/creating-and-highlighting-code-blocks#syntax-highlighting)가 적용된다.

<pre>
<code>
```swift
let name: String = "Yoon"
print("My name is \(name).")
```
</code>
</pre>

```swift
let name: String = "Yoon"
print("My name is \(name).")
```

* 2번. HTML의 `<pre><code>{~code~}</code></pre>` 이용방식

  **생략함**

<br>

## 2.5. 수평선 (Horizon)

페이지 나누기 용도로 많이 사용한다.

```<hr/>```과 '*', '-', '_' 를 사용하는 4가지 방법이 있다.

```
<hr/>

* * *

***

*****

- - -

---

___
```

* 적용 예

<hr/>

* * *

***

*****

- - -

---

___

<br>

## 2.6. 링크 (Link)

```
사용문법: [Title](link)
적용예: [Google](https://google.com)
```
[Google](https://google.com)

* 꺽쇠표시를 활용해서 링크를 보이게 할 수 있다.

```
사용문법: <link>
적용예: <https://google.com>
```
<https://google.com>

<br>

## 2.7. 강조 (Emphasis)

```c
// 이텔릭체 : 별표(*) 또는 언더바(_)로 1번 감싸기
*이텔릭체*
_이텔릭체_

// 볼드체 : 별표(*) 또는 언더바(_)로 2번 감싸기
**볼드체**
__볼드체__

// 물결(~) 2번 감싸기
~~취소선~~

// 밑줄은 마크다운 언어로 안된다. HTML의 방식을 사용한다.
<u>밑줄</u>
```

*이텔릭체*
_이텔릭체_

**볼드체**
__볼드체__

~~취소선~~
  
<u>밑줄</u>

* 여기서 **백슬래쉬 이스케이프(`\`)**를 이용하면 기능을 가진 문자들을 텍스트처럼 사용 할 수 있다.
  
  주의할 점은 앞과 뒤의 형식이 똑같이 백슬래쉬 뒤의 특수문자이다.
  
  감싸는 형태가 아니다.
  ```
  \__ 이스케이프 문자로 양옆에 언더 바 표시 가능 \__
  ```
  __ 이스케이프 문자로 양옆에 언더 바 표시 가능 __

<br>

## 2.8. 이미지 (Image)

* 2.8.1. 간단한 사용법

링크 사용법과 비슷한데 앞에 `!`표시가 붙음
```h
// 이미지만 삽입
![이미지 설명](이미지 링크)
![이미지 설명](이미지 링크 "부가 설명")

// 링크를 포함해서 이미지 삽입
[![이미지 설명](이미지 링크)](연결될 url)
```

![핑크춘식](https://pbs.twimg.com/media/E9s0ao1VEAM3uYi?format=jpg&name=small "춘식이의 그림일기")

<br>

* 2.8.2. 이미지 크기 조정하기

**위의 1번 방식은 사이즈 조절 기능은 없기 때문에 아래의 html 태그 방식을 사용하면 이미지의 크기를 조정할 수 있다.**

위의 방식의 링크 뒤에 ` width="" height=""` 부분을 추가하면 된다.

```html
<img src="이미지 링크" width="200px" height="200px"> // 픽셀(px) 설정
<img src="이미지 링크" width="50%" height="50%"> // 비율(%) 설정
```

<img src="https://pbs.twimg.com/media/E9s0ao1VEAM3uYi?format=jpg&name=small" width="200px" height="200px">

<img src="https://pbs.twimg.com/media/E9s0ao1VEAM3uYi?format=jpg&name=small" width="50%" height="50%">

<br>

* 2.8.3. 이미지 정렬하기

마찬가지로 <img> 태그로 이미지를 첨부하고, 이미지 태그를 p태그로 감싼다. 크기 조정도 같이 할 수 있다.

- `왼쪽` 정렬 (**기본값**) : img 태그에 left로 align 속성을 준다.

<img src="https://pbs.twimg.com/media/E9s0aFdVUAEHphb?format=jpg&name=small" width="100px" height="100px" align="left">

<br>

```html
<img src="이미지 링크" width="100px" height="100px" align="left">
```

<br>

- `오른쪽` 정렬 : img 태그에 right로 align 속성을 준다.

<img src="https://pbs.twimg.com/media/E9s0Z08UYAIxtI7?format=jpg&name=small" width="100px" height="100px" align="right">

<br>

```html
<img src="이미지 링크" width="100px" height="100px" align="right">
```

<br>

- **`가운데` 정렬** : img 태그를 p태그로 감싸고 center로 align 속성을 준다. 문자 정렬처럼 center태그로 감싸는 것도 가능하다.

```html
<p align="center"><img src="이미지 링크" width="100px" height="100px"></p>

<center><img src="이미지 링크" width="100px" height="100px"></center>
```

<br>

<p align="center"><img src="https://pbs.twimg.com/media/E9s0aWRVcAImtCu?format=jpg&name=small" width="100px" height="100px"></p>

<br>

<center><img src="https://pbs.twimg.com/media/E9s0aWRVcAImtCu?format=jpg&name=small" width="100px" height="100px"></center>

<br>

## 2.9. 줄바꿈 (Line Breaks)
3칸 이상 띄어쓰기(` `)를 하거나 HTML의 `<br>`을 넣으면 줄이 바뀐다.

```c
줄 바꾸기   // 스페이스바 3번 입력
줄 바꾸기 <br> 확인
```

줄 바꾸기   
줄 바꾸기 <br> 확인

<br>

## 2.10. 표(Table)

마크다운 문법엔 없다... HTML의  `<table>` 태그로 변환해서 표시한다.

헤더 셀을 구분할 때 3개 이상의 -(hyphen/dash) 기호가 필요하다.

헤더 셀을 구분할 때 :(Colons) 기호로 셀(열/칸) 안에 내용을 정렬할 수 있다.
  - 가운데 정렬 :---:
  - 왼쪽 정렬 :---
  - 오른쪽 정렬 ---:

```
| 날짜 | 메뉴 | 가격 |
|:---:|:---:|:---:|
| 12월 31일 | 맥주 | 5000원 |
| 1월 1일 | 떡국 | 8000원 |
```

| 날짜 | 메뉴 | 가격 |
|:---:|:---:|:---:|
| 12월 31일 | 맥주 | 5000원 |
| 1월 1일 | 떡국 | 8000원 |

<br>

---

### 참고문서

[[공통] 마크다운 markdown 작성법](https://gist.github.com/ihoneymon/652be052a0727ad59601)

[MarkDown 사용법 총정리](https://heropy.blog/2017/09/30/markdown/)

[Aligning images](https://gist.github.com/DavidWells/7d2e0e1bc78f4ac59a123ddf8b74932d)