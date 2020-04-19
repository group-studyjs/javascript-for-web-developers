# 언어의 기초

### 1. 변수명 짓기

변수명(식별자)를 지을 때, 지켜야할 규칙이 있다.

1. 첫번 째 문자는 **글자** 혹은 **\_**, **\$** 만 가능하다.

```js
const 1num; // x
const num1 // o

const _name; // o
const $name; // o
```

2. 예약어는 변수명으로 불가능.

```js
const true; // x
const case; // x
// ...
```

3. 카멜케이스, 스네이크케이스 (option)

```js
const myname // bad
const myName // good

const MAXLENGTH = 10 // bad
const MAX_LENGTH = 10 // good
```

> 카멜케이스(대문자사용), 스네이크케이스(언더바사용) 을 혼용해서 사용한다고 한다.

<hr/>

### 2. 세미콜론

책에서는 세미콜론을 항상 쓰는 것을 권장하지만, 인터넷을 찾아보니 세미콜론을 쓰지 않아도 된다는 글도 있었다.

- 참고 글 : [https://bakyeono.net/post/2018-01-19-javascript-use-semicolon-or-not.html](https://bakyeono.net/post/2018-01-19-javascript-use-semicolon-or-not.html)

세미콜론을 쓰지 않아도 되는 이유는 인터프리터가 **문장의 끝이라고 생각되는 지점**에 세미콜론을 자동으로 붙여주기 때문이다. 이 기능을 세미콜론 자동 삽입(**ASI**, automatic semicolon insertion)이라고 한다.

책에서는 세미콜론을 붙이지 않았을 때의 문제를 다음과 같은 예시를 들어 설명한다.

```js
function returnObject() {
    return
    {
        a:1,
        b:2
    }
}
```

위 코드는 인터프리터가 ASI를 통해 다음과 같이 인식해서 `undefined`를 return 하게 된다.

```js
function returnObject() {
    return;
    {
        a:1,
        b:2
    };
}
```

하지만, 다음과 같이 짜면 정상적으로 인식한다.

```js
function returnObject() {
  return {
    a: 1,
    b: 2,
  }
}
```

그리고 해당 글에서는 세미콜론을 넣지 않아도 된다는 데서 더 나아가, 세미콜론을 쓰지 말아야 한다고 주장하는 이유에 대해 설명한다. ASI 방식을 이해하고 그에 맞는 코딩스타일을 정립하면 세미콜론을 넣을 필요가 없고, 오히려 세미콜론을 넣는 규칙을 신경쓰는 것이 더 성가실 수도 있다는 것이다.

```js
class Foo {
  constructor() {
    if (baz) {
      return 42 // ok
    } // <– AVOID!
    return 12 // ok
  } // <– AVOID!
} // <– AVOID!
```

그리고 ASI가 일을 방해하지 않도록 하려면 딱 한 가지 규칙만 기억하면 된다고 한다. 그것은 바로 행의 시작을 `\[`, `\(`, `\``으로 하지 않는 것이다. 꼭 그렇게 해야 한다면, 다음과 같이 세미콜론으로 행을 시작하면 된다고 한다.

```js
;[1, 2, 3].forEach(bar)
```

이를 피하기 위해서 값을 변수안에 집어넣거나 `const numbers = [].forEach()` 값을 return 해줄 수도 있다. `return [].forEach()`

<hr/>

### 3. data Type

자바스크립트 데이터 타입은 총 7개로 원시타입 6개, 참조타입 1개로 이루어져있다.

| 원시타입  | 참조타입 |
| --------- | -------- |
| undefined | Object   |
| null      |          |

> symbol은 ECMAScript 6에서 추가되었음.

### 4. [type 1]. undefined

변수를 선언하고 초기화시켜주지 않으면 해당 변수는 자동으로 **undefined** 값을 할당받는다.

이 값은 빈 값(`null`)이 아니며 실제로는 Garbage 값을 가지고 있다고 한다.

```js
let message
typeof message
// "undefined"
```

추가로 호이스팅시에도 , 변수에 자동으로 **undefined** 값을 할당받는다. `var`

```js
console.log(message) // undefined
var message = 'hi'
```

하지만 let,const 키워드를 사용하면 **undefined**를 할당하기 전에 호출하기 때문에 error를 출력한다. `let, const`

```js
console.log(message) // error
let message

let message
console.log(message) // undefined
```

> 이를 TDZ라고 함.

<hr/>

### 5. [type 2]. null

**null**은 빈 값을 의미하며 정확히는 **빈 객체**를 가리킨다. 그렇기 때문에 typeof 연산자를 썼을 때 null일 경우, object를 반환한다.

```js
let car = null
typeof car
// "object"
```

> `null==undefined`이 true로 나오는 것은 다음장에서 설명

<hr/>

### 5. [type 3]. boolean

**boolean** 은 true, false 두가지 값만 가지는 타입이다. 하지만 Boolean() 함수를 호출해서 다른 타입을 boolean 타입으로 변환시킬 수도 있고, if문 같은 제어문에서는 타입들이 자동으로 boolean으로 변환된다.

| type      | true       | false        |
| --------- | ---------- | ------------ |
| undefined |            | undefined    |
| null      |            | null         |
| 숫자      | 0빼고 다   | 0,NaN        |
| 문자열    | ''빼고 다  | ''(빈문자열) |
| 객체      | 다({}포함) | null         |

<hr/>

### 6. [type 4]. 숫자

일반적으로 숫자를 변수에 할당하면 **10진법**의 숫자가 변수에 담긴다.

하지만, 앞자리에 `0` (8진법), `0x` (16진법)을 붙이면 각각 8진법, 16진법의 숫자를 할당할 수도 있다. 하지만 다음 각각의 숫자들이 해당진법의 숫자를 넘지 않아야함.

```js
// 8진법
let num = 070 // 56

// 9가 8진법을 넘으므로, 10진법으로 출력됨
let num = 079 // 79
```

```js
let num = 0xa // 10

// 대소문자는 가리지 않는다.
let num = 0xf // 15
```

부동소수점 숫자인 경우, 사칙연산할 때 주의해야함! 정수인 경우, 정확한 계산이 되지만, 부동소수점 숫자는 부정확하다.

```js
if (1 + 1 == 2) console.log('true')
// true

if (1.5 + 0.5 == 2) console.log('true')
// 출력 하지 않음 2.000000000004
```

NaN은 숫자의 값 중 하나로, 숫자로 변환할 수 없는 경우 반환되는 값이다.

```js
isNaN(NaN) // true
isNaN('blue') // true

isNaN(10) // false : 10
isNaN('10') // false : 10
isNaN(true) // false : 1
isNaN(false) // false : 0
```

즉, 자바스크립트에서는 숫자가 아니지만 숫자로 변환할 수 있는 타입끼리의 연산은 자동으로 변환해서 연산을 수행한다.

```js
console.log('6' - '1') // 5
console.log(true * 3) // 3

// 예외적으로 문자열 + 문자열은 문자열로서 더하기를 수행함
console.log('5' + '5' == '55') // true, 10이 아님!
```

추가로, 자바스크립트에서는 0으로 나누는 것이 허용된다. 하지만 숫자가 아닌 값이나 0을 0으로 나눌 경우 NaN을 출력한다.

```js
console.log(1 / 0) // Infinity
console.log(1 / -0) // -Infinity

console.log('숫자가 아님' / 0) // NaN
console.log(0 / 0) // NaN
```

그리고 숫자가 아닌 값을 숫자로 바꾸는 함수는 대표적으로 세가지이며 `+`를 붙이는 방식도 있다.

| 변환방식     | 설명                                                                                                                                            |
| ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| Number()     | 리딩제로(숫자 앞에 붙이는 0)을 무시함.(16진법 제외) 그렇기 때문에 8진법 표기법(070)은 모두 10진법으로 변환되지만 16진법 표기법(0xA)는 잘 표기됨 |
| parseInt()   | 마찬가지 리딩제로 무시함.(16진법 제외) 문자열의 경우, 숫자가 아닌 문자가 나올 때까지 변환. 부동소수점은 버림. 두번째 인자로 x진법 변환 가능     |
| parseFloat() | 리딩제로 항상 무시함. (16진법 포함)                                                                                                             |
| +(값 앞에)   |                                                                                                                                                 |

```js
Number('1234dd') // NaN
Number('070') // 70

parseInt('1234dd') // 1234
parseInst('12dd34dd') // 12
parseInt('070') // 70
parseInt('70', 8) // 56

parseFloat('123.45.6') // 123.45
parseFloat('0xA') // 0

let num = '123'
typeof num // string
typeof +num // number
```

x진법의 수(로변환 가능한 값)을 10진법으로 변환할 때는 `parseInt`, 10진법의 수를 x진법의 문자열로로 고칠 경우에는 `toString` 메소드를 사용하면 된다.

```js
let num = 70
num.toString(2) // "1000110"

parseInt('1000110', 2) // 70
```

### 7. [type 5]. 문자열

문자열 내부에서 특수문자는 보통 `\`기호를 이용해서 사용한다.

> 책에서는 [[리터럴(literal)](https://ko.wikipedia.org/wiki/%EB%A6%AC%ED%84%B0%EB%9F%B4) : 소스 코드의 고정된 값을 대표하는 용어]라고 정의함

| 리터럴 | 의미                                      |
| ------ | ----------------------------------------- |
| \n     | 줄바꿈                                    |
| \t     | 탭                                        |
| \b     | 백스페이스                                |
| \xnn   | 16진수 코드(\x41 : A)                     |
| \unnnn | UTF-16 인코딩 규칙을 사용하는 16진수 코드 |

그리고 문자열은 **불변성**의 성질을 가지고 있어서 문자열 자체를 바꿀 수 없고, 해당 문자열 변수에 새로운 값을 할당하는 방식으로 값을 바꾸어야 함.

```js
let str = 'hot'
str[0] = 'p' // 변화없음

str = 'p' + str.slice(1) // good
str = str.replace('h', 'p') // good

// 이런식으로 새로 할당해주어야됨.
```

그리고 문자가 아닌 값을 문자열로 변환방법은 대표적으로 두가지이며 `""`를 더하는 방식도 있다.

| 변환방식   | 설명                                                       |
| ---------- | ---------------------------------------------------------- |
| String()   | null, undefined같은 toString 메소드가 없는 값들도 변환가능 |
| toString() | 인자에 들어가는 값 x에 따라 x진법의 수로 변환 가능         |
| (값)+ ""   | 리딩제로 항상 무시함. (16진법 포함)                        |

```js
let value = null
String(value) // "null"
value.toString() // error

let num = 5
let value = num + ''
typeof value // string
```

### 8. [type 6]. 객체

ECMAScript 에서 객체는 데이터와 기능의 집합을 의미한다.

```js
const obj = new Object()
const arr = new Array()
const func = new Function()
```

> 의문! 배열,함수,객체인스턴스 모두 객체인가??

프로퍼티(속성)과 메소드를 추가하여 독립된 새로운 객체를 만들 수 있다.

```js
var obj = {
    // 속성
    property : "property",

    // 메소드
    method: function() {}
    // 혹은
    method() {}
}
```

new 키워드를 통하여 인스턴스를 만들 수도 있다.

```js
function User(name) {
  this.name = name
  this.isAdmin = false
}

let user = new User('Jack')

console.log(user.name) // Jack
console.log(user.isAdmin) // false
```

### 참고

[https://bakyeono.net/post/2018-01-19-javascript-use-semicolon-or-not.html](https://bakyeono.net/post/2018-01-19-javascript-use-semicolon-or-not.html)

[https://developer.mozilla.org/ko/docs/Web/JavaScript/Data_structures](https://developer.mozilla.org/ko/docs/Web/JavaScript/Data_structures)

[https://ko.javascript.info/constructor-new](https://ko.javascript.info/constructor-new)
