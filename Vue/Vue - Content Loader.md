# Vue - Content Loader

### Content Loader 란?

사용자에게 실제 컨텐츠를 로딩하기 전에 어떤 컨텐츠를 보여줄지 예상가능하도록 설정하는 Placeholder입니다.

![image-20210821063858896](./images/create-vue-content-loader-1)

<br>

### 왜 필요한가?

사용자에게 Content Loader가 없다면, 이미지를 다운로드 받는 동안 기존 레이아웃이 깨지는 현상 등이 발생합니다. 또한, 렌더링되는 과정이 그대로 보여지면, 사용자에게 화면전환이 부드럽지 않다는 인식을 줍니다.

<br>

### 제작 방법

#### vue-content-loader

 Vue에서는 React의 Content Loader 컴포넌트 라이브러리를 차용하여 만든 vue-content-loader가 있으며, Typescript도 지원을 해주기 때문에 이것을 이용하겠습니다.

 사용가능한 여러가지 컴포넌트가 있는데 그 중에서 커스터마이징하기 좋은 `ContentLoader`컴포넌트를 사용하겠습니다. 예제코드는 다음과 같습니다.

```javascript
<script>
  import { ContentLoader } from "vue-content-loader"

  export default {
    components: { ContentLoader }
  }
</script>

<template>
  <content-loader
    :width="400"
    :height="460"
    :speed="2"
    primaryColor="#f3f3f3"
    secondaryColor="#ecebeb"
  >
    <circle cx="35" cy="28" r="15" /> 
    <rect x="58" y="18" rx="2" ry="2" width="140" height="10" /> 
    <rect x="58" y="34" rx="2" ry="2" width="140" height="10" /> 
    <rect x="25" y="72" rx="2" ry="2" width="336" height="132" />
  </content-loader>
</template>
```

<br>

vue-content-loader github 사이트: https://github.com/egoist/vue-content-loader

vue-content-loader 실습 사이트: https://skeletonreact.com/

<br>

#### 직사각형 content-loader 배치

ContentLoader를 직사각형 컴포넌트를 이용하여 구성할 것입니다.

이를 위해 실제 레이아웃을 이루는 직사각형 모양의 컨텐츠들의 상대적위치(x, y)와 너비(width), 높이(height)를 설정해야합니다.

유튜브를 예를 들면, 다음과 같습니다.

![image-20210821063858896](./images/create-vue-content-loader-2)

모든 content loader들은 동영상 정보를 담는 파란색 박스 안에 위치해야합니다.

이 때, 파란색 박스를 기준으로 content loader들의 상대위치와 너비, 높이를 계산해야 합니다.

빨간색 직사각형 content-loader는 파란색 박스 내부에 위치하기 때문에 파란색 박스 내 상대적 위치 값인 **x: 30, y:10이며, 너비 width: 120, 높이 height: 90**입니다.

저는 이를 위해 HTMLElement.getBoundingClientRect() 함수를 이용하였습니다.

<br>

#### HTMLElement.getBoundingClientRect() 함수

HTMLElement 을 직사각형으로 생각할 때, 그려지는 절대적인 위치 값과 직사각형의 너비, 높이를 알려주는 함수입니다. 이를 이용하면, 파란색 박스 내에서 빨간색 박스의 상대적인 위치 값을 알 수 있으며, 방법은 다음과 같습니다.

1. 파란색 박스의 절대적인 위치 값 계산
2. 빨간색 박스의 절대적인 위치 값 계산
3. 빨간색 박스의 상대적인 위치 값 = (빨간색 박스의 절대적인 위치 값) - (파란색 박스의 절대적인 위치 값)

<br>

### Content Loader 노출종료시점

실제 우리가 보여줘야할 컨텐츠들이 모두 로딩이 완료될 때, Content Loader 노출을 종료해야합니다.

컨텐츠에는 단순히 텍스트 종류도 있겠지만, **이미지 URL**도 있을 것입니다. 

이미지 URL의 형태의 이미지는 텍스트처럼 비동기로 컨텐츠를 가져온다고해서, 바로 로딩되는 것이 아니라 한번더 로딩을 거쳐야합니다. 이 로딩까지 완료됐을 때, Content Loader 노출을 종료해야 부드러운 화면전환을 이뤄낼 수 있습니다.

이미지 URL이 로딩됐는지 알기 위해서는 Vue의 img태그에서는 `@load` 이벤트를 사용하고, `Swiper` 모듈 내에서는 `@imagesReady` 를 사용하면 됩니다.

<br>

### 참고자료

https://github.com/egoist/vue-content-loader#readme

https://github.com/danilowoz/react-content-loader

https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/viewBox

https://stackoverflow.com/questions/442404/retrieve-the-position-x-y-of-an-html-element-relative-to-the-browser-window

https://renatello.com/vue-js-image-loaded/

https://swiperjs.com/swiper-api#events

