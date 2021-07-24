---
layout: post
title: "Git 협업 방식"
date: 2020-01-19 21:02:00 +0900
categories: [git]
---

## 실제 협업은 어떻게 하나?

### 1. branch 활용 (git flow)

- 우선 오픈소스의 파일을  local directory로 가져온다. (`git clone`)

  ``` shell
  $ git clone [협업해야될 원격저장소 주소]
  # github에 저장된 repository명으로 directory를 생성하고, clone해온다.
  $ git clone [협업해야될 원격저장소 주소] [directory명]
  # 지정해준 directory명으로 새로 directory를 생성하고, clone해온다.
  ```

- 새로운 branch를 생성한다.

  ```shell
  $ git branch [생성할 branch 명]
  ```

- 수정하고서, 수정한 branch를 협업 원격저장소에 push한다.

  ```shell
  $ git push [협업 원격저장소 주소] [내가 수정한 branch 명]
  ```

- 이제 github로 이동해서, 내가 수정한 branch를 master에 merge를 요청한다.

  ```
   github의 원격저장소에서 'Pull requests'를 클릭한다.
   'compare and pull request'를 클릭한다.
  ```

- github에서 merge를 할 수 있는지를 판단할 것이고, 이를 알려준다.

- 제목과 내용을 추가한다.

  ```
  # 변경사항
  	1. 프로젝트 명 추가
  	2. 프로젝트 기능명세 추가
  ```

- `Pull request`하고 나면, `Pull request` 메뉴를 통해 코드 한줄 한줄에 대해 리뷰를 해줄 수 있다. 

  ```
  최종적으로
  	1. Comment # 코드에 대해 조금 부족한 점이 있으면, Comment를 남긴다.
  	2. Approve # 승인하면, Merge를 통해서 base branch에 반영한다.
  ```

  

### 2. fork - PR (github flow)

- github에서 내가 참여하고 싶은 오픈소스의 원격저장소의 `fork`를 누른다.
- 동일한 이름의 원격저장소가 나의 github로 복사가 될 것이다.
- 나의 github의 원격저장소를 local directory로 `clone`해온다.
- 나의 local directory에서 수정을 한 후에 github로 `push`한다.
- 수정사항이 올라가면, github의 Code란에서 `pull request` 또는 `compare`라는 메뉴가 제공된다.
- 여기서, pull request를 진행하면, 1 번에서 진행했던 것처럼 원래 오픈소스에 merge를 요청할 수 있게 된다.

