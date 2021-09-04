# Javascript String replace

### 기능 및 문법

문자열에서 어떤 **패턴문자열**을 **대체할 문자열**로 교체하고, **교체된 새로운 문자열(newStr)**을 반환하는 함수입니다.

여기서, **패턴문자열**은 `문자열 리터럴(substr)`이 될 수도 있고, `정규표현식(regexp)`일 수도 있습니다.

**대체할 문자열**은 `문자열 리터럴(newSubstr)`이 될 수도 있고, `함수(function)`일 수도 있습니다.

```javascript
var newStr = str.replace(regexp|substr, newSubstr|function)
```

<br>

### 예시

```javascript
const p = 'The quick dog hot dog';
console.log(p.replace('dog', 'monkey'));
// expected output: "The quick monkey hot dog"

const regex = /Dog/i;
console.log(p.replace(regex, 'ferret'));
// expected output: "The quick ferret hot dog"

const regex2 = /Dog/ig;
console.log(p.replace(regex2, 'ferret'));
// expected output: "The quick ferret hot ferret"
```

위의 예시를 통해 알 수 있듯이 다음을 알 수 있습니다.

1. 리터럴 문자열로 선언한 `패턴문자열`과 일치하는 첫번째 문자열만 교체됩니다.

2. 정규표현식의 `i`옵션은 `패턴문자열`의 대소문자를 구분하지 않는다는 의미입니다.

3. 정규표현식의 `g`옵션은 `패턴문자열`과 일치하는 모든 문자열을 교체한다는 의미입니다.

<br>

### 그룹핑을 이용한 문자열 단어 치환

```javascript
var re = /(\w+)\s(\w+)/;
var str = 'John Smith';
var newstr = str.replace(re, '$2, $1');
console.log(newstr);  // Smith, John
```

문자열과 공백으로 나뉘어진

`패턴문자열`과 일치하는 문자열들을 각각 그룹핑한 결과가 **$N**(N은 숫자)에 맵핑됩니다.

위는 그룹핑된 문자열을 `대체문자열`로 삽입하는 방식입니다.

<br>

### 활용

#### 숫자 외 입력제한

**input태그**에 **input 이벤트**가 발생할 때, 입력 값으로 **숫자** 외의 값이 들어오면, 제거하는 함수입니다.

```typescript
removeCharacterExceptNumber(input: string): string {
    let exceptNumber = /[^0-9]/gi;
    return input.replace(exceptNumber, '');
}
```

<br>

#### 영어 외 입력제한

**input태그**에 **input 이벤트**가 발생할 때, 입력 값으로 **영어** 외의 값이 들어오면, 제거하는 함수입니다.

```typescript
removeCharacterExceptEnglish(input: string): string {
    let exceptEnglish = /[^a-zA-Z]/gi;
    return input.replace(exceptEnglish, '');
}
```

<br>

#### 한글 외 입력제한

**input태그**에 **input 이벤트**가 발생할 때, 입력 값으로 **한글** 외의 값이 들어오면, 제거하는 함수입니다.

```typescript
removeCharacterExceptKorean(input: string): string {
    let exceptKorean = /[^ㄱ-힣]/gi;
    return input.replace(exceptKorean, '');
}
```

<br>

위를 활용하여 복합적으로 입력을 제한할 수 있습니다.

#### 영어, 한글 외 입력제한

**input태그**에 **input 이벤트**가 발생할 때, 입력 값으로 **영어, 한글** 외의 값이 들어오면, 제거하는 함수입니다.

```typescript
removeCharacterExceptKorEng(input: string): string {
    let exceptKoreanEnglish = /[^a-zA-Zㄱ-힣]/gi;
    return input.replace(exceptKoreanEnglish, '');
}
```

<br>

### 참고자료

https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/replace

