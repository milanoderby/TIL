---
layout: post
title: "Git Branch와 Merge"
date: 2020-01-19 20:53:00 +0900
categories: [git]
---

## 1. Branch, Merge 관련 명령어

### (1) 기존의 큰 줄기에서 나오는 Branch를 생성하는 명령어: `git branch`

- 큰 프로젝트에서 기존의 기능은 유지하고, 새로운 기능을 추가하기 위해 큰 줄기에서 뻗어나오는 새로운 줄기를 생성한다. 브랜치는 비용이 적게 들고, 쉽다. (많은 도전을 권장)

  ``` shell
  $ git branch [새로 생성할 브랜치 명]
  $ git branch new
  ```

- 현재 프로젝트에서  `Local Directory`에서 존재하는 `branch`정보를 출력한다.

  ```shell
  $ git branch
  ```

- 특정 branch를 삭제한다.

  ```shell
  $ git branch -d [삭제할 브랜치 명]
  # 위 명령어는 삭제할 브랜치의 변화가 이미 현재 브랜치에 적용되어 있을 때에만 삭제가능하다.
$ git branch -D [삭제할 브랜치 명]
  # 위 명령어는 브랜치의 변화가 현재 브랜치에 적용이 되어있든, 안되어있든 강제로 삭제한다.
  ```
  
  

### (2) 특정 branch로 이동한다: `git checkout` , `git switch`

- 원하는 시점이나 원하는 분기로 이동한다.

  ```shell
  $ git switch [이동할 브랜치 명]
  $ git switch -c [이동할 브랜치명] # branch를 새로 생성하고, 해당 branch로 이동한다.
  $ git checkout [이동할 브랜치 명]
  ```

  

### (3) 현재 줄기에서 다른 줄기의 내용을 병합하는 명령어: `git merge`

- 새로운 기능을 만든 branch를 다시 큰 프로젝트로 합칠 때, 사용하는 명령어

  ```shell
  $ git switch [base가 되는 branch] # base가 되는 branch로 이동한다.
  $ git merge [합병할 branch 명] # base가 되는 branch에 합병할branch를 추가한다.
  $ git merge --no-ff [합병할 branch명] # fast forward merge를 하지 않고, merge한다. 주로 merge를 한 흔적을 남기고 싶을 때, 이러한 merge방식을 사용한다.
  ```

  

## 2. Merge Conflict (Merge시, 충돌이 발생할 수도 있다.)

## Merge 시나리오

### 1. Fast Forward Merge

브랜치 분기가 일어났지만, merge 시점에서 branch 한쪽에서만 commit등이 쌓여있는 경우

(ex. new에만 commit이 있고, master에는 없었을 때)

위의 경우에는 master가 new쪽으로 위치만 변경한 모습으로 merge가 됨.

merge한 것이 티가 나지 않으므로 다른 개발자에게 merge한 흔적을 남기고 싶다면, no-ff 옵션을 사용한다.



### 2. Auto-merge

merge시점에 양쪽 브랜치에 commit들이 쌓여있지만, conflict가 발생하지 않는 경우

git이 자동으로 merge를 진행할 수 있다.



### 3. Merge Conflict 발생

merge시점에 양쪽 브랜치에 commit들이 쌓여있고, conflict가 발생하는 경우

- 동일 파일 내에 서로 다른(상충하는) 내용이 있을 경우

- Merge Conflict가 발생하면

  ```
  >>>>>>>>>>>>>>>>>>>>>
  Master branch domain
  =====================
  Child branch domain
  <<<<<<<<<<<<<<<<<<<<<
  ```

  위와 같이 구역 내부에서 Master branch 내용과 Child branch 내용을 표시해주고, 어떤 것을 사용할지 사용자가 정하게 한다.

- 위와 같은 Merge Conflict를 해결하기 위해서는 내가 사용할 내용만 남기고, >>> === <<<와 같은 특수문자들은 제거하여 git에게 내가 이러한 Conflict를 인지하고, Resolve했다는 것을 알려준다.

- 그 후, add 및 commit를 하면 파일에 대한 Conflict가 해결이 되었기 때문에 commit이 성공적으로 잘 된다.

  ```shell
  현재 상태: (master | Merging)
  $ vi [Conflict가 발생한 문서] #(Conflict가 발생한 문서를 편집하여 Conflict를 해결한다.)
  $ git add [Conflict 해결한 문서]
  $ git commit -m "Resolve Conflict"
  해결한 상태: (master) # Merging이 사라짐
  ```

  