# TS2532: Object is possibly 'undefined'.

위의 오류를 해결하기 위한 방법

<br>

## Optional Chaining

So what is optional chaining? Well at its core, optional chaining lets us write code where we can immediately stop running some expressions if we run into a `null` or `undefined`. The star of the show in optional chaining is the new `?.` operator for *optional property accesses*. When we write code like

```
let x = foo?.bar.baz();
```

<br>

즉, 어떤 변수가 undefined 상태라면, 위의 연산은 끝까지 진행되지 않고, 멈출 것입니다.

https://devblogs.microsoft.com/typescript/announcing-typescript-3-7-beta/

https://github.com/tc39/proposal-optional-chaining/

https://github.com/tc39/proposal-nullish-coalescing