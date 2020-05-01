# 1. 객체로 매개변수 전달 시, 값으로 전달

예제코드1. obj 객체의 값은 변경가능하지만 다른 객체를 가리키게는 하지 못함.

```js
function setName(obj) {
  obj.name = 'Nicholas'
  obj = new Object()
  obj.name = 'Greg'
  // 바뀐 obj 객체의 값을 사용하려면 이 값자체를 리턴해주면 된다.
  return obj
}

let person = new Object()
console.log(setName(person).name) // Greg
console.log(person.name) // Nicholas
```

예제코드2. 객체 변수의 값은 객체 자체가 아니라 힙에 저장된 객체를 가리키는 포인터이다.

```js
let obj1 = new Object()
let obj2 = obj1
obj1.name = 'Nicholas'
console.log(obj2.name) // Nicholas
```

# 2. 타입판별 instanceOf

원시타입이 아닌 객체일 경우 typeof로 타입을 유추하기 어려울 수 있음. 이 때 `instanceof`를 이용해주면 좋음.

```js
const numbers = [1, 2, 3, 4]
typeof numbers //  "object"
numbers instanceof Object // true

numbers instanceof Array // true
numbers instanceof Function // false
```

# 3. 실행 컨텍스트

**실행 컨텍스트** : 자바스크립트가 해석되고 실행되는 환경

> **execution context** is an abstract concept of an environment where the Javascript code is evaluated and executed. Whenever any code is run in JavaScript, it’s run inside an execution context.

처음 코드를 읽을 때, 모든 실행 컨텍스트를 포괄하는 전역 컨텍스트 가 생성되며, 전역 컨텍스트 내에서 함수가 호출되면 함수 컨텍스트가 생성된다.

자바스크립트 엔진은 이러한 컨텍스트를 `객체` 형태로 관리한다.

이러한 컨텍스트 (환경) 내에서는 `변수객체` , `scope chain` , `this` 가 담긴다.

# 4 .실행 컨텍스트 구성

## 4-1. 변수 객체 (변수, 함수선언, arguments)

전역객체의 경우, arguments는 존재하지 않음.

함수실행 컨텍스트는 실행될 때가 아닌 선언될 때, 변수 객체를 구성함.

선언식 함수 객체의 경우 변수구성시, [[Scopes]] 프로퍼티를 가지게 된다. [[Scopes]] 프로퍼티는 함수 객체만이 소유하는 내부 프로퍼티(Internal Property)로서 자신의 실행 환경(Lexical Enviroment)과 자신을 포함하는 외부 함수의 실행 환경과 전역 객체를 가리키는데 이때 자신을 포함하는 외부 함수의 실행 컨텍스트가 소멸하여도 [[Scopes]] 프로퍼티가 가리키는 외부 함수의 실행 환경(Activation object)은 소멸하지 않고 참조할 수 있다. 이것이 클로저이다.

> 요거 어렵다..ㅠㅠ

![](https://poiemaweb.com/img/ec_7.png)

함수선언식은 변수 객체(VO)에 함수명을 프로퍼티로 추가하고 즉시 함수 객체를 즉시 할당하지만 함수 표현식은 일반 변수의 방식을 따른다.

그렇기 때문에 함수선언식은 함수호이스팅이 가능하지만, 표현식은 함수호이스팅이 되지 않는다.

```js
// 함수 선언식

hello() // "hello"
let num = 0

function hello() {
  return console.log('hello')
}
// 실행 컨텍스트에서의 변수객체 : [num, hello]
```

> 의문! 여기서 hello 함수의 [[Scopes]]에 전역 스코프가 담기기 때문에 hello() 해도 전역스코프를 참조하기 때문에 hello함수를 잘 가져오는 것?

```js
// 함수 표현식

hello() // Uncaught TypeError: hello is not a function at <anonymous>
let num = 0

const hello = function () {
  return console.log('hello')
}
// 전역 컨텍스트에서의 변수객체 : [num, hello]
```

> 의문! 여기서는 일반 변수로 hello가 컨텍스트 변수객체에 들어갔으나, undefined 상태라 에러?

[함수호이스팅](https://poiemaweb.com/js-function#2-%ED%95%A8%EC%88%98-%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85)

## 4-2. scope chain

스코프 체인(Scope Chain)은 일종의 리스트로서 전역 객체와 중첩된 함수의 스코프의 레퍼런스를 차례로 저장하고 있음. 해당 컨텍스트에 변수정보가 없을 시, 이 scope chain을 통해 scope를 참조한다.

스코프 체인은 호출시 구성되는 것이 아니라 선언시 구성됨. `lexical scoping`

```js
function foo() {
  console.log(x)
}

function bar() {
  let x = 15
  foo()
}

let x = 10
foo() // 10
bar() // 10
```

여기서 foo의 scope chain은 [전역객체, foo] 이고,

foo가 전역에서 실행되든, bar안에서 실행되든 전역변수 x = 10을 참조한다.

## 4-3. this

this는 현재 실행 컨텍스트에서의 호출자를 의미한다.

```js
const obj = {
  value: 'obj의 값',
  inner: function () {
    console.log(this.value) // 'obj의 값' 출력
  },
}
obj.inner()
```

브라우저 환경에서는 호출자 this에 window 전역객체가 담긴다.

## 4-4. use-strict 에서 this

‘use strict’ 모드에서는 현재 this의 값이 null 또는 undefined인 경우 전역 객체를 변환하지 않는다.

```js
function nonStrictMode() {
  return this
}

function strictMode() {
  'use strict'
  return this
}

console.log(nonStrictMode()) // window
console.log(strictMode()) // undefined
console.log(window.stricMode()) // window
```

# 5. 자바스크립트의 블록 레벨 스코프

var 키워드는 블록레벨스코프를 지원하지 않지만 let 키워드는 블록레벨스코프를 지원한다.

```js
for (var i = 0; i < 10; i++) {}
console.log(i) // 10

for (let j = 0; j < 10; j++) {}
console.log(j) // error
```

# 6. 메모리 관리

일반적으로 가비지 컬렉션을 지원하는 프로그래밍 환경에서는 개발자가 메모리 관리를 신경쓰지 않아도 되지만, 웹 브라우저에서 사용할 수 있는 메모리는 일반적인 데스크톱 애플리케이션의 가용 메모리에 비해 매우 적기 때문에 가능한 한 최소한의 메모리만 사용해서 페이지의 성능을 올릴 수 있도록 해야한다.

메모리 사용을 최적화하는 가장 좋은 방법은 코드 실행에 필요한 데이터만 유지하는 것이다. 필요없어진 데이터는 null을 할당하여 참조를 제거하여준다. 지역변수의 경우에는 컨텍스트를 빠져나가는 순간 자동으로 참조가 제거되기 때문에 주로 전역변수 및 전역객체의 프로퍼티가 직접 제거해야할 대상이다.

# 7. 참고

[https://www.zerocho.com/category/JavaScript/post/5741d96d094da4986bc950a0](https://www.zerocho.com/category/JavaScript/post/5741d96d094da4986bc950a0)

[https://poiemaweb.com/js-execution-context](https://poiemaweb.com/js-execution-context)

[https://poiemaweb.com/js-function#2-%ED%95%A8%EC%88%98-%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85](https://poiemaweb.com/js-function#2-%ED%95%A8%EC%88%98-%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85)
