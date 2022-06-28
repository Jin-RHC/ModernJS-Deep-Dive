# 20. strict mode란?

## 20.1 strict mode란?

- **자바스크립트** 언어의 문법을 좀 더 엄격히 적용해 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 **문제를 일으킬 수 있는 코드**에 대해 **명시적인 에러를 발생**시킨다.
- ESLint 같은 린트 도구를 사용해도 유사한 효과를 얻을 수 있다. (필자는 린트 도구를 선호한다.)
- **ES6**에서 도입된 클래스와 모듈은 기본적으로 strict mode가 적용된다.

## 20.2 strict mode의 적용

- **strict mode**를 적용하려면 **전역의 선두** 또는 **함수 몸체의 선두**에 `'use strict';`를 추가한다.
- 전역의 선두에 추가하면 스크립트 전체에 **strict mode**가 적용된다.

```python
'use strict';
function foo(){
    x = 10; //ReferenceError: x is not defined
}
foo();
```

- 코드의 선두에 `'use strict';` 를 위치시키지 않으면 strict mode가 제대로 동작하지 않는다.

```python
function foo(){
    x = 10; // 에러를 발생시키지 않는다.
		'use strict';
}
foo();
```

## 20.3 전역에 strict mode를 적용하는 것은 피하자

- 전역에 적용한 strict mode는 스크립트 단위로 적용된다.

```jsx
// 즉시 실행 함수 선두에 strict mode 적용
(function () {
  'use strict';
})();
```

## 20.4 함수 단위로 strict mode를 적용하는 것도 피하자

- 함수 몸체의 선두에 추가한 경우 해당 함수와 중첩함수에 모두 적용된다.
- strict mode와 non strict mode를 함께 쓰는 것은 바람직하지 않으므로 즉시 실행 함수로 감싼 스크립트 단위로 적용하는것이 바람직하다.

```jsx
(function () {
  // non-strict mode
  var let = 10; // 에러가 발생하지 않는다.

  function foo() {
    'use strict';

    let = 20; // SyntaxError
  }
  foo();
})();
```

## 20.5 strict mode가 발생시키는 에러

### 20.5.1 암묵적 전역

- 선언하지 않은 변수를 참조하면 ReferenceError를 발생시킨다.

```jsx
function foo() {
  x = 10;
}
foo();
console.log(x); //10
```

### 20.5.2 변수, 함수, 매개변수의 삭제

- delete 연산자로 변수, 함수 ,매개변수를 삭제하면 **SyntaxError**가 발생한다.

```jsx
(function () {
  'use strict';
  var x = 1;
  delete x;
  function foo(a) {
    delete a;
  }
  delete foo;
})();
```

### 20.5.3 매개변수 이름의 중복

- 중복된 매개변수 이름을 사용하면 **SyntaxError**가 발생한다.

```jsx
(function () {
  'use strict';
  function foo(x, x) {
    return x + x;
  }
  console.log(foo(1, 2));
})();
```

### 20.5.4 with 문의 사용

- **with 문**을 사용하면 SyntaxError가 발생한다.
  - with문은 전달된 객체를 스코프 체인에 추가한다. ⇒ 객체 이름을 생략할 수 있다.
  - 하지만 성능과 가독성이 나빠지는 문제가 있어 with문을 사용하지 않는 것이 좋다,

```jsx
(function () {
  'use strict';
  with ({ x: 1 }) {
    console.log(x);
  }
})();
```

## 20.6 strict mode 적용에 의한 변화

### 20.6.1 일반함수의 this

- 함수를 생성자 함수가 아닌 **일반 함수**로서 호출하면 `this`에 `undefined`가 바인딩된다.
- 생성자 함수가 아닌 일반 함수 내부에서는 this를 사용할 필요가 없기 때문이며 이때 에러는 발생하지 않는다.

```jsx
(function () {
  'use strict';
  function foo() {
    console.log(this); //undefined
  }
  function Foo() {
    console.log(this); //Foo
  }

  foo();
  new Foo();
})();
```

### 20.6.2 arguments 객체

- 매개변수에 전달된 **인수를 재할당하여 변경해도** `arguments`객체에 **반영되지 않는다.**

```jsx
(function (x) {
  'use strict';
  x = 10;
  console.log(arguments); //{0: 5, length: 1}
})(5);
```
