# Vue - v-show 와 style: display=none 차이

#### 결론부터 말하자면, Vue 내부에서 `v-show` 는 `html의 style의 display 속성`을 이용하는 것이 아닙니다.

그렇기 때문에, `v-show=false` 와 `style="display:none"`는 서로 다른 것입니다.
`v-show=true` 값으로 바꾸어준다고 해서, `style="display:none"`이 `style="display:block"` 으로 변경되지 않습니다.

`v-show` 를 이용하여 특정 엘리먼트가 보여줄지 말지를 결정할 때는, `style="display:block"`와 같이 사용하여 엘리멘트 `display` 속성 값은 항상 보여지는 것으로 설정해야합니다.