### URI, URL, URN

### 포함관계

![URI/URL Venn Diagram](./images/uri)

### URI - Uniform Resource Identifier

인터넷에 있는 자원을 가리키는 식별자입니다.

URI라는 것은 추상적으로, 자원을 식별할 수 있는 방법에 여러가지가 있을 수 있다는 것을 의미합니다. (자원의 위치, 자원의 이름 등)

URI는 URL과 URN을 포함하는개념입니다.

<br>

### URL - Uniform Resource Locator

URL은 URI의 하위개념으로 인터넷에 있는 자원의 위치정보를 제공합니다.

URL이 URI의 하위개념으로 시작했지만, 사실상 URI의 하위 개념 중 가장 자주 사용되는 개념이기 때문에, 대부분의 파이를 차지한다고 보면 됩니다.

<br>

### URL 의 형태 및 규칙

URL의 기본 형태는 URI를 따르기 때문에 URI의 형태를 먼저 봅니다.

```
scheme:[//[username[:password]@]host[:port]][/path][?query][#fragment]
```

`[]`문자 안의 문자열은 생략가능하다는 것을 의미합니다.

#### URL 형태 다이어그램

![URI syntax diagram](C:\Github\TIL\알고리즘\images\url-regular-expression)

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

### URN - Uniform Resource Name

URN은 URI를 가리키는데, 그 표현방식이 URI의 이름을 가리킵니다.

일반적으로 완전 고유한 값이 아니기 때문에 URI를 가리킬 때, 잘 사용되지 않는 개념입니다.

<br>

### 참고자료

https://stackoverflow.com/questions/176264/what-is-the-difference-between-a-uri-a-url-and-a-urn

https://en.wikipedia.org/wiki/URL