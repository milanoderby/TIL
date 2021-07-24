## Javascript Module Pattern - 기본 개념

### 익명 클로저(`Anonymous Closure`)

- 자바스크립트의 특징 중 하나로, Module Pattern이 가능하게 하는 기반구조

- 익명 함수를 생성하면, 익명 함수 안에 존재하는 코드들은 클로저라는 영역에 존재하게 된다.

- 클로저 영역에서는 내부 변수들의 `application` 의 생명주기 동안 `privacy` 와 `state` 를 보장한다.

  ```javascript
  (function () {
  	// ... all vars and functions are in this scope only
  	// still maintains access to all globals
  }());
  ```

  - 위의 함수에서 `function` 이라는 키워드로 시작하면, 함수선언식(`function declaration`)으로 취급하지만, 뒤에 `()` 를 붙임으로써 함수표현식(`function expression`) 으로 취급하게 한다.

<br>

### 전역 변수 가져오기 (`Global Import`)

- 자바스크립트에는 암묵적 전역(`implied globals`) 이라는 특징이 존재한다.

- 암묵적 전역(`implied globals`)이란? 어떤 변수가 쓰였을 때, 인터프리터는 `scope chain`을 따라서 점차 넓은 `scope`에서 변수가 선언된 위치를 탐색한다. 만약 변수 선언부를 찾지 못했을 때, 그 변수는 전역변수로 간주된다는 특성이다. 즉, 할당만 되어있고, 선언부는 없는 변수는 전역변수로 생성된다.

- 이 특성을 이용하여, 전역변수를 가져오는 것은 상당히 관리하기 힘든 코드로 만들 수도 있다. 왜냐하면, 주어진 파일 내에서 어떤 변수가 전역변수인지 불분명하기 때문이다.

- 다행히도, 암묵적 전역(`implied globals`)을 이용하는 것 보다 빠르고, 명확하게 전역변수를 가져오는 방법이 존재한다. 단순히, 전역변수를 익명함수의 인자로 넘겨줌으로써, 전역변수를 우리가 원하는 Module 안으로 전달할 수 있다.

  ```javascript
  (function ($, YAHOO) {
  	// now have access to globals jQuery (as $) and YAHOO in this code
  }(jQuery, YAHOO));
  ```

  - 위와 같은 방식으로, 외부모듈인 jQuery 모듈과 YAHOO 모듈을 나의 모듈 안에 주입할 수 있다.

<br>

### 모듈 내보내기(`Module Export`)

- 우리가 만든 전역변수들을 단순히 사용하는 것이 아니라 선언하는 것을 원할 때, 익명 함수의 `return` 명령어를 이용하여 쉽게 내보낼 수 있다.

  ```javascript
  var MODULE = (function () {
  	var my = {},
  		privateVariable = 1;
  
  	function privateMethod() {
  		// ...
  	}
  
  	my.moduleProperty = 1;
  	my.moduleMethod = function () {
  		// ...
  	};
  
  	return my;
  }());
  ```

  - 위의 코드의 결과로, `MODULE` 이라는 전역 모듈을 선언했고, 이 모듈을 이용하여 `MODULE.moduleProperty` 와 `MODULE.moduleMethod` 를 `public property` 로 이용할 수 있다. 
  - `MODULE` 은 익명 함수의 클로저를 이용하여 `private internal state` 를 유지한다. privateVariable 변수에 직접 접근하는 것을 막아준다. 오로지, `return value` 로 받은 변수들을 이용하여 접근할 수 밖에 없도록 한다.

<br>

### 결론

- 장점
  - 캡슐화: 불필요한 변수로의 접근을 방지해주고, 필요한 변수와 함수만을 반환해준다.
  - 모듈화: 하나의 javascript 파일에 특정 기능에 필요한 변수와 함수를 구현함으로써 기능적인 분리가 가능하다.
- 단점
  - 특정 변수의 가시성 수정자를 바꿨을 때, 변수가 쓰여진 곳의 코드를 모두 바꿔야 된다.
  - 나중에 추가된 private 변수들에 접근할 수 없다.
  - 자동화된 단위테스트를 만들 수가 없다.
  - bug의 hot fix가 필요할 때, 복잡성이 추가된다. private 변수의 상태를 확인하기 위해서 public method를 return 해줘야한다. 그렇기 때문에, private 변수를 추가할 때, 해당 변수는 유연하게 바꿀 수 없다는 것을 기억할 필요가 있다.

<br>

참조문서

- [JavaScript Module Pattern: In-Depth](http://www.adequatelygood.com/JavaScript-Module-Pattern-In-Depth.html)
- [Learning JavaScript Design Patterns](https://addyosmani.com/resources/essentialjsdesignpatterns/book/index.html#modulepatternjavascript)

