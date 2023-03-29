# Vue - Typescript + Vuex 모듈화 적용방법

기존 Vuex 문서에 Typescript 지원에 대한 가이드([https://next.vuex.vuejs.org/guide/typescript-support.html#typing-store-property-in-vue-component](https://next.vuex.vuejs.org/guide/typescript-support.html#typing-store-property-in-vue-component))가 있으나, 내용이 부실하여 실제 개발에 적용하기 어려운 점이 있어 기록으로 남깁니다.

<br>

### store/modules/ProductStore.ts 파일

``` typescript
import Product from "@/domain/product/Product";
import {ActionContext, ActionTree, GetterTree, Module, MutationTree} from "vuex";
import api from "@/api/backend-api";

export class ProductState {
  public product?: Product;
}

const mutations: MutationTree<ProductState> = {
  setProduct(state: ProductState, product: Product): void {
    state.product = product;
  },
};

const getters: GetterTree<ProductState, any> = {

};

const actions: ActionTree<ProductState, any> = {
  async fetchProduct(context: ActionContext<ProductState, any>, productId: number) {
    await api.productById(Number(productId)).then(response => {
      context.commit('setProduct', new Product(response.data.result));
    });
  }
};

export const ProductStore: Module<ProductState, any> = {
  namespaced: true,
  state: new ProductState(),
  getters,
  mutations,
  actions
};
```

<br>

#### 어려운 점

* Typescript + ESLint 환경에서 계속 TypeError가 나기 때문에 Vuex 모듈화를 할 때, 사용되는 객체들의 적절한 타입을 찾는 것이 어려웠습니다.

#### 문제해결

* 상태관리대상: 상태관리할 객체입니다. 여기선, ProductState 클래스의 객체입니다.
* GetterTree<상태관리대상, any> : Vuex getters 객체타입
* MutationTree<상태관리대상, any> : Vuex mutataions 객체타입
* ActionTree<상태관리대상, any> : Vuex actions 객체타입
* Module<상태관리대상, any> : Vuex의 모듈타입입니다.

<br>

### store/index.ts 파일

``` typescript
import { createStore } from 'vuex'
import {ProductStore} from '@/store/modules/ProductStore'
import {ReceiverStore} from "@/store/modules/ReceiverStore";

export default createStore({
    modules: {
        ProductStore,
        ReceiverStore
    }
});
```

위에서 선언한 모듈들을 포함하여 저장소로 생성합니다.

<br>

### Vue Component 에서 정의 및 호출

``` typescript
export default defineComponent({
    name: 'ProductDetailView',
    props: ['productId'],
    components: {
      ProductInfoComponent,
      ProductUsageComponent,
      ProductNoticeComponent,
      ProductDetailImageComponent,
      OrderButtonComponent,
      TopButtonComponent
    },
    computed: {
      ...mapState({
        product: (state: any) => state.ProductStore.product,
      }),
      ...mapGetters('ProductStore', [

      ])
    },
    methods: {
      ...mapMutations('ProductStore', [
        'setProduct'
      ]),
      ...mapActions('ProductStore', [
          'fetchProduct'
      ])
    }
  });
```

`...mapXXX` 방식으로 상태관리 객체의 state, getter, mutation, action 을 위와 같이 정의할 수 있습니다.

실제 호출 시에는, `computed`와 `methods`내에 속한 다른 변수와 메소드를 호출할 때처럼 변수명, 메소드명만 호출하여 사용하면 됩니다.