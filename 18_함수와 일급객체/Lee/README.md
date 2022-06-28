# 18. 함수와 일급객체

- 일급 객체가 뭔가요?
- 자바스크립트에서 함수가 일급 객체라면, 일급 객체로 뭘 할 수 있나요?
- 꼬리 질문) 함수형 프로그래밍이 뭔가요? 🔥🔥
- 꼬리 질문) 순수 함수가 뭔가요? 일반 함수와는 어떤 차이가 있죠? 🔥🔥

## 18.1 일급 객체

### 일급객체의 조건

1. **무명의 리터럴**로 생성할 수 있다.
2. **변수**나 **자료구조**(객체, 배열 등)에 저장할 수 있다.
3. **함수의 매개변수**에 전달할 수 있다.
4. **함수의 반환값**으로 사용할 수 있다,

⇒ **자바스크립트의 함수**는 모두 만족하므로 **일급객체**이다.

```jsx
// 함수는 무명의 리터럴로 생성 가능, 변수에 저장 가능
const increase = function (num) {
  return ++num;
};

const decrease = function (num) {
  return --num;
};

// 함수는 객체에 저장 가능
const predicates = { increase, decrease };

// 매개변수로 전달 가능, 반환값으로 사용 가능
function makeCounter(predicate) {
  let num = 0;

  return function () {
    num = predicate(num);
    return num;
  };
}

// 매개변수에게 함수 전달 가능
const increaser = makeCounter(predicates.increase);
console.log(increaser()); // 1
console.log(increaser()); // 2

const decreaser = makeCounter(predicates.decrease);
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

- 함수가 **일급 객체**라는 것은 **함수를 객체와 동일**하게 사용할 수 있다는 것이다. **객체**는 **값**이기 때문에 **함수는 값과 동일**하게 취급할 수 있다.
- 함수는 값을 사용할 수 있는 어디서든지 리터럴로 정의할 수 있고, 런타임에 함수 객체로 평가된다.

> 일급 객체로서 함수가 가지는 가장 큰 특징 ⇒ **일반 객체**와 같이 **함수의 매개변수에 전달**할 수 있으며, **함수의 반환값으로 사용**할 수 있다는 것이다.

- **일반 객체와 차이**
  - 일반 객체는 호출할 수 없지만 함수 객체는 호출을 할 수 있다.
  - 함수 객체에는 일반 객체에는 없는 함수 고유의 프로퍼티를 가진다.

## 18.2 함수 객체의 프로퍼티

- 함수는 **객체**이다. ⇒ **프로퍼티**를 가질 수 있다.

![Untitled](18%20%E1%84%92%E1%85%A1%E1%86%B7%E1%84%89%E1%85%AE%E1%84%8B%E1%85%AA%20%E1%84%8B%E1%85%B5%E1%86%AF%E1%84%80%E1%85%B3%E1%86%B8%E1%84%80%E1%85%A2%E1%86%A8%E1%84%8E%E1%85%A6%20d4c9f41b7cdd4105a015ff431f3f45cf/Untitled.png)

- `arguments`, `caller`, `length`, `name`, `prototype` 프로퍼티는 일반 객체에는 없는 **함수 객체의 데이터 프로퍼티**다.
- `__proto__`는 함수 객체의 고유의 프로퍼티가 아니다. `Object.prototype` 객체의 접근자 프로퍼티이다.
- `Object.prototype` 객체의 프로퍼티를 상속 받는데, `Object.prototype` 객체의 프로퍼티는 모든 객체가 상속 받아 사용할 수 있다. 이에 대해서는 19장에 나올 예정이다.

### 18.2.1 arguments 프로퍼티

- `arguments`는 **함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체**이다. 함수 내부에서 지역 변수 처럼 사용된다. ⇒ 함수 외부에서는 참조할 수 없다.
- `arguments` 프로퍼티는 ES3부터 표준에서 폐지되어 `Function.arguments`와 같은 사용법은 권장되지 않고 함수 내부에서 지역 변수처럼 사용할 수 있는 `arguments` 객체를 참조하도록 한다.
- 인수만큼 전달하지 않아도 에러가 발생하지 않고 매개변수는 함수 몸체 내부에서 변수와 동일하게 취급된다. ⇒ 매개변수 선언 후 undefined로 초기화, 인수가 할당

```jsx
function multiply(x, y) {
  console.log(arguments);
  return x * y;
}

