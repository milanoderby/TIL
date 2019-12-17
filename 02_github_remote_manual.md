# Github 원격저장소 관리

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