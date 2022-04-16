# URL 정규표현식

### URL 의 형태 및 규칙

URL의 기본 형태는 URI를 따르기 때문에 URI의 형태를 먼저 봅니다.

```
scheme:[//[username[:password]@]host[:port]][/path][?query][#fragment]
```

`[]`문자 안의 문자열은 생략가능하다는 것을 의미합니다.

#### URL 형태 다이어그램

![URI syntax diagram](./images/url-regular-expression)

<br>

#### scheme

scheme 의 뒤에는 `:` 문자가 따라옵니다.

scheme 은 일반적으로 문자(letter, number를 제외한 alphabet으로만 해석하겠습니다.) 로 시작되며,  `+` , `.` , `-` 문자도 중간에 들어올 수 있습니다.

scheme 은 일반적으로 대/소문자를 가리지는 않지만, 일반적으로 소문자를 사용합니다.

scheme 의 예시로는 `http`, `https`, `ftp`, `mailto`, `file`, `data` ,`irc` 등이 있습니다.

<br>

#### authority

scheme 의 바로 뒤에오는 구성요소는 authority 라고 지칭하며, 생략될 수 있는 요소입니다.

authority 는 `//` 문자로 시작합니다.

authority 는 아래와 같이 구성되어 있습니다.

1. userinfo (username 과 password를 포함하는 개념)
2. host
3. port

<br>

#### userinfo

userinfo 는 authority 내에서 생략가능한 요소입니다.

userinfo 뒤에는 `@` 문자가 따라옵니다.

userinfo는 username과 생략가능한 password를 포함하는 개념입니다.

password 앞에는 `:` 문자가 옵니다.

다만, `username:password` 형태를 사용하는 것은 보안적인 이유로 deprecated 되었습니다.

<br>

#### host

hostname, IP 주소를 값으로 가집니다.

일반적인 alphanumeric 문자 외에 `.` 문자를 포함할 수 있습니다.

<br>

#### port

port는 authority 내에서 생략가능한 요소입니다.

port 앞에는 `:` 문자가 옵니다.

<br>

#### path

path 는 `/` 로 구분되는 연속된 path 조각으로 구성되어있습니다.

path 자체는 항상 정의되는 값이지만, 값이 비어있을 수 있습니다. (Empty String)

path 조각 역시, 비어있을 수 있습니다. 그로 인해, `//`와 같은 형태도 나타날 수 있습니다.

authority 가 있다면, path 는 비어있거나, `/` 문자로 시작해야됩니다.

authority 가 없다면, path 조각이 비어있으면 안됩니다. `//` 문자가 authority의 요소로 해석될 수 있기 때문입니다.

#### query

query 는 생략가능한 요소입니다.

query 앞에는 `?` 문자가 옵니다.

문법이 명확하게 정의된 것은 아니지만, 일반적으로 여러 개의 key-value 형태가 구분자 (`&` 또는 `;` 를 사용) 로 구분되어 나타납니다.

<br>

#### fragment

fragment 는 생략가능한 요소입니다.

fragment 앞에는 `#` 문자가 옵니다.

부가적으로 자원의 위치를 알려줍니다. 

일반적으로 HTML 문서의 id 속성을 사용하여 특정 요소를 가리킵니다.

<br>

### URL 에서 사용가능한 문자 규칙

