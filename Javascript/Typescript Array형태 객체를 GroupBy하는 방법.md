# Typescript Array형태 객체를 GroupBy하는 방법

### 필요성

예를 들어, 다음과 같은 화면을 구성해야한다고 가정합시다.
주문내역목록화면에서 **주문내역목록**을 각 **주문날짜**별로 묶어서 노출해야하는 것입니다.

이를 단순히 서버에게 Map으로 내려달라고 부탁할 수도 있지만, 일반적이지 않은 방법입니다.
그렇기 때문에, **주문내역목록(Array\<Order>)** 을 받아서 **주문날짜(Map\<string, Array<Order>>)** 별로 그룹핑하는 방식을 사용하는 것입니다.

이 방식의 장점은 Vue의 computed속성과 함께 사용했을 때 나타납니다. 단순히, Array\<Order> 형태의 주문내역목록 데이터에 주문내역을 추가하기만 해도 **computed** 속성에 명시한 **groupBy** 메소드로 인해서, 자동적으로 **주문날짜** 별로 **주문내역목록**을 노출됩니다.
<br>

### 타입스크립트를 이용한 코드

``` javascript
    computed: {
      orderGroupedByDate(): Map<string, Array<any>> {
        return groupBy(this.orders, "orderCompleteYmd");
      },
    },
```
<br>
``` javascript
groupBy<T, K extends keyof T>(array: T[], key: K) : Map<T[K], T[]> {
    const map = new Map<T[K], T[]>();
    array.forEach(item => {
      const itemKey = item[key];
      if (map.has(itemKey)) {
        map.get(itemKey)!.push(item);
      } else {
        map.set(itemKey, new Array<T>(item));
      }
  });
  return map;
},
```