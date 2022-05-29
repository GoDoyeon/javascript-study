# 15장. let, const 키워드와 블록 레벨 스코프

## 15.1 var 키워드로 선언한 변수의 문제점

- 변수 중복 선언 허용
- 함수 레벨 스코프
- 변수 호이스팅에 의해 변수 선언문 이전에 참조 가능

**함수 레벨 스코프(Function-level Scope)**  
함수 내에서 선언된 변수는 함수 내에서만 유효하며 함수 외부에서는 참조할 수 없다. 즉, 함수 내부에서 선언한 변수는 지역 변수이며 함수 외부에서 선언한 변수는 모두 전역 변수이다.

**블록 레벨 스코프(Block-level Scope)**  
모든 코드 블록(함수,if문,for문,while문,try/catch문 등)내에서 선언된 변수는 코드 블록 내에서만 유효하며 코드 블록 외부에서는 참조할 수 없다. 즉, 코드 블록 내부에서 선언한 변수는 지역 변수이다.

### 15.1.1 변수 중복 선언

```jsx
var x = 1;
var y = 1;

// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
// 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
var x = 100;
// 초기화문이 없는 변수 선언문은 무시된다.
var y;

console.log(x); // 100
console.log(y); // 1
```

### 15.1.2 함수 레벨 스코프

var 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정한다.  
함수 레벨 스코프는 전역 변수를 남발할 가능성을 높여 의도치 않게 전역 변수가 중복 선언되는 경우가 발생한다.

```jsx
var x = 1;

if (true) {
  // x는 전역 변수다. 이미 선언된 전역 변수 x가 있으므로 x 변수는 중복 선언된다.
  // 이는 의도치 않게 변수값이 변경되는 부작용을 발생시킨다.
  var x = 10;
}

console.log(x); // 10
```

```jsx
var i = 10;

// for문에서 선언한 i는 전역 변수이다. 이미 선언된 전역 변수 i가 있으므로 중복 선언된다.
for (var i = 0; i < 5; i++) {
  console.log(i); // 0 1 2 3 4
}

// 의도치 않게 i 변수의 값이 변경되었다.
console.log(i); // 5
```

### 15.1.3 변수 호이스팅

`var` 키워드로 변수를 선언하면 변수 호이스팅에 의해 변수 선언문 이전에 참조할 수 있다.

```jsx
// 이 시점에는 변수 호이스팅에 의해 이미 foo 변수가 선언되었다(1. 선언 단계)
// 변수 foo는 undefined로 초기화된다. (2. 초기화 단계)
console.log(foo); // undefined

// 변수에 값을 할당(3. 할당 단계)
foo = 123;

console.log(foo); // 123

// 변수 선언은 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 실행된다.
var foo;
```

## 15.2 let 키워드

`var` 키워드의 단점을 보완하기 위해 ES6에서는 새로운 변수 선언 키워드인 `let` 과 `const` 를 도입했다.

### 15.2.1 변수 중복 선언 금지

```jsx
var foo = 123;
// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
// 아래 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
var foo = 456;

let bar = 123;
// let이나 const 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용하지 않는다.
let bar = 456; // SyntaxError: Identifier 'bar' has already been declared

```

### 15.2.2 블록 레벨 스코프

`var` 키워드로 선언한 변수 : 함수의 코드만을 지역 스코프로 인정하는 함수 레벨 스코프  
`let` 키워드로 선언한 변수 : 모든 코드 블록(함수, if문, for문 등)을 지역 스코프로 인정하는 블록 레벨 스코프

```jsx
// 블록 레벨 스코프
let foo = 1; // 전역 변수

{
  let foo = 2; // 지역 변수
  let bar = 3; // 지역 변수
}

console.log(foo); // 1
console.log(bar); // ReferenceError: bar is not defined

```

### 15.2.3 변수 호이스팅

`let` 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 동작하지만 여전히 호이스팅이 발생하기 때문에 참조 에러가 발생한다.

```jsx
console.log(foo); // ReferenceError: foo is not defined
let foo;

```

