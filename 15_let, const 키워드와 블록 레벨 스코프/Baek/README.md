# 15. let, const 키워드와 블록 레벨 스코프

Created: 2022년 6월 23일 오후 3:29

## 15.0. 새롭게 알게된 것들

- 변수의 라이프 사이클
- `const`의 맹점

### 15.0.1. 질문 리스트

- var 키워드의 문제점은 무엇이 있나요? 🔥🔥
- let 키워드는 var 키워드와 어떤 점이 다른가요? 🔥🔥🔥
- TDZ 🔥🔥🔥
- const 키워드는 어떤 특징이 있나요? 🔥🔥
- 식별자 네이밍 규칙은 어떤 것들이 있나요?
- 네이밍 컨벤션은 어떤 것들이 있나요?

## 15.1. var 키워드로 선언한 변수의 문제점

- ES5까지의 변수 생성법인 `var` 의 특징 때문에 생기는 문제들

### 15.1.1. 변수 중복 선언 허용

- `var` 키워드로 선언한 변수는 중복 선언이 가능.

```tsx
var x = 1;
var y = 1;

var x = 100;
var y; // 초기화 문이 없는 변수 선언문인 경우 무시

console.log(x); // 100
console.log(y); // 1
```

- 5번째 줄의 `var y`는 그냥 `y;` 로 취급
- 위 예제처럼 이미 선언된 변수에 새로 할당하게 되면 이미 선언된 변수가 의도와는 다르게 변경되는 부작용 발생

### 15.1.2. 함수 레벨 스코프

- `var` 키워드로 선언한 변수는 함수 레벨 스코프
- 따라서 함수 외부에서 `var` 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수가 된다.
- `if`문의 예시

```tsx
var x = 1;

if (true) {
  var x = 10; // 중복 선언됨
}

console.log(x); // 10
```

- `for`문의 예시

```tsx
var i = 10;

for (var i = 0; i < 5; i++) {
  console.log(i); // 0 1 2 3 4
}

// 의도치 않게 i 변수의 값이 변경됨
console.log(i); // 5
```

함수 레벨 스코프는 전역 변수 남발할 가능성을 높임. 의도치 않은 중복 선언 발생

### 15.1.3. 변수 호이스팅

`var` 키워드로 변수를 선언하면 호이스팅에 의해 선언문이 스코프의 선두로 끌어 올려져서 동작한다.

```tsx
console.log(foo); // undefined
foo = 123;

console.log(foo); // 123

var foo;
```

가독성을 떨어뜨리고 오류를 발생시킬 여지를 남긴다.

## 15.2. `let` 키워드

`var`키워드의 단점을 보완하기 위해 ES6에서 새로운 변수 선언 키워드인 `let`과 `const`를 도입

### 15.2.1. 변수 중복 선언 금지

동일한 스코프에서 이름이 같은 변수를 중복 선언하면 `syntax error` 발생

```tsx
var foo = 123;
var foo = 456; // ok

let bar = 123;
let bar = 456; // SyntaxError: Identifier 'bar' has already been declared
```

### 15.2.2. 블록 레벨 스코프

`var`키워드로 선언한 변수는 함수 레벨 스코프를 따름

하지만 `let`키워드로 선언한 변수는 모든 코드블록(함수, `if`문, `for`문, `while`문, `try/catch`문 등)을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.

```tsx
let foo = 1;

{
  let foo = 2;
  let bar = 3;
}

console.log(foo); // 1
console.log(bar); // ReferenceError: bar is not defined
```

`let`키워드로 선언된 변수는 블록 레벨 스코프를 따름. 따라서 위 예제의 코드 블록 내에서 선언된 `foo`변수와 `bar`변수는 지역 변수. 지역 `foo`는 전역 `foo`와 이름만 같을 뿐 다른 변수. `bar`는 블록 스코프 외부에서는 참조할 수 없음

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F13e90908-0bed-465a-b857-98d5d46cd454%2FUntitled.png?table=block&id=206b8021-5e78-45e1-af49-62a214a24370&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

함수도 코드 블록이므로 스코프를 만들고, 함수 내의 코드 블록은 함수 레벨 스코프에 중첩된다.

### 15.2.3. 변수 호이스팅

`var`와는 다르게 `let`은 호이스팅이 발생하지 않는다.

```tsx
console.log(foo); // ReferenceError: foo is not defined
let foo;
```

`let` 키워드로 선언한 변수를 선언문 이전에 참조하면 `ReferenceError`가 발생

`var` 키워드는 런타임 이전에 선언 단계와 초기화 단계가 한 번에 진행

즉, 선언 단계에서 컨텍스트의 렉시컬 환경에 식별자를 등록해 자바스크립트 엔진에 변수의 존재를 알린다. 그리고 즉시 `undefined`로 변수를 초기화 한다.

따라서 선언문 이전에 변수에 접근하더라도 스코프에는 변수가 존재하기 때문에 에러가 발생하지 않고, 초기화한 `undefined`가 반환된다.

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fd62b1e75-678a-4c65-b700-44aae14167a6%2FUntitled.png?table=block&id=2206a66c-464a-4a83-9886-f41276f6783c&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

```tsx
// 런타임 이전에 이미 선언과 초기화가 발생
console.log(foo); // undefined

var foo;
console.log(foo); // undefined

foo = 1;
console.log(foo); // 1
```

반면, `let`키워드로 선언한 변수는 선언 단계와 초기화 단계가 분리되어 진행된다. 즉, 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 선언 단계가 먼저 실행되지만, 초기화 단계는 변수 선언문에 도달했을 때 실행된다.

초기화 단계가 실행되기 이전에 변수에 접근하게 되면 `Reference Error`가 발생하는데, 스코프 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간을 일시적 사각지대 `Temporal Dead Zone`이라고 부른다.

