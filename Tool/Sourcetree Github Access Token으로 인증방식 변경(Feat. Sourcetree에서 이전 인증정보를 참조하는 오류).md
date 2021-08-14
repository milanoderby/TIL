# Sourcetree Github Access Token으로 인증방식 변경(Feat. Sourcetree에서 이전 인증정보를 참조하는 오류)

### 문제의 발생

갑작스럽게 Github Push가 안되었다. ~~(물론, 예고도 없던 것은 아니었지만)~~

어쨌든, 에러로그를 요약하자면, 2021년 8월 13일 부로 Github 측에서 패스워드 인증방식에 대한 지원을 중단한다는 내용입니다.

![image-20210814041221965](./images/sourcetree-push-error-1)

<br>

### 인증방식을 PersonalAccessToken 방식으로 변경

1. Github에서 권장하는 PersonalAccessToken을 생성합니다. 아래 Github Docs를 참고하여 따라합니다.

   이 때, 생성된 PersonalAccessToken 은 최초생성시점에만 복사할 수 있으며, 추후에는 다시 볼 수 없는 값입니다. 또한, 추후에도 쓰여야하는 값이므로 비밀번호처럼 소중히 본인만 알 수 있도록 복사하여 보관하여야 합니다.

   https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token

2. Sourcetree 의 Github 계정연동 방식을 PersonalAccessToken 방식으로 변경합니다. 이를 위해 Sourcetree창 상단의 `도구` - `옵션` 을 클릭합니다.

   ![image-20210814042001838](./images/sourcetree-push-error-2)

3. `옵션` 팝업창 - `인증` - `연동된 github` 계정 클릭 - `편집` 클릭

   ![image-20210814042221957](./images/sourcetree-push-error-3)

4. `호스팅 계정 편집` 팝업창 - 인증방식 `Basic` 으로 변경 - `비밀번호 새로고침` 클릭

   ![image-20210814042356692](./images/sourcetree-push-error-4)

5. `1번` 과정에서 생성한 토큰 값을 복사하여 `비밀번호 입력` 팝업창에 `암호`란에 입력합니다.

   ![image-20210814042653378](./images/sourcetree-push-error-5)

<br>

### Github에서 이전 인증정보를 캐싱하는 오류

이와 같이 하면, Push 오류가 발생하지 않을 줄 알았으나, 계속해서 같은 오류가 발생하였습니다.

처음에는, Sourcetree 컨플루엔스에서 제공하는 [암호변경 가이드](https://confluence.curvc.com/pages/viewpage.action?pageId=72290170)대로 시행하면, 이 문제도 해결될 줄 알았으나, 해결되지 않았습니다.

 `Windows` 환경의 Sourcetree에서는 인증정보를 변경하여도 패스워드를 캐싱하여 발생하는 오류가 있습니다. Sourcetree 자체에서 비밀번호를 저장하는 `passwd` 파일이 있습니다. 이것을 삭제해주면 됩니다.

![image-20210815003201118](./images/sourcetree-push-error-6)

삭제하고, 다시 Sourcetree에서 push를 시도하면, 새로운 비밀번호를 물어봅니다. 이 때, Personal Access Token을 입력하면, 문제가 해결됩니다.

<br>

위의 작업을 수행해도 인증비밀번호가 변하지 않는 경우가 있습니다. 어쩔 수 없이 stackoverflow를 찾아가서, 이것저것 해봤습니다.

결론적으로 알아낸 방법은 저장되어 있는 `credential` 을 `reject`하는 것입니다.

문제가 되는 git local 레포지토리의 터미널을 들어가서 아래 명령어를 수행해줬습니다.

```bash
$ git credential reject
protocol=https
host=github.com
```

뒤이어, Sourcetree에서 push를 시도하면, 새로운 비밀번호를 물어봅니다. 이 때, Personal Access Token을 입력하면, 문제가 해결됩니다.

<br>

### 결론

원인을 찾으려고 돌고 돌아왔는데, 결론적으로 stackoverflow 보고서, 하나씩 다 시도해본 방법이 가장 빨랐습니다.

<br>

### 참고자료

https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token

https://whitekeyboard.tistory.com/332

https://stackoverflow.com/questions/20195304/how-do-i-update-the-password-for-git