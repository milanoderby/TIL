## Vue - router-link 사용 시, a태그 대신 다른 태그로 변환

`router-link` 태그는 기본적으로 `a` 태그로 변환을 해줍니다.
아래의 코드는

``` javascript
<router-link class="btn btn_default" to="/products">
  전체 상품 보기
</router-link>
```

다음 코드로 변환됩니다.

``` javascript
<a href="/products" class="btn btn_default"> 전체 상품 보기 </a>
```

<br>

## 목적: button 태그로 렌더링

`button` 태그로 변환하는 것을 원한다면, 다른 방법을 사용해야 합니다.

stackoverflow 를참고하면, `tag` 속성을 추가하면 된다고 합니다.
(참고: [https://stackoverflow.com/questions/45638239/enclosing-a-router-link-tag-in-a-button-in-vuejs](https://stackoverflow.com/questions/45638239/enclosing-a-router-link-tag-in-a-button-in-vuejs)

``` javascript
<a href="/products" class="btn btn_default" to="/products"> 전체 상품 보기 </a>
```

하지만, 실제 적용했을 때, 위와 같이 `a` 태그에 `to` 속성만 추가될 뿐 변화가 없었습니다.

<br>

## 문제의 원인

자세히 보니, Vue-Router 4.x 버전부터는 `tag` 속성을 이용한 방법이 fadeout 되고, `v-slot`과 `custom` 태그를 이용하는 방법으로 개발자가 customizing 하는 것을 권장하도록 변경되었습니다.
(참고: [https://next.router.vuejs.org/api/#router-link-s-v-slot](https://next.router.vuejs.org/api/#router-link-s-v-slot)

<br>

## 문제해결

하지만, 위의 방법은 새로 `Custom Tag` 즉, Vue 컴포넌트를 하나 더 추가해야되므로 보다 더 간단한 다른 방법을 사용하여 해결하였습니다.

엘리멘트에 클릭 이벤트에 routing 하는 메소드를 걸어줍니다.

``` javascript
<button class="btn btn_default" @click="moveProductsPage">
    전체 상품 보기
</button>
```

아래와 같이 Vue 컴포넌트의 메소드를 추가합니다.

``` javascript
methods: {
    moveProductsPage(): void {
        this.$router.push("/products");
    }
}
```