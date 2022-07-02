# 22. this

- this가 뭔가요? 🔥
- this 바인딩이란? 🔥
- this는 동적으로 바인딩이 된다고 하는데 바인딩되는 객체가 어떻게 다르나요?

## 22.1 this 키워드

- 객체는 **상태를 나타내는 프로퍼티**와 **동작을 나타내는 메서드**를 **하나의 논리적인 단위로 묶은 복합적인 자료구조**다.
- **메서드**는 **자신이 속한 객체의 상태를 변경**할 수 있어야 하여 **자신이 속한 객체를 가리키는 식별자를 참조**할 수 있어야 한다.
- **리터럴 방식의 객체**는 메서드에서 재귀적으로 참조할 수 있지만 일반적이지 않으며 **바람직하지 않은 방법**이다.

```jsx
const circle = {
  radius: 5,
  getDiameter() {
    return 2 * circle.radius; // 재귀
  },
};

console.log(circle.getDiameter());
```

- 자바스크립트는 **`this`**라는 특수한 **자기참조 변수**를 제공한다.
- `this`는 js 엔진에 의해 암묵적으로 생성되며 함수를 호출하면 arguments 객체와 this가 암묵적으로 함수 내부에 전달된다.
- 지역변수로 사용 가능하다.
- `this`가 가리키는 값인 `this`바인딩은 함수 호출 방식에 의해 동적으로 결정된다.

```jsx
const circle = {
  radius: 5,
  getDiameter() {
    return 2 * this.radius; // this는 메서드를 호출한 객체를 가리킨다.
  },
};

console.log(circle.getDiameter());
```

### this 바인딩

**바인딩**: 식별자와 값을 연결하는 과정을 의미한다. 변수선언은 변수 이름(식별자)과 확보된 메모리 공간의 주소를 바인딩하는 것이다. this 바인딩은 this(식별자 역할)와 this가 가르킬 객체를 바인딩 하는 것이다.

```jsx
// this는 어디서든지 참조 가능하다.
// 전역에서 this는 전역 객체 window를 가리킨다.
console.log(this); // window

function square(number) {
  // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
  console.log(this); // window
  return number * number;
}
square(2);

const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
    console.log(this); // {name: "Lee", getName: ƒ}
    return this.name;
  },
};
console.log(person.getName()); // Lee

function Person(name) {
  this.name = name;
  // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  console.log(this); // Person {name: "Lee"}
}

const me = new Person('Lee');
```

- `this`는 객체의 메서드 내부 또는 생성자 함수 내부에서만 의미가 있다.
- 일반 함수 내부의 `this`는 `window`가 바인딩되고, `strict mode`에서는 `undefined`가 바인딩된다. (일반 함수에서는 this를 사용할 필요가 없기 때문이다.)

## 22.2 함수 호출 방식과 this 바인딩

- **this 바인딩**은 함수 호출 방식, 함수가 **어떻게 호출**되었는지에 따라 **동적으로 결정**된다.
- **렉시컬 스코프**와 **this 바인딩**은 결정 시기가 다르다.
  ⇒ 함수의 상위 스코프를 결정하는 방식인 **랙시컬 스코프**는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프를 결정한다. 하지만 **this 바인딩**은 **함수 호출 시점**에 결정된다.
- **함수의 호출 방식**
  1. 일반함수 호출
  2. 메서드 호출
  3. 생성자 함수 호출
  4. Function.prototype.apply/call/bind 에 의한 간접호출

```jsx
// this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.
const foo = function () {
  console.dir(this);
};

// 동일한 함수도 다양한 방식으로 호출할 수 있다.

// 1. 일반 함수 호출
// foo 함수를 일반적인 방식으로 호출
// foo 함수 내부의 this는 전역 객체 window를 가리킨다.
foo(); // window

// 2. 메서드 호출
// foo 함수를 프로퍼티 값으로 할당하여 호출
// foo 함수 내부의 this는 메서드를 호출한 객체 obj를 가리킨다.
const obj = { foo };
obj.foo(); // obj

// 3. 생성자 함수 호출
// foo 함수를 new 연산자와 함께 생성자 함수로 호출
// foo 함수 내부의 this는 생성자 함수가 생성한 인스턴스를 가리킨다.
new foo(); // foo {}

// 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
// foo 함수 내부의 this는 인수에 의해 결정된다.
const bar = { name: 'bar' };

foo.call(bar); // bar
foo.apply(bar); // bar
foo.bind(bar)(); // bar
```

