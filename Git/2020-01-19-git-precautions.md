---
layout: post
title: "Git 주의사항"
date: 2020-01-19 21:02:00 +0900
categories: [git]
---

## Git 주의사항

### 1. Git은 디렉토리 단위로 관리된다. 

- 특정 directory에서 `git init` 명령어를 사용하면, 해당 directory에는 `.git` 이라는 숨김 디렉토리가 생성되고, 이는 모든 git 정보가 저장 및 관리된다.

- 따라서, git으로 관리되는 디렉토리의 하위 디렉토리에서 `git init` 명령어를 사용한다면, 그 하위 디렉토리에서 `commit` 명령어를 사용하면, 어떤 `.git` 디렉토리에 변경정보를 저장해야할 지 모르게 된다.

- 이러한 문제를 해결하기 위해서는 하위 디렉토리의 `.git` 디렉토리를 `rm -r` 명령어로 삭제해주면 된다.



### 2. Merge시의 주의사항

- 같은 파일에 대해서 2개의 branch의 내용이 포함관계를 이룬다면, Git이 `Auto Merge`를 할 수 있다.
- 같은 파일에 대해 2개의 branch가 포함관계를 이루지 않고, 서로 다른 내용이 저장되어 있다면, Git은 `Auto Merge`를 할 수 없다. 이 경우, `Conflict`를 `Resolve`해주고, 다시 Commit를 한다.



### 3. Git은 임시파일에 대한 구분을 직접 할 수 없다. - `.gitignore`를 이용

- `Git push` 할 때, 불필요한 파일은 `push` 안하게 하는 방법
- `http://gitignore.io/` 에 들어가서 내가 사용하는 환경에 관련된 기본 설정파일들에 대해 명시를 해주고, 생성버튼을 누른다.
- 불필요한 파일의 종류
  1. vscode가 만들어내는 `.vscode` 
  2. eclipse가 만들어내는`.metadata`
  3. 그 외 git에 올리지 않아도 되는 설정파일
- `requirements.txt` 같은 파일은 `python` 에서 어떤 개발모듈들을 사용했는지를 알려주어야 하므로 `push`해줘야하는 파일이다. 배포환경에 대한 정보를 알려주는 정보이기 때문이다.