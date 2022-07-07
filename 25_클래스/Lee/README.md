- 자바스크립트에서 클래스가 생기기 전에는 어떤 방식으로 객체지향 패턴을 구현했나요?
- 그럼 생성자 함수와 클래스는 어떤 차이가 있나요?
- 클래스 정의
- 클래스의 상속

## 25.1 클래스

- **자바스크립트** ⇒ **프로토 타입 기반 객체지향 언어** (클래스가 필요 없는 객체지향 프로그래밍 언어)
- **ES5**에서는 **생성자 함수**와 **프로토타입**을 통해 객체지향 언어의 상속 구현 가능

```jsx
// ES5 생성자 함수
var Person = (function () {
  // 생성자 함수
  function Person(name) {
    this.name = name;
  }

  // 프로토타입 메서드
  Person.prototype.sayHi = function () {
    console.log('Hi! My name is ' + this.name);
  };

  // 생성자 함수 반환
  return Person;
})();

// 인스턴스 생성
var me = new Person('Kyeom');
me.sayHi(); // Hi! My name is Kyeom
```

- ES6에서 클래스가 도입됨. 클래스 기반 프로그래밍 언어와 비슷하게 가능, But 함수로 제공되며 프로토타입 기반 패턴을 클래스 기반 패턴으로 사용할 수 있도록 해주는 [문법적 설탕](https://dkje.github.io/2020/09/02/SyntaxSugar/) 이다.

### 생성자 함수와 클래스의 동작의 차이점

```jsx
1. 클래스를 new 연산자 없이 호출하면 에러가 발생한다. 하지만 생성자 함수를 new 연산자 없이 호출하면 일반 함수로서 호출된다.

2. 클래스는 상속을 지원하는 extends와 super 키워드를 제공한다. 하지만 생성자 함수는 extends와 super 키워드를 지원하지 않는다.

3. 클래스는 호이스팅이 발생하지 않는 것처럼 동작한다. 하지만 함수 선언문으로 정의된 생성자 함수는 함수 호이스팅이, 함수 표현식으로 정의한 생성자 함수는 변수 호이스팅이 발생한다.

4. 클래스 내의 모든 코드에는 암묵적으로 strict mode가 지정되어 실행되며 strict mode를 해제할 수 없다. 하지만 생성자 함수는 암묵적으로 strict mode가 지정되지 않는다.

5. 클래스의 constructor, 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false다. 다시 말해, 열거되지 않는다.
```

- 클래스는 생성자 함수 기반 객체 생성 방식보다 견고하고 명료하다.
- 특히 클래스의 extends와 super 키워드는 상속 관계 구현을 더욱 간결하고 명료하게 한다.
- 따라서 클래스는 문법적 설탕보다 **새로운 객체 생성 메커니즘**으로 보는 것이 합당하다.

## 25.2 클래스 정의

- 클래스는 `class` 키워드를 사용하여 정의한다.
- 클래스 이름은 생성자 함수와 마찬가지로 파스칼 케이스를 사용하는 것이 일반적이다.

```jsx
// 클래스 선언문
class Person {}
```

- 표현식으로도 클래스 정의 가능
- 함수와 마찬가지로 이름을 가질 수도 있고, 가지지 않을 수도 있음

```jsx
// 익명 클래스 표현식
const Person = class {};
// 기명 클래스 표현식
const Person = class Myclass {};
```

- 클래스를 표현식으로 정의할 수 있다 ⇒ **클래스**는 **일급 객체**이다!
- **일급 객체**의 특징을 가짐.
  - **무명의 리터럴**로 **생성**할 수 있다.
  - **변수**나 **자료구조**에 **저장**할 수 있다.
  - **함수의 매개변수**에게 **전달**할 수 있다.
  - **함수의 반환값**으로 **사용**할 수 있다.

```jsx
// 클래스 선언문
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name; // name 프로퍼티는 public하다.
  }

  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }

  // 정적 메서드
  static sayHello() {
    console.log('Hello!');
  }
}

// 인스턴스 생성
const me = new Person('Kyeom');

// 인스턴스의 프로퍼티 참조
console.log(me.name); // Kyeom
// 프로토타입 메서드 호출
me.sayHi(); // Hi! My name is Kyeom
// 정적 메서드 호출
Person.sayHello(); // Hello!
```

### 클래스와 생성자 함수의 정의 방식 비교하기

1. 생성자 함수
1. 클래스

```jsx
var Person = (function () {
  // 1. 생성자 함수
  function Person(name) {
    this.name = name;
  }

  // 2. 생성자 함수의 프로토타입 메서드 정의
  Person.prototype.sayHi = function () {
    console.log('Hi' + this.name);
  };

  // 3. 생성자 함수의 정적 메서드 정의
  Person.sayHello = function () {
    console.log('Hello!');
  };

  // 생성자 함수 반환
  return Person;
})();
```

```jsx
class Person {
  // 1. 클래스의 생성자
  constructor(name) {
    this.name = name;
  }
  // 2. 클래스의 프로토타입 메서드 정의
  sayHi() {
    console.log(`Hi + ${this.name}`); // ES6 template literal
  }

  // 3. 클래스의 정적 메서드 정의
  static sayHello() {
    console.log('Hello!');
  }
}
```

## 25.3 클래스 호이스팅

```jsx
// 클래스 선언문
class Person {}

console.log(typeof Person); // function
```

- 클래스는 함수로 평가된다.
- 클래스 선언문으로 정의한 클래스는 함수 선언문과 같이 런타임 이전에 먼저 평가되어 함수 객체를 생성한다.
- 생성자 함수와 프로토타입이 함수 객체를 생성하는 시점에서 같이 생성되며 항상 둘은 쌍으로 존재한다.
- 클래스는 클래스 정의 이전에 참조할 수 없다.

```jsx
console.log(Person);
// ReferenceError: Cannot access 'Person' before initialization

// 클래스 선언문
class Person {}
```

```jsx
const Person = '';

{
  // 호이스팅이 발생하지 않는다면 ''이 출력되어야 한다.
  console.log(Person);
  // ReferenceError: Cannot access 'Person' before initialization

  // 클래스 선언문
  class Person {}
}
```

- 클래스는 `let`과 `const`키워드로 선언한 변수처럼 호이스팅 된다.
- **일시적 사각지대(TDZ)**에 빠져서 호이스팅이 일어나지 않는 것 처럼 동작한다.
- `var`, `let`, `const`, `function`, `function*`, `class` 키워드를 사용하여 선언된 **모든 식별자는 호이스팅**된다. **모든 선언문**은 **런타임 이전에 먼저 실행**되기 때문이다.

## 25.4 인스턴스 생성

- 클래스는 생성자 함수이며 **`new`** 연산자와 함께 호출되어 인스턴스를 생성한다.

```jsx
class Person {}

// 인스턴스 생성
const me = new Person();
console.log(me); // Person {}
```

- 함수는 **`new`**연산자의 사용 여부에 따라 일반 함수로 호출되거나 인스턴스 생성을 위한 생성자 함수로 호출되지만 클래스는 인스턴스를 생성하는 것이 유일한 존재 이유이므로 반드시 **`new`**연산자와 함께 호출해야 한다.

```jsx
class Person {}

// 클래스를 new 연산자 없이 호출하면 타입 에러가 발생한다.
const me = Person();
// TypeError: Class constructor Foo cannot be invoked without 'new'
```

- 클래스 표현식으로 정의된 클래스의 경우 다음 예제와 같이 클래스를 가리키는 식별자(Person)을 사용해 인스턴스를 생성하지 않고 기명 클래스 표현식의 클래스 이름(MyClass)을 사용해 인스턴스를 생성하면 에러가 발생한다.

```jsx
const Person = class MyClass {};

// 함수 표현식과 마찬가지로 클래스를 가리키는 식별자로 인스턴스를 생성해야 한다.
const me = new Person();

// 클래스 이름 MyClass는 함수와 동일하게 클래스 몸체 내부에서만 유효한 식별자다.
console.log(MyClass); // ReferenceError: MyClass is not defined

const you = new MyClass(); // ReferenceError: MyClass is not defined
```

- 이는 기명 함수 표현식과 마찬가지로 클래스 표현식에서 사용한 클래스 이름은 외부 코드에서 접근 불가능하기 때문이다.