console.log(multiply()); // NaN
console.log(multiply(1)); // NaN
console.log(multiply(1, 2)); // 2
console.log(multiply(1, 2, 3)); // 2
```

- arguments 객체의 Symbol() 프로퍼티

유사 배열 객체와 이터러블

- ES6에서 도입된 이터레이션 프로토콜을 준수하면 순회 가능한 자료구조인 이터러블이 된다.
- 이터러블이 도입된 ES6부터 arguments 객체는 유사 배열이면서 동시에 이터러블이다.

### 18.2.2 caller 프로퍼티

- `caller` 프로퍼티는 함수 자신을 호출한 함수를 가리킨다.

```jsx
function foo(func) {
  return func();
}

function bar() {
  return 'caller: ' + bar.caller;
}

console.log(foo(bar)); // caller: function foo(func) {...}
console.log(bar())); // caller: null
```

- 함수 호출 bar()의 경우 bar함수를 호출한 함수는 없다.
- caller 프로퍼티는 null을 가리킨다.

### 18.2.3 length 프로퍼티

- 함수 객체의 `length` 프로퍼티는 함수를 정의할 때 선언한 매개변수의 개수를 가리킨다.
- `arguments` 객체의 `length` 프로퍼티와 함수 객체의 `length` 프로퍼티 값은 다를 수 있으므로 주의해야 한다.
- `arguments` 객체의 `length` 프로퍼티는 인자의 개수를 가르키고, 함수 객체의 `length` 프로퍼티는 매개변수의 개수를 가리킨다.

```jsx
function foo() {}
console.log(foo.length); // 0

function bar(x) {
  return x;
}
console.log(bar.lengh); // 1

function baz(x, y) {
  return x * y;
}
console.log(baz.length); // 2
```

### 18.2.4 name 프로퍼티

- `name` 프로퍼티는 **함수 이름**을 나타낸다.
- ES6 이전까지는 비표준이었다가 ES6에서 정식 표준이 되었다.
- `name` 프로퍼티는 ES5와 ES6에서 동작을 다르게 한다.
  - ES5 : 함수 표현식의 경우 name 프로퍼티를 빈 문자열 값을 가짐.
  - ES6 : 함수 객체를 가리키는 식별자 값으로 갖는다.

```jsx
// 기명 함수 표현식
var nameFunc = function foo() {};
console.log(nameFunc.name); // foo

// 익명함수 표현식
var anonymousFunc = function () {};
console.log(anonymousFunc.name); // ES5: "", ES6: anonoymousFunc

// 함수 선언문
function bar() {}
console.log(bar.name); // bar
```

### 18.2.5 **proto** 접근자 프로퍼티

- 모든 객체는 `[[Prototype]]` 이라는 내부 슬롯을 가진다. ⇒ 객체지향 프로그래밍의 상속을 구현하는 프로토타입 객체를 가르킨다. 19장!
- `__**proto__**` 프로퍼티는 `[[Prototype]]` 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티다.
- 내부 슬롯에 직접 접근할 수 없고, 간접적인 접근 방법을 제공하는 경우에 한하여 접근할 수 있다.

```jsx
const obj = { a: 1 };

console.log(obj.__proto__ === Object.prototype); // true

console.log(obj.hasOwnProperty('a')); // true
console.log(obj.hasOwnProperty('__proto__'); // false
```

**hasOwnProperty 메서드**

- `hasOwnProperty`메서드는 인수로 전달받은 프로퍼티 키가 객체 고유의 프로퍼티 키인 경우에만 반환하고 상속받은 프로토타입의 프로퍼티 키인 경우 `false`를 반환한다.

### 18.2.6 prototype 프로퍼티

- prototype 프로퍼티는 생성자 함수로 호출할 수 있는 함수 객체, constructor만 소유할 수 있는 프로퍼티이다.
- 일반 객체와 생성자 함수로 호출할 수 없는 non-constructor에는 prototype 프로퍼티가 없다.

```jsx
// 함수 객체는 prototype 프로퍼티를 가진다.
(function () {}.hasOwnProperty('prototype')); // true

// 일반 객체는 prototype 프로퍼티를 가지지 않는다.
({}.hasOwnProperty('prototype')); // false
```
