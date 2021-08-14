# Git Pull/Push 시, 다른 Repository 를 함께 Fetch/Push 할 때

언제 설정이 꼬였는지 모르겠지만, 제목과 같은 문제가 발생하여 문제를 해결까지의 과정을 남깁니다.

이는 다른 원격저장소를 바라보고 있다는 뜻입니다.

1. 현재 로컬 Repository의 원격저장소를 확인합니다.

   ```bash
   $ git remote -v
   ```

   <br>

   ![image-20210814132858191](./images/git-other-remote-repository-1)

   저는 위와 같이 `Programmars` 레포지토리인데, `Backjoon-Online-Judge` 레포지토리를 바라보고 있는 이상현상이 발생하였습니다.

   중요한 건, 해당 레포지토리 뿐만 아니라, 다른 레포지토리에서도 동일한 현상이 발생하고 있었습니다.

2. 전역설정 확인

   ```bash
   $ git config --global --list
   ```

   <br>

   ![image-20210814133122844](./images/git-other-remote-repository-2)

   위와 같이 `remote.origin.url` 이라는 설정 값에 `Backjoon-Online-Judge` 원격 레포지토리 주소가 설정되어 있었습니다. 이 설정이 문제이기 때문에 제거합시다.

3. 문제가 되는 전역설정 제거

   ```bash
   $ git config --global --unset remote.origin.url
   ```

   <br>

   전역설정 관련 명령어는 아래 사진과 같습니다.

   ![image-20210814133354637](./images/git-other-remote-repository-3)

4. 정상동작 확인

   ```bash
   $ git remote -v
   ```

   <br>

   다시, 위의 명령어를 사용하여 정상적인 Remote Repository를 바라보고 있는지 확인합니다. 결과는 아래와 같습니다.

   ![image-20210814133521761](./images/git-other-remote-repository-4)

<br>

### 참고자료

https://git-scm.com/book/ko/v2/Git%EC%9D%98-%EA%B8%B0%EC%B4%88-%EB%A6%AC%EB%AA%A8%ED%8A%B8-%EC%A0%80%EC%9E%A5%EC%86%8C