### 22.2.1 일반 함수 호출

- 일반 함수로 호출하면 기본적으로 `this`에는 전역 객체가 바인딩된다.

```jsx
function foo() {
  console.log("foo's this: ", this); // window
  function bar() {
    console.log("bar's this: ", this); // window
  }
  bar();
}
foo();
```

- `strict mode`에서는 `this`에 `undefined`가 바인딩된다.

```jsx
function foo() {
  'use strict';

  console.log("foo's this: ", this); // undefined
  function bar() {
    console.log("bar's this: ", this); // undefined
  }
  bar();
}
foo();
```

- 메서드 내의 중첩 함수라도 일반 함수로 호출되면 함수 내부의 `this`에는 전역 객체가 바인딩된다.

```jsx
// var 키워드로 선언한 전역 변수 value는 전역 객체의 프로퍼티다.
var value = 1;
// const 키워드로 선언한 전역 변수 value는 전역 객체의 프로퍼티가 아니다.
// const value = 1;

const obj = {
  value: 100,
  foo() {
    console.log("foo's this: ", this); // {value: 100, foo: ƒ}
    console.log("foo's this.value: ", this.value); // 100

    // 메서드 내에서 정의한 중첩 함수
    function bar() {
      console.log("bar's this: ", this); // window
      console.log("bar's this.value: ", this.value); // 1
    }

    // 메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 중첩 함수 내부의 this에는 전역 객체가 바인딩된다.
    bar();
  },
};

obj.foo();
```

- 콜백 함수도 일반 함수로 호출되면 `this`에 전역 객체가 바인딩된다.

```jsx
var value = 1;

const obj = {
  value: 100,
  foo() {
    console.log("foo's this: ", this); // {value: 100, foo: ƒ}
    // 콜백 함수 내부의 this에는 전역 객체가 바인딩된다.
    setTimeout(function () {
      console.log("callback's this: ", this); // window
      console.log("callback's this.value: ", this.value); // 1
    }, 100);
  },
};

obj.foo();
```

- 메서드 내부 중첩 함수나 콜백 함수에 `this`바인딩을 메서드의 `this`와 일치 시키기 위한 방법이 3가지가 있다.

**1) 변수 사용**

```jsx
var value = 1;

const obj = {
  value: 100,
  foo() {
    // this 바인딩(obj)을 변수 that에 할당한다.
    const that = this;

    // 콜백 함수 내부에서 this 대신 that을 참조한다.
    setTimeout(function () {
      console.log(that.value); // 100
    }, 100);
  },
};

obj.foo();
```

**2)** `Function.prototype.apply/call/bind`**메서드 활용**

```jsx
var value = 1;

const obj = {
  value: 100,
  foo() {
    // 콜백 함수에 명시적으로 this를 바인딩한다.
    setTimeout(
      function () {
        console.log(this.value); // 100
      }.bind(this),
      100
    );
  },
};

obj.foo();
```

**3) 화살표 함수 활용**

```jsx
var value = 1;

const obj = {
  value: 100,
  foo() {
    // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.
    setTimeout(() => console.log(this.value), 100); // 100
  },
};

obj.foo();
```

### 22.2.2 메서드 호출

- 메서드 내부의 `this`에는 메서드를 호출한 객체, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표`.` 연산자 앞에 기술한 객체가 바인딩된다.
- 주의할 것은 메서드 내부의 `this`는 메서드를 소유한 객체가 아닌 **메서드를 호출한 객체에 바인딩** 된다는 것이다.

```jsx
const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩된다.
    return this.name;
  },
};

// 메서드 getName을 호출한 객체는 person이다.
console.log(person.getName()); // Lee
```

### 22.2.3 생성자 함수 호출

### 22.2.4 Function.prototype.apply/call/bind 메서드에 의한 간접 호출
