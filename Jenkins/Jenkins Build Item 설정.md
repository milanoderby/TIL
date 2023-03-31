# Jenkins Build Item 설정

새로운 Item 클릭 - FreeStyle 설정

<br>

## General 설정

### GitHub project 체크

- Project url: 연관된 github 프로젝트를 설정할 수 있다. 나중에 Jenkins Job 설정에서 해당 github 사이트로 참조할 수 있다.

<br>

## Job Notifications 설정

### Notification Endpoints 설정 - 각종 상황에서 Jenkins Build 상황에 대한 것을 전달함

해당 설정은 Jenkins에서 실패할 때, Dooray Hook으로 메세지를 전달하는 목적

- Format: JSON
- Protocol: HTTP
- URL Source: Plain Text - Dooray Hook URL 주소 입력
- Timeout: 30000 (30초가 지나면, 알림이 실패했다고 판단)
- Retries: 0 (알림을 재시도할 횟수)
- Log: 0 (전송할 로그들의 라인 수)
- Notes: 추가적으로 메세지에 입력할 세부사항

<br>

### 오래된 빌드 삭제

- Strategy: Log Rotation (Log Rotation을 이용하여 순차적으로 삭제된다.)
  - 빌드 이력 유지 기간(일): 15일
  - 보관할 최대갯수: 50개

<br>

### 이 빌드는 매개변수가 있습니다: 빌드 시, 전달할 매개변수를 입력 

- String parameter
  - 매개변수명: branch 추가
  - Default Value: alpha (매개변수를 못받았을 경우, alpha로 설정)
  - 설명: 매개변수명 설정
  - Trim the string: 매개변수명의 앞뒤 공백을 제거한다.

<br>

## 소스코드 관리

### Git 선택: 실제 실행할 Git Repository를 선택한다.

- Repositories
  - Repository URL: Github Repository 
  - Credentials: 기존에 Jenkins 설정을 통해 Git 보안 키를 등록해야 한다.

- Branches to build
  - Branch Specifier: ${branch} (위에서 설정한 입력 매개변수를 여기서 사용해준다.)
- Repository browser: Jenkins view의 변화를 검색할 수 있는 외부 시스템 링크를 추가한다. (`자동` 으로 설정)
- Additional Behaviours: Build와 관련된 추가적인 작업을 설정한다.
  - Prune stale remote-tracking branches: 추적된 원격브랜치들 중에서 오래된 것을 삭제한다.

<br>

## 빌드 유발

빌드를 원격으로 유발

Build On Time: Cron 문법으로 원하는 시간대에 작동하도록 스케줄링하는 설정

Build periodically: Build On Time과의 정확한 차이를 모르겠다.

그 외 다양한 설정이 있으나 설정해주지 않는다.

<br>

## 빌드 환경

필요 시, SonarCube 와 연동하기 위해 Prepare SonarQube Scanner environment를 설정할 수 있다.

그 외 다양한 설정이 있으나 설정해주지 않는다.

<br>

## Build: 프로젝트에 맞게 어떤 빌드 도구를 이용할지 설정

### Invoke Gradle script

Invoke Gradle

- Gradle Version: Gradle 6.5.1

Use Gradle Wrapper

- Tasks `:front:clean :front:build -x test --warning-mode=summary` : 해달 프로젝트를 빌드하기 위해 적절한 Gradle 명령어를 입력한다.

<br>

### TODO: Jenkins 추가로 정리할 것!!