```tsx
// 런타임 이전: 선언 단계
// TDZ
console.log(foo);

// TDZ End
// 초기화 단계
let foo;
console.log(foo); // undefined

// 할당 단계
foo = 1;
console.log(foo); // 1
```

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fa10b811f-9bf3-4dde-8774-d8bae89ba947%2FUntitled.png?table=block&id=b34f459a-56fb-45b3-9cdb-4cca33c10d40&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

`let`키워드로 선언한 변수는 호이스팅이 일어나지 않는 것 처럼 보인다.

하지만 실제로 발생하지 않는 것은 아님

```tsx
let foo = 1; // global
{
  console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
  let foo = 2; // local
}
```

`let` 키워드로 선언한 변수의 경우 변수 호이스팅이 발생하지 않는다면 전역 변수 `foo`의 값을 출력해야한다. 하지만 `let` 키워드로 선언한 변수도 여전히 호이스팅이 발생하기 때문에 `ReferenceError`가 발생한다.

`javascript`는 ES6에서 도입 된 `let`, `const`를 포함해서 모든 선언(`var`, `function`, `function*`, `class` 등) 을 호이스팅한다. 단 ES6에서 도입 된 `let`, `const`, `class`를 사용한 선언문은 호이스팅이 발생하지 않는 것 처럼 동작한다.

### 15.2.4. 전역 객체와 `let`

`var` 키워드로 선언한 전역 변수와 전역 함수, 그리고 선언하지 않은 변수에 값을 할당한 암묵적 전역은 전역 객체 `window`의 프로퍼티가 된다. 전역 객체의 프로퍼티를 참조할 때는 `window`를 생략할 수 있다.

- 브라우저 환경에서만 해당하는 얘기인 듯합니다

```tsx
// 전역 변수
var x = 1;
// 암묵적 전역
y = 2;
// 전역 함수
function foo() {}

console.log(window.x); // 1

console.log(x); // 1

console.log(window.y); // 2
console.log(y); // 2

console.log(window.foo); // f foo() {}
console.log(foo); // f foo() {}
```

반면, `let` 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아님. 즉, `window.foo`와 같이 접근할 수가 없음. `let` 전역 변수는 보이지 않는 개념적인 블록 내에 존재하게 된다. (23장 참조)

```tsx
let x = 1;
console.log(window.x); // undefined
```

## 15.3. `const` 키워드

`const` 키워드는 상수를 선언하기 위해 사용한다. 하지만 반드시 상수만을 위해 사용하는 것은 아님. `const` 키워드는 대부분의 특징이 `let`과 동일. `let`과의 차이점을 중심으로 설명.

### 15.3.1. 선언과 초기화

**`const` 키워드로 선언한 변수는 반드시 선언과 동시에 초기화를 해야한다.**

```tsx
const foo = 1;
```

```tsx
const foo;
```

위의 경우 `SyntaxError`가 발생

`const` 키워드로 선언한 변수는 `let` 키워드로 선언한 변수와 마찬가지로 블록 레벨 스코프를 따름.

변수 호이스팅이 발생하지 않는 것 **처럼** 동작

```tsx
{
  console.log(foo); // RefError : Cannot access 'foo' before initialization
  const foo = 1;
  console.log(foo); // 1
}

console.log(foo); // RefError: foo is not defined
```

### 15.3.2. 재할당 금지

`const`는 다른 변수와는 다르게 유일하게 재할당 금지

```tsx
const foo = 1;
foo = 2; // TypeError: Assignment to constant variable
```

### 15.3.3. 상수

> 상수는 재할당이 금지된 변수

`const` 키워드로 선언한 변수에 **원시 값**을 할당한 경우 변수 값을 변경할 수 없다. 원시 값은 변경 불가능한 값이므로 재할당 없이 값을 변경할 수 있는 방법이 없기 때문인데, 이러한 특징을 이용해 `const`로 상수를 표현하는 데 사용하기도 함.

```tsx
let preTaxPrice = 100;

// 0.1의 가독성이 좋지 않다
let afterTaxPrice = preTaxPrice + preTaxPrice * 0.1;

console.log(afterTaxPrice);
```

- `0.1`은 세율이기 때문에 쉽게 바뀌지 않는 값, 프로그램 전체에서 고정된 값을 사용.
- 이 때 세율을 상수로 정의하면 값의 의미를 쉽게 파악할 수 있고, 변경할 수 없는 고정 값으로 사용 가능.

`const` 키워드로 선언된 변수는 재할당이 금지된다. **만약 원시값이 할당된 경우 변경할 수 있는 방법은 없다.**

또 상수는 프로그램 전체에서 공통적으로 사용하므로, 나중에 세율이 변경되면 상수만 변경하면 되기 때문에 유지보수 성이 대폭 향상된다.

**네이밍 룰**

- 대문자로 선언
- 단어 간의 구문은 언더스코어 `_` 를 사용해 구분하는 스네이크 케이스로 표현하는게 일반적

```tsx
const TAX_RATE = 0.1;

let afterTaxPrice = preTaxPrice + preTaxPrice * TAX_RATE;

console.log(afterTaxPrice);
```

### 15.3.4. `const` 키워드와 객체

`const` 키워드로 선언된 변수에 원시 값을 할당한 경우에는 값을 변경할 수 없지만,
**`const` 키워드로 선언된 변수가 객체 값을 가지는 경우에는 변경할 수 있다.** 그래서 앞서서도, **`const`가 불변을 의미하는 것은 아니**라고 했던 것.

```tsx
const person = {
  name: "Baek",
};

person.name = "Kim";

console.log(person); // {name: "Kim"}
```

- 변수에 할당된 `참조값`은 불변이기는 함
- `readonly`
