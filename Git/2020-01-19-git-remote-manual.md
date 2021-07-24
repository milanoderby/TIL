---
layout: post
title: "Git 원격저장소 관리"
date: 2020-01-19 20:50:00 +0900
categories: [git]
---

# Git 원격저장소 관리

### (1) 원격 저장소 정보 추가: `git remote`

- Github 원격(remote) 저장소(repository)를 생성하고 폴더와 연결한다.

- 원격저장소 정보를 추가하고 싶을 때 사용한다.

  ```shell
  $ git remote add [생성할 원격저장소 명] [원격저장소 주소]
  $ git remote add origin https://github.com/milanoderby/test_project.git
  ```


- 현재 디렉토리에서 여러 개의 원격저장소를 가질 수 있다. 

- 이 때, 각각의 원격저장소의 정보를 출력하고 싶을 때

  ```shell
  $ git remote # 하나의 원격저장소 정보만 출력
  $ git remote -v # verbose mode: (모든 원격저장소 정보를 출력)
  ```

- 원격저장소의 이름을 바꾸고 싶을 때

  ```shell
  $ git remote rename [현재 원격저장소 명] [바꾸고 싶은 이름]
  $ git remote rename origin real
  ```



### (2) 원격 저장소에 local directory의 변경정보를 저장: `git push`

- Github 원격저장소에 현재까지의 변경사항 및 파일을 저장한다.

  ```shell
  $ git push [내보낼 원격저장소 명] [local저장소에서 내보낼 branch명]
  ```



### (3) 원격저장소의 파일들을 local directory로 복사: `git clone`

- Github 원격저장소의 파일들을 local directory로 가져온다.

  ```shell
  $ git clone [협업해야될 원격저장소 주소]
  # github에 저장된 repository명으로 directory를 생성하고, clone해온다.
  $ git clone [협업해야될 원격저장소 주소] [directory명]
  # 지정해준 directory명으로 새로 directory를 생성하고, clone해온다.
  ```



### (4) 원격저장소의 변경된 파일을 local directory로 복사: `git pull`

- 원격저장소의 파일 중 협업이나, 다른 PC에서 작업한 내용을 원격저장소에 저장 등의 이유로 변경된 local directory와 원격저장소간의 불일치를 맞춰주는 역할을 한다.

- `pull = fetch + merge` 이다.

  ```shell
  $ git pull [가져올 원격저장소 명] [원격저장소에서 가져올 branch명]
  # 지정한 원격저장소의 브랜치를 local directory의 현재 branch에 pull한다.
```
  
  