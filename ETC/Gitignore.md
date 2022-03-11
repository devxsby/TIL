# Gitignore

## .gitignore 이란?

Github에 파일을 업로드 하다보면 제외해야 할 파일들이 생기는 상황이 있다.

예를 들어 용량이 너무 크거나, 내 로컬 개발 환경에 종속적인 파일이 있을 때 혹은 보안이 중요한 경우일 때 말이다.

이 때 우리는 .gitignore 파일을 생성해서 Github에 업로드 하지 않을 파일들을 지정할 수 있다.

어떤 것을 커밋대상에서 제외시켜야 할 지 궁금하다면 [깃허브 공식 문서](https://github.com/github/gitignore )를 참고하자 !

<br>

## 그렇다면 .gitignore 디렉토리는 어디에 생성해야 하는가?

.gitignore 파일은 .git 폴더와 같은 경로에 있어야 한다.

**즉, 항상 해당 프로젝트의 최상위 폴더에 있어야 한다.**

<br>

**어떻게 만드는가?**

- 해시(#)는 주석역할이다.
  
- 표준 Glob 패턴을 사용한다.

- 슬래시(/)로 시작하면 하위 디렉토리에 적용되지 않는다.
  ```
  /TODO # 현재 폴더 중에서 TODO 폴더에 있는 모든 파일을 ignore
  ```

- 디렉토리는 슬래시(/)를 끝에 사용하는 것으로 표현한다.
  ```
  TODO/ # 프로젝트 전체 폴더 중 TODO라는 폴더명을 사용하는 TODO 폴더의 하위 파일은 모두 무시

  doc/*.txt # doc 폴더 바로 밑에 있는 파일 중 모든 txt 파일 ignore

  doc/**/*.pdf # doc 폴더 하위에 있는 모든 pdf 파일 ignore
  ```

- 느낌표(!)로 시작하는 패턴의 파일은 예외 처리로 하고 무시하지 않는다.
  - 단, 한 번 제외된 폴더 내의 파일들은 다시 추가할 수 없다.
  
  ```
  !lib.a # 무시하는 모든 확장자 .a 파일들 중에서 lib.a 파일은 무시하지 않음
  ``` 

<br>

## 자동으로 찾아서 넣고 싶을 때

<img src="https://www.toptal.com/developers/gitignore/img/gitignore-logo-horizontal@2x.png"  width="300" height="50"/>

직접 추가해도 되고 내용을 자동적으로 생성하고 싶다면 [이 사이트](https://www.toptal.com/developers/gitignore)에 키워드를 검색해보자.

보통 .project 혹은 .classpath 혹은 .settings/ 등이 나온다.

<br>

## .gitignore가 적용되지 않을 때

.gitignore가 제대로 적용되지 않을때도 캐시가 문제일 확률이 높다.

이럴 땐 캐시 내용을 전부 삭제 후 다시 add all 하고 커밋하면 된다

`이렇게요`
```
$ git rm -r --cached
$ git add .
$ git commit -m "git ignore add"
$ git push
```

<br>

## .gitignore 목록에 있어도 추가 하고 싶을 때?

우리는 앞서 .gitignore 파일을 최상단으로 지정하였기 때문에 하위 폴더에도 자동적으로 적용이 된다.

그러나 특정 하위 폴더에서는 다른 ignore 정책을 적용하고 싶을 수도 있다.

그 때는 해당 폴더에 다시 .gitignore 파일을 만들면 상위 정책과 다르게 만들 수 있다.

그냥 바로 추가하고 싶을때는 add 할때 -f(force) 옵션을 사용해서 강제로 추가할 수도 있다.

--- 
<br>

## 참고 자료

[Git(8) .gitignore 이란](https://devbirdfeet.tistory.com/31)

[[git] .gitignore가 적용되지 않을 때(git 캐시 삭제하기)](https://yongdev91.tistory.com/11)
