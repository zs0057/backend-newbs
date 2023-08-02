# Git

---

# [**[Git Tutorial](https://www.w3schools.com/git/)**]

---

## Git and GitHub Introduction

### What is Git?

- 2005년 Linux Torvalds가 만든 버전 관리 시스템
- 코드 변경 추적, 협업 등을 위해 사용된다

### Working with Git

- Git을 사용해 폴더를 repository로 만든다
- Git은 숨겨진 폴더를 만들어서 해당 폴더의 변경 사항을 추적
- changed, added or deleted 파일은 modified 된 것으로 간주
- modified 파일을 선택하여 stage 할 수 있다
- staged 파일은 committed 된 것
- Git으로 모든 commit의 전체 기록을 볼 수 있다
- 이전 commit으로 되돌릴 수 있다
- Git은 모든 commit에서 모든 파일의 복사본을 저장하고 있지 않다. 모든 commit의 변경 사항을 추적 할 뿐이다

## Git Getting Started

### Configre Git

- Git에게 자신의 이름과 메일을 알려줄 수 있다

```html
$ git config --global user.name "AFpine"
$ git config --global user.email "mrt2000@jbnu.ac.kr"
```

- global : 컴퓨터의 모든 repo에 대한 이름을 설정
    - 현재 repo에만 설정하려면 global을 쓰지 않으면 된다

### Creating Git Folder and Initialize Git

- 폴더를 만들고 그 폴더에서 Git을 초기화할 수 있다

```html
$ mkdir project
$ cd /project
$ git init
```

- Git은 변경 사항을 추적하기 위해 숨겨진 폴더를 만든다

## Git New Files

### Git Adding New Files

- repo에 html 파일을 commit 하는 상황을 생각하자
- repo 폴더 안의 파일은 두 가지 상태 중 하나다
    - Tracked : Git이 알고 있고 repo에 추가된 파일
    - Untracked : 디렉토리 안에 있지만, repo에 추가되지 않은 파일
- 처음에 비어있는 repo에 파일을 추가하면 전부 untracked 상태다

## Git Staging Environment

### Git Staging Environment

- 작업하는 동안 파일을 추가, 편집 및 제거할 수 있다
- 작업의 일부를 완료할 때마다 staging environment에 파일을 추가해야 한다
- staged 파일은 repo에 commit 될 준비가 된 파일이다

```html
$ git add index.html
$ git status [--short]
	<!-- 변경 사항에 index.html이 생성되었음을 알 수 있다
		?? - Untracked files
		A - Files added to stage
		M - Modified files
		D - Deleted files
	-->
$ git add --all
	<!-- 모든 파일을 stage 상태로 추가한다 -->
```

## Git Commit

### Git Commit

- staged 파일을 commit 할 수 있다
- commit을 하면 작업의 진행 상황과 변경 사항을 추적할 수 있다
- Git은 commit을 “save point” 처럼 생각한다
    - 그러므로 문제가 생기거나 변경할 점이 있을 때 돌아갈 수 있다
- commit에는 메세지를 항상 포함해야한다
    - 언제 어떤 파일의 변경이 있었는지 확인할 수 있다

```html
$ git commit -m "First commit"
```

### Git Commit without Stage

- staging을 생략하고 commit 할 수도 있다
    - 일반적으로 권장하지 않는다

```html
$ git commit -a -m "update index.html"
```

- log 커맨드로 repo의 commit 기록을 확인할 수 있다

```html
$ git log
```

## Git Branch

### Working with Git Branches

- branch는 repo의 새로운/별도의 버전
- branch를 사용하면 main branch에 영향을 주지 않으면서 프로젝트의 다른 부분을 작업할 수 있다
    - 작업이 완료되면, 해당 branch를 main branch에 merge할 수 있다

### New Git Branch

- branch 명령어를 사용하여 새로운 branch를 만들고, 확인할 수 있다
    - 현재 * 가 붙은 branch에서 작업중
- checkout 명령어를 사용해서 작업할 branch로 옮겨갈 수 있다

```html
$ git branch new_branch
$ git checkout new_branch
```

### Switching Between Branches

- branch를 바꾸면, 실제 폴더안의 파일도 바뀐다

### Emergency Branch

- 현재 다른 branch에서 작업 중에, main branch에서 문제가 생겼다고 가정하자
- 현재 branch의 코드를 수정하거나 main branch를 직접 수정해서 오류를 고치고 싶지 않다
- 그렇다면 다른 emergency branch를 만들고 거기서 오류를 수정해서 main branch와 merge 하면 된다

## Git Branch Merge

### Merge Branches

- 위의 상황을 계속 생각해보자
- emergency branch는 main branch에서 직접 나왔고, 오류 수정중에 main branch에 다른 변경 사항이 없었다
- 그러므로 Git은 emergency branch를 main branch의 연장선 이라고 생각한다
- merge 후, emergency branch는 더 이상 필요가 없으므로 삭제할 수 있다

```html
$ git checkout main
$ git merge emergency_fix_branch
$ git branch -d emergency_fix_branch
```

### Merge Conflict

- merge할 두 branch에서 conflict가 발생할 수 있다
    - 어떤 파일의 버전 간에 conflict가 생길 수 있다
- 파일을 수정해서 같게 만들고 merge를 진행하면 된다

## Git GitHub Getting Started

### Push Local Repository to GitHub

- github에서 repo를 만든다
    - remote repo
- local Git repo의 origin을 remote repo로 설정한다
    - origin : remote repo의 alias
- remote repo에 내 local repo의 파일들을 push 한다

```html
$ git remote add origin http://~
$ git push --set-upstream origin main
```

## Git Pull from GitHub

### Pulling to Keep up-to-date with Changes

- 프로젝트를 진행할 때, 가장 최신 commit 상태에서 작업을 해야 할 것이다
- 그럴 때 pull을 사용한다
    - pull은 fetch와 merge 명령어의 조합이다

### Git Fetch

- fetch는 추적중인 branch/repo의 모든 변경 사항을 가져온다

```html
$ git fetch origin
```

### Git Pull

- 그냥 fetch와 merge를 한번에 하는 것

```html
$ git pull origin
```

## Git Push to GitHub

### Push Changes to GitHub

- 이전에 하던대로 하면 된다

```html
$ git commit -a -m "html change"
$ git push origin
```

# [**[Git Cheat Sheet](https://cs.fyi/guide/git-cheatsheet)]**
---
