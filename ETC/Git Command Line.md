# Git 명령어

* 자주 사용하는 것 위주로 정리합니다,
  
* Conflict 발생 시 ... **Just Google it !**

![Git Cheat Sheet](https://res.cloudinary.com/practicaldev/image/fetch/s--Zib71Fgv--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/n082uxea33j6zq3mca7u.png)

<br>

## 1. 설정과 초기화

- $ git init : 현재 위치에서 지역 저장소 생성

- $ git config --global user.name "[사용자명]" : Git 환경에서 사용자 이름을 [사용자명]으로 지정

- $ git config --global user.email "[사용자이메일]" : Git 환경에서 사용자 이메일을 [사용자이메일]으로 지정

- $ git status : Git의 상태를 확인

<br>

## 2. 기본 사용법

- $ git clone [git_path] : Github에 있는 코드 내 로컬로 가져오기

- $ git add [파일.확장자명] : 수정사항 있는 파일을 스테이지에 올림
      * git add . : 전부 올림

- $ git reaet [파일 이름] : 스테이지에 올렸던 파일 다시 내리기

- $ git commit -m "[커밋메시지]" : 커밋 메시지를 붙여 커밋

- $ git push -u orgin master : Local repository의 내용을 처음으로 Remote repository에 올릴 때 사용

- $ git push : Local repository의 내용을 Remote repository에 올리기(보내기)

- $ git pull : Remote repository의 내용을 Local repository로 끌어오기(받기)

- $ git fetch : git 서버에서 최신 코드 받아오기

<br>

## 3. 커밋 다루기

- $ git log : 커밋 내역 확인

- $ git show [커밋 id] : 특정 커밋 내역 확인

- $ git commit --amend : 최신 커밋 새로운 커밋으로 덮어쓰기

- $ git diff [커밋 A id] [커밋 B id] : 두 Commit 간의 차이 비교

- $ git reset [옵션] [커밋 id]: 커밋 되돌리기
     * $ git reset HEAD~n : 현재로부터 n번째 이전 커밋으로 되돌리기  
     * `옵션`
        1. --soft : HEAD가 특정 Commit을 가리키도록 이동
        2. --mixed : staging area도 특정 Commit 처럼 리셋
        3. --hard : working directory도 특정 Commit처럼 리셋
   
- $ git tag [tag 이름] [커밋 id] : 특정 커밋에 태그를 붙임

<br>

## 4. 브랜치

- $ git branch [새 브랜치 이름] : 새로운 브렌치 생성

- $ git branch -b [새 브랜치 이름] : 새로운 브렌치 생성하고 그 브렌치로 바로 이동

- $ git branch -d [기존 브랜치 이름] : 브랜치 삭제하기

- $ git checkout [옮겨갈 기존 브랜치 이름]: 현재 브랜치에서 해당 기존 브랜치로 이동

- $ git checkout -b [옮겨갈 새 브랜치 이름] : 현재 브랜치에서 새 브랜치로 이동

- $ git merge [기존 브랜치 이름] : 현재 브랜치에 다른 브랜치를 병합

- $ git branch -r : 원격 브랜치 목록보기

- $ git branch -a : 로컬 브랜치 목록보기

- $ git branch -m branch [바꾸기전이름] [바꿀이름] : 브랜치 이름 바꾸기

<p align="center">  
  <img src="https://gmlwjd9405.github.io/images/git-stash/git-stash-stashing-changes.png"  width="300" height="200"/>

- $ git stash : 진행 중인 작업을 `임시 저장`하고 싶을 때 사용
  - $ git stash list : 저장한 stash 목록 확인

  - $ git stash apply [stash id] : stash 가져와 적용하기

  - $ git stash drop [stash id] : 해당 stash 삭제

  - $ git stash pop : apply 하고 drop 기능 합친 것

  - $ git stash pop : 나중에 꺼내와서 작업 마무리 하기
    - **apply, pop 사용할 때 뒤에 "--index" 옵션을 붙이면 스테이지 상태까지 같이 복원 됨**

  - $ git stash show -p [stash id] | git apply -R : stash 되돌리기

<br>

## 5. 기타

- $ git help [command name] : 공식 매뉴얼 확인하기(나가기 q 입력)
  * $ git help -a : 깃 명령어 전체 리스트 확인 

<br>

---

## 참고 자료

[Git Cheat Sheet: Commands and Best Practices](https://www.jrebel.com/blog/git-cheat-sheet)

[Git Cheat Sheet 📄](https://dev.to/doabledanny/git-cheat-sheet-50-commands-free-pdf-and-poster-4gcn)