1. ASCII 문자열 집합에서 사용되는 일부 문자, 숫자, 특수문자(`-`, `.`, `_`, `~`)만 허용합니다.
2. ASCII 문자열 집합에서 사용되지 않는 문자들과 안전하지 않은 문자들(`space`, `\`, `<`, `>`, `{`, `}`)은 허용되지 않습니다.
3. 그 외 URL의 구성요소별로 예약어(`/`, `?`, `#`)가 사용됩니다.

<br>

### URL 정규표현식 생성

위의 정보들을 종합하여 아래 Pattern의 정규표현식을 생성할 수 있습니다.

```
(^[a-zA-Z]+):(//(\\w[\\w.~-]*\\w)(:[0-9]*)?)?([\\w/~-]*)?([\\w?&=;~-]*)?
```

부분부분 잘라서 보겠습니다.

`(^[a-zA-Z]+)`

- 매칭되는 문자열은 URL의 scheme을 의미합니다.
- `()` 는 그룹핑을 위한 것입니다.
- 가장 첫 부분에 alphabet 으로 구성된 1개 이상의 문자열과 매칭됩니다.
- 사실, 위의 스펙에서는  `+` , `.` , `-` 문자도 중간에 들어올 수 있다고 언급되어있지만, 대부분의 scheme은 그렇지 않기 때문에 이 정규식에는 반영하지 않았습니다.

<br>

`:`

- 매칭되는 문자는 scheme 의 뒤에 딸려오는 문자를 의미합니다.

<br>

`(//(\\w[\\w.~-]*\\w)(:[0-9]*)?)?([\\w/~-]*)?`

- 매칭되는 문자열은 URL의 authority을 의미합니다.
- `()` 는 그룹핑을 위한 것입니다.
- `()` 뒤에 나오는 `?` 는 수량자 `{0,1}` 과 동일한 의미로 존재할 수도 있고, 없을 수도 있다는 optional 스펙을 반영한 것입니다.
- authority에는 userinfo 요소도 포함되어야 하지만, derpecated 된 요소이기 때문에 이 정규식에는 반영하지 않았습니다.
- 가장 바깥에 있는 `()`를 걷어낸 내용을 아래에서 살펴보겠습니다.

<br>

`//`

- 매칭되는 문자는 authority 의 앞에 오는 문자를 의미합니다.

<br>

`(\\w[\\w.~-]*\\w)`

- 매칭되는 문자열은 URL의 host를 의미합니다.
- `()` 는 그룹핑을 위한 것입니다.
- 특수문자를 제외한 alphanumeric 문자로 시작하고, 끝나야하는 스펙을 반영했습니다.
- 중간에는 특수문자 `.` , `~`, `-` , `_`이 들어갈 수 있도록 허용했습니다. (`\\w` 는 alphanumeric + `_`를 포함)

<br>

`(:[0-9]*)?`

- 매칭되는 문자열은 URL의 port를 의미합니다.
- `()` 는 그룹핑을 위한 것입니다.
- `:` 는  port 의 앞에 오는 문자를 의미합니다.
- 여러 개의 숫자로만 구성되는 문자열과 매칭됩니다.
- `?` 는 수량자 `{0,1}` 과 동일한 의미로 존재할 수도 있고, 없을 수도 있다는 optional 스펙을 반영한 것입니다.

<br>

`([\\w/~-]*)?`

- 매칭되는 문자열은 URL의 path를 의미합니다.
- `()` 는 그룹핑을 위한 것입니다.
- alphanumeric 문자와 특수문자 `/` , `~`, `-` , `_`이 들어갈 수 있도록 허용했습니다. (`\\w` 는 alphanumeric + `_`를 포함)
- `?` 는 수량자 `{0,1}` 과 동일한 의미로 존재할 수도 있고, 없을 수도 있다는 optional 스펙을 반영한 것입니다.
- 사실, authority 가 없는 경우도 고려해야하지만, 일반적인 URL의 형태는 authority 가 있는 형태이므로 이 정규식에서는 고려하지 않았습니다.

<br>

`([\\w?&=;~-]*)?`

- 매칭되는 문자열은 URL의 query를 의미합니다.
- `()` 는 그룹핑을 위한 것입니다.
- alphanumeric 문자와 특수문자 `?` , `&` , `=` , `;` , `~`, `-` , `_`이 들어갈 수 있도록 허용했습니다. (`\\w` 는 alphanumeric + `_`를 포함)
- `?` 는 수량자 `{0,1}` 과 동일한 의미로 존재할 수도 있고, 없을 수도 있다는 optional 스펙을 반영한 것입니다.

<br>

### 참고자료

https://en.wikipedia.org/wiki/URL

https://github.com/milanoderby/TIL/blob/master/CS/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/URI%2C%20URL%2C%20URN.md

https://github.com/milanoderby/TIL/blob/master/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98/%EC%A0%95%EA%B7%9C%ED%91%9C%ED%98%84%EC%8B%9D.md