`var` 키워드로 선언한 변수 : 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 선언 단계와 초기화 단계가 한번에 진행된다.  
`let` 키워드로 선언한 변수 : 선언 단계와 초기화 단계가 분리되어 진행된다.  

자바스크립트는 ES6에서 도입된 let, const를 포함해서 모든 선언(var, let, const, function, function\*, class 등)을 호이스팅한다. 단, let, const, class를 사용한 선언문은 호이스팅이 발생하지 않는 것처럼 동작한다.

### 15.2.4 전역 객체와 let

`var` 키워드로 선언한 변수 : (전역 변수와, 전역 함수, 선언하지 않은 변수에 값을 할당한 암묵적 전역은) 전역 객체 window의 프로퍼티가 된다.  
`let` 키워드로 선언한 변수 : 전역 객체의 프로퍼티가 아니다. -> `window.foo` 와 같이 접근할 수 없다.

```jsx
// 전역 변수
var x = 1;
// 암묵적 전역
y = 2;
// 전역 함수
function foo() {}

// var 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티다.
console.log(window.x); // 1
// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(x); // 1

// 암묵적 전역은 전역 객체 window의 프로퍼티다.
console.log(window.y); // 2
console.log(y); // 2

// 함수 선언문으로 정의한 전역 함수는 전역 객체 window의 프로퍼티다.
console.log(window.foo); // ƒ foo() {}
// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(foo); // ƒ foo() {}
```

## 15.3 const 키워드

`const` 키워드는 상수를 선언하기 위해 사용한다. (반드시 상수만을 위해 사용하는 것은 아님)

### 15.3.1 선언과 초기화

`const` 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 한다.

```jsx
const x = 1;

const y; // SyntaxError: Missing initializer in const declaration
```

`const` 키워드로 선언한 변수는 `let` 키워드로 선언한 변수와 마찬가지로 블록 레벨 스코프를 가진다.

```jsx
// 블록 레벨 스코프를 갖는다.
console.log(foo); // ReferenceError: foo is not defined
```

`const` 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 동작한다.

```jsx
{
  // 변수 호이스팅이 발생하지 않는 것처럼 동작한다
  console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
  const foo = 1;
  console.log(foo); // 1
}
```

### 15.3.2 재할당 금지

`const` 키워드로 선언한 변수는 재할당이 금지된다.

```jsx
const foo = 1;
foo = 2; // TypeError: Assignment to constant variable.
```

### 15.3.3 상수

상수는 재할당이 금지된 변수를 말한다.  
`const` 키워드로 선언한 변수에 원시 값을 할당한 경우 변수 값을 변경할 수 없다.  

상수의 이름은 대문자로 선언해 상수임을 명확히 나타낸다.  

```jsx
// 세율을 의미하는 0.1은 변경할 수 없는 상수로서 사용될 값이다.
// 변수 이름을 대문자로 선언해 상수임을 명확히 나타낸다.
const TAX_RATE = 0.1;

// 세전 가격
let preTaxPrice = 100;

// 세후 가격
let afterTaxPrice = preTaxPrice + (preTaxPrice * TAX_RATE);

console.log(afterTaxPrice); // 110

```

### 15.3.4 const 키워드와 객체

`const` 키워드로 선언한 변수에 원시 값을 할당한 경우 변수 값을 변경할 수 없다. 하지만 `const` 키워드로 선언한 변수에 객체를 할당한 경우 값을 변경할 수 있다.  
`const` 키워드는 재할당을 금지할 뿐 “불변”을 의미하지 않는다. 새로운 값을 재할당 하는 것은 불가능하지만 프로퍼티 동적 생성, 삭제, 프로퍼티 값의 변경을 통해 객체를 변경하는 것은 가능하다.

```jsx
const person = {
  name: 'Lee'
};

// 객체는 변경 가능한 값이다. 따라서 재할당없이 변경이 가능하다.
person.name = 'Kim';

console.log(person); // {name: "Kim"}

```

## 15.4 var vs let vs const

변수 선언에는 기본적으로 `const` 를 사용하고 `let` 은 재할당이 필요한 경우에 한정해 사용하는 것이 좋다.  
변수의 스코프는 최대한 좁게 만든다.
