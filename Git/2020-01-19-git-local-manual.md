---
layout: post
title: "Git 로컬저장소 관리"
date: 2020-01-19 20:47:00 +0900
categories: [git]
---

## 1. Git 로컬저장소 관리

### (1) Git 계정 설정

- `git` 설정 (user.name & user.email): 최초 1회 설정

- `name`은 `committer`를 확인할 때, 구분하기 위한 닉네임처럼 사용됨, `email`은 github계정확인 용도

  ```shell
  $ git config --global user.name "[닉네임으로 사용할 이름]"
  $ git config --global user.email "[github 계정]"
  ```

  <br>

### (2) Git으로 새 프로젝트 관리 시작: `git init`

- `git bash`에서 `git`으로 관리할 `New Project`폴더로 이동한 이후에 아래의 명령어로 `git`관리를 시작한다.

  ```shell
  $ git init
  ```

  <br>

### (3) Commit을 위한 Staging: `git add` , `git restore`

- 코드의 상태변경 스냅샷을 찍고 싶은 파일들을 `Staging Area`에 올린다.

  ```shell
  $ git add [파일 이름]
  # [파일 이름]대신 .을 사용하면 모든 파일변경사항을 staging area로 올림
  ```
  
- 코드의 상태변경 스냅샷을 찍지 않을 파일들은 제거한다. `Staging Area`에서 파일을 내린다.

  ```shell
  $ git restore --staged [파일 이름]
  # [파일 이름]대신 .을 사용하면 staging area에 존재하는 모든 파일변경사항을 내림
  # --cached는 --staged와 동일한 option이다.
  ```
  
  <br>

### (4) 코드의  변경상태를 확인: `git diff`

- 현재 local directory에서 변경된 파일의 내용을 보여준다.

  ```shell
  $ git diff
  # 현재 branch에서 가장 최근 commit 시점 기준 working directory에서 존재하는 파일들의 변경사항을 보여준다.
# Staging Area에 add하기 전에 어떤 파일 변경사항들이 add되는지 미리 확인할 수 있다.
  
  $ git diff --staged
  # 이미 Staging Area에 올라간 파일 변경사항을 확인할 수 있다.
  
  $ git diff [commit명 / branch명]
  # 특정 commit/branch 시점에서의 File과 현재 working directory의 File 변경사항를 보여준다.
  ```
  
  <br>

### (5) 버전 관리를 위한 스냅샷 저장: `git commit`

- 현재 상태에 대한 스냅샷을 `commit`하여, 버전 관리를 진행한다.

  ```shell
  $ git commit -m "커밋 메시지"
  # 커밋 메세지는 이전 commit시점에서 변경된 사항을 간단히 기록한다.
  # 50자 이하로 적는 것을 권장한다.
  
  $ git commit -a
  # File들의 모든 "modified"된 상태를 Staging하고, 바로 Commit까지 해준다.
  # 다만, 주의할 점은 "New File"은 Staging 및 Commit되지 않는다. 
  ```
  
  <br>

### (6) 그 외 명령어

- 현재 `git`의 상태를 조회 `git status`

  ```shell
  $ git status
  ```

- 프로젝트의 버전 관리 이력을 조회

  ```shell
  $ git log
  $ git log --oneline # commit log 각각의 정보를 한 줄에 출력한다.
  $ git log --graph # commit log들을 그래프 형태로 출력한다.
  $ git log -p # 각 변경사항들을 상세하게 보여준다.
  $ git log --stat --summary #각 변경사항들을 대략적으로 보여준다.
  ```

- 명령어 사용방법을 조회

  ```bash
  $ git help [명령어] # WebPage로 명령어 Manula Page를 띄워준다.
  ```

- `git`의 버전관리 이력을 시각적으로 보여준다.

  ```bash
  $ gitk
  ```

- 현재 디렉토리를 workspace로 하는 `vscode`  실행

  ```shell
  $ code .
  ```

  <br>
