# Vue FE 개발 시, 고려사항

## 초기설정

Vue CLI

- vue.config.js

<br>

## 기능

백엔드 서버와 API 통신

- axios

라우터

- Vue Router

상태관리

- Vuex

<br>

## 성능

UI / UX

- 사용자가 누르는 `버튼 영역의 크기` 가 적절하고, 쉽게 클릭할 수 있는지 확인한다.

애니메이션

- Vue Transition
  - 화면 이동 시, 애니메이션 효과 (슬라이딩)
- Greensock
  - 애니메이션 2개를 연속적으로 사용하고 싶을 때

성능분석 툴

- Chrome LightHouse

<br>

## 배포

배포방법

- Local 환경에서 Build한 결과물만 웹서버 배포경로에 배포한다.
- Github에 올라가있는 소스코드를 Server에서 Build 후, 웹서버 배포경로에 배포한다.

무중단배포

- Spring Boot 서버와 동시배포가 필요

<br>

### 그 외 주의사항

성격이 급한 사람들이 있기 때문에 ajax 서버 통신 이후, 응답 화면을 보여줘야할 때에는 HTTP 요청이 2번 이상 전송되는 것을 막아야합니다.
