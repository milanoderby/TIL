# Github 원격저장소 관리

## 1. push

- 현재까지의 파일 상황을 commit 후에 원격저장소에 저장하고 싶다면

  ```shell
  $ git push [원격저장소 명] [branch 명]
  $ git push origin master
  ```

  

## 2. clone

- 원격저장소에 저장되어 있는 모든 파일들을 내 PC 디렉토리에 가져오고 싶을 때

  ```shell
  $ git clone [원격저장소 주소]
  $ git clone https://github.com/milanoderby/test_project.git
  ```



## 3. pull

- 협업할 때, 현재 업데이트 되어 있는 원격저장소의 변경 정보를 내 PC 디렉토리에 업데이트하고 싶을 때

  ```shell
  $ git pull [원격저장소 명] [branch 명]
  $ git pull origin master
  ```



## 4. remote

- 현재 디렉토리에서 여러 개의 원격저장소를 가질 수 있다. 

- 이 때, 각각의 원격저장소의 정보를 출력하고 싶을 때

  ```shell
  $ git remote # 하나의 원격저장소 정보만 출력
  $ git remote -v # verbose mode: (모든 원격저장소 정보를 출력)
  ```

  

## 5. rename

- 원격저장소의 이름을 바꾸고 싶을 때

  ```shell
  $ git rename [현재 원격저장소 명] [바꾸고 싶은 이름]
  $ git rename origin real
  ```



## 6. remote add

- 원격저장소 정보를 추가하고 싶을 때

  ```shell
  $ git remote add [생성할 원격저장소 명] [원격저장소 주소]
  $ git remote add origin https://github.com/milanoderby/test_project.git
  ```

  