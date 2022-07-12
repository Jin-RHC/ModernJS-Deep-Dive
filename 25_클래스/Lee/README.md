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

## 25.5 메서드

- 클래스 몸체에는 0개 이상의 메서드만 선언할 수 있다.
- 클래스 몸체에서 정의할 수 있는 메서드는 constructor(생성자), 프로토타입 메서드, 정적 메서드의 세 가지가 있다.

### 25.5.1 constructor

- **constructor**는 인스턴트를 생성하고 초기화하기 위한 특수한 메서드이며 이름을 변경할 수 없다.

```jsx
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }
}
```

- 클래스는 인스턴스를 생성하기 위한 **생성자 함수**

```jsx
// 클래스는 함수다
console.log(typeof Person); // function
console.dir(Person);
```

- console.dir(Person)을 크롬 브라우저의 개발자 도구에서 실행해보면 function과 똑같다는 것을 알 수 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5450574d-2bb5-45a3-9147-23007ba95f2e/Untitled.png)

- 클래스가 생성한 인스턴스 내부

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5448ebf5-fc25-42d7-b2f9-7504206c446b/Untitled.png)

- **constructor** 내부에서 this에 추가한 name 프로퍼티가 클래스가 생성한 인스턴스의 프로퍼티로 추가된 것을 확인할 수 있다.

```jsx
class Person {
  // 생성자
  constructor() {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }
}

// 생성자 함수
function Person(name) {
  // 인스턴스 생성 및 초기화
  this.name = name;
}
```

- **this**가 아닌 다른 객체를 명시적으로 반환하면 **return**문의 객체가 반환된다.

```jsx
class Person {
  // constructor는 생략하면 아래와 같이 빈 constructor가 암묵적으로 정의된다.
  constructor() {}
}

// 빈 객체가 생성된다.
const me = new Person();
console.log(me); // Person {}
```

- 명시적으로 원시값을 반환하면 원시값은 무시되고 암묵적으로 this가 반환된다.

```jsx
class Person {
  constructor(name) {
    this.name = name;

    // 명시적으로 원시값을 반환하면 원시값 반환은 무시되고 암묵적으로 this가 반환된다.
    return 100;
  }
}

const me = new Person('Lee');
console.log(me); // Person { name: "Lee" }
```

- 이처럼 constructor 내부에서 명시적으로 this가 아닌 다른 값을 반환하는 것은 클래스의 기본동작을 훼손한다. ⇒ constructor 내부에서 **return 문을 반드시 생략**해야 한다.

### 25.5.2 프로토타입 메서드

- 생성자 함수를 사용해서 인스턴스를 생성하는 경우, 프로토타입 메서드를 생성하기 위해서는 프로토타입 메서드를 명시적으로 추가해야한다.

```jsx
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHi = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person('Lee');
me.sayHi(); // Hi! My name is Lee
```

- 클래스 몸체에서 정의한 메서드는 prototype 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드가 된다.
- 클래스가 생성한 인스턴스는 프로토타입 체인의 일원이 된다.

```jsx
// 생성자 함수
class Person {
// 생성자
  function Person(name) {
    this.name = name;
  }

  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }
}

const me = new Person('Lee')
me.sayHi(); // Hi! My name is Lee
```

- Person클래스는 프로토타입 체인을 생성하여 프로토타입 메서드가 된다.
- 모든 객체 생성 방식뿐만아니라 클래스에 의해 생성된 인스턴스에도 동일하게 적용된다.

### 25.5.3 정적 메서드

> **19.12** **"정적 프로퍼티/메서드"**
> 정적(static) 메서드는 인스턴스를 생성하지 않아도 호출할 수 있는 메서드를 말한다

- 클래스에서는 메서드에 static 키워드를 붙이면 정적 메서드(클래스 메서드)가 된다.
- 정적 메서드는 인스턴스로 호출할 수 없다 ⇒ 정적 메서드가 바인딩된 클래스는 인스턴스의 프로토타입 체인상에 존재하지 않기 때문이다.

```jsx
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }

  // 정적 메서드
  static sayHi() {
    console.log('Hi!');
  }
}

// 정적 메서드는 클래스로 호출한다.
// 정적 메서드는 인스턴스 없이도 호출할 수 있다.
Person.sayHi(); // Hi!

// 인스턴스 생성
const me = new Person('Lee');
me.sayHi(); // 타입 에러
```

### 25.5.4 정적 메서드와 프로토타입 메서드의 차이

```
1. 정적 메서드와 프로토타입 메서드는 자신이 속해 있는 포로토타입 체인이 다른다.
2. 정적 메서드는 클래스로 호출하고 프로토타입 메서드는 인스턴스로 호출한다.
3. 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있다.
```

```jsx
class Square {
  // 정적 메서드
  static area(width, height) {
    return width * height;
  }
}

console.log(Square.area(10, 10)); // 100;
```

- 정적 메서드 area는 2개의 인수를 전달받아 면적을 계산하는데 인스턴스 프로퍼티를 참조하지 않는다.
- 만약 인스턴스 프로퍼티를 참조해야 한다면 프로토타입 메서드를 사용해야 한다.

```jsx
class Square {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  // 프로토타입 메서드
  area() {
    return this.width * this.height;
  }
}

const square = new Square(10, 10);
console.log(Square.area()); // 100;
```

- 메서드 내부의 this는 메서드를 소유한 객체가 아니라 메서드를 호출한 객체, 즉 메서드 이름 앞의 마침표(.) 연산자 앞에 기술한 객체에 바인딩된다.
- 위 예제의 경우 square 객체로 프로토타입 메서드 area를 호출했기 떄문에 area 내부의 this는 square 객체를 가리킨다.

### 25.5.5 클래스에서 정의한 메서드의 특징

```
1. function 키워드를 생략한 메서드 축약 표현을 사용한다.
2. 객체 리터럴과는 다르게 클래스에서 메서드를 정의할 때는 콤마가 필요 없다.
3. 암묵적으로 strict mode로 실행된다.
4. for...in 문이나 Object.keys 메서드 등으로 열거할 수 없다. 즉, 프로퍼티의 열거 가능 여부를 나타내며, 불리언 값을 갖는 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false다.
5. 내부 메서드 [[Construct]]를 갖지 않는 non-constructor다. 따라서 new 연산자와 함께 호출할 수 없다.
```

## 25.6 클래스의 인스턴스 생성과정

- new 연산자와 함께 클래스를 호출하면 생성자 함수와 마찬가지로 내부 매서드 [[Construct]]가 호출된다. (new 연산자 없이 호출 X!)
- 생성자 함수와 비슷하게 인스턴스가 생성된다.

### **생성 과정**

1. **인스턴스 생성과 this 바인딩**
   - `new`연산자와 함께 클래스를 호출하면 constructor의 내부 코드가 실행되기에 앞서 암묵적으로 빈 객체가 생성된다. 빈 객체 ⇒ 클래스가 생성한 인스턴스
   - 암묵적으로 생성된 빈 객체(인스턴스)는 this에 바인딩 된다.
   - **constructor** 내부의 `this`는 클래스가 생성한 인스턴스를 가리킨다.
2. **인스턴스 초기화**
   - **constructor**의 내부 코드가 실행되어 `this`에 바인딩되어 있는 **인스턴스를 초기화**한다.
   - `this`에 바인딩 되어있는 인스턴스에 프로퍼티를 추가하고 **constructor**가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티 값을 초기화한다.
3. **인스턴스 반환**
   - 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 `this`가 암묵적으로 반환된다.

```jsx
class Person {
  // 생성자
  constructor(name) {
    // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
    console.log(this); // Person {}
    console.log(Object.getPrototypeOf(this) === Person.prototype); // true

    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.name = name;

    // 3. 완성된 인스턴스가 바인딩 된 this가 암묵적으로 반환된다.
  }
}
```

## 25.7 프로퍼티

### 25.7.1 인스턴스 프로퍼티

- 인스턴스 프로퍼티는 constructor 내부에서 정의해야한다.

```jsx
class Person {
  constructor(name) {
    // 인스턴스 프로퍼티
    this.name = name; // name 프로퍼티는 public 하다.
  }
}

const me = new Person('Lee');

console.log(me.name); // Lee
```

- this에 인스턴스 프로퍼티를 추가한다.

### 25.7.2 접근자 프로퍼티

- **접근자 프로퍼티**는 **자체적으로는 값을 갖지 않고** 프로퍼티 **값**을 **읽거나 저장**할 때 사용하는 **접근자 함수로 구성된 프로퍼티**다.

- 객체 리터럴로 표현한 모습

```jsx
// 객체 릭터럴
const person = {
  firstName: 'Sanghyeon',
  lastName: 'Lee',

  // getter 함수
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },
  // setter 함수
  set fullName() {
    [this.fristName, this.lastName] = name.split(' ');
  }
};
```

- 클래스로 표현한 모습

```jsx
class Person {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  // getter 함수
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  }

  // setter 함수
  set fullName() {
    [this.fristName, this.lastName] = name.split(' ');
  }
}

const me = new Person('Sanghyeon', 'Lee');

// setter 함수 호출
me.fullName = 'Heegun Lee';

// getter 함수 호출
console.log(me.fullName);
```

-

### 25.7.3 클래스 필드 정의 제안

- 클래스 필드 정의 ⇒ 클래스가 생성할 인스턴스 프로퍼티를 가르키는 용어, 내부에서 변수처럼 사용가능
- 자바스크립트 클래스에서 인스턴스 프로퍼티를 선언하고 초기화하려면 반드시 constructor 내부에 this에 프로퍼티를 추가해야한다.
- 참조도 마찬가지로 this를 사용하여 참조해야 한다.
- 원래는 클래스 몸체에 메서드만 선언이 가능하고 클래스 필드를 선언하면 SyntaxError가 발생했지만, 최신 버전에서는 정상적으로 작동한다. ⇒ 새 표준인 ‘**class field declarations**’, 인스턴스 프로퍼티를 마치 클래스 기반 객체지향 언어의 클래스 필드처럼 정의 가능

```jsx
class Person {
  // 클래스 필드 정의
  name = 'Lee';
}

const me = new Person();
console.log(me); // Person {name: "Lee"}
```

- 몸체에서 클래스 필드를 정의하는 경우는 this에 클래스 필드를 바인딩해서는 안된다. ⇒ this는 클래스의 constructor와 메서드 내에서만 유효하다.

```jsx
class Person{
	this.name = ''; //SyntaxError
}
```

- 클래스 필드를 참조하는 경우에 반드시 this를 사용해야함.

```jsx
class Person {
  name = 'merry';

  constructor() {
    console.log(name); // ReferenceError
  }
}
```

- 클래스 필드에 초기값을 할당하지 않으면 undefined를 갖는다.

```jsx
class Person {
  name;
}

const me = new Person();
console.log(me); // Person {name: undefined}
```

- 인스턴스를 생성할 때 외부의 초기값으로 초기화 해야하면 constructor에서 클래스 필드를 초기화 해야한다.
- 함수는 일급객체이므로 함수를 클래스 필드에 할당 가능 ⇒ 클래스 필드를 통해서 메서드를 정의할 수 있다.

```jsx
class Person {
  name = 'Lee';

  getName = function () {
    return this.name;
  };
  // 화살표 함수로도 정의 가능
  // getName = () => this.name;
}

const me = new Person();
console.log(me); // Person {name: 'Lee', getName: f}
console.log(me.getNmae()); // Lee
```

- 클래스 필드에 함수를 할당하는 경우에는 프로토타입 메서드가 아닌 인스턴스 메서드가 된다. ⇒ 모든 클래스 필드는 인스턴스 프로퍼티가 되기 때문! ⇒ **클래스 필드에 함수를 할당하는 것은 권장 X**

### **인스턴스 프로퍼티를 정의하는 방식**

1. `constructor`**에서 인스턴스 프로퍼티를 정의** : 외부 초기값으로 클래스필드를 초기화 해야한다면!
2. **클래스 필드 정의** : 외부 초기값으로 클래스 필드를 초기화 할 필요 없다면 가능

### 25.7.4 private 필드 정의 제안

- 자바스크립트는 캡슐화를 완전하게 지원하지 않음
- 다른 클래스 기반 `private`, `public` 등 키워드를 지원하지 않고 언제나 `public` 으로 제공
- 하지만 현재 `private` 필드를 정의할 수 있는 새로운 표준 사양이 제안되어 있다.
- 최신 브라우저와 최신 `Node.js`에서 이미 구현되어 있다.

```jsx
class Person {
  // private 필드 정의
  #name = '';

  constructor(name) {
    // private 필드 참조
    this.name = #name;
  }
}

const me = new Person('merry');
console.log(me.#name); // SyntaxError
```

**Typescript** - `public`, `private`, `protected` 모두 지원하며 같은 의미로 사용가능하다.

| 접근 가능성                 | public | private |
| --------------------------- | ------ | ------- |
| 클래스 내부                 | O      | O       |
| 자식 클래스 내부            | O      | X       |
| 클래스 인스턴스를 통한 접근 | O      | X       |

- `private`필드는 클래스 내부에서만 참조할 수 있지만 접근자 프로퍼티를 통해서 간접적으로 접근은 가능하다.

```jsx
class Person {
  // private 필드 정의
  #name = '';

  constructor(name) {
    // private 필드 참조
    this.name = #name;
  }

  get name() {
    return this.#name;
  }
}

const me = new Person('merry');
console.log(me.name); // merry
```

- `private` 필드는 반드시 클래스 몸체에 정의해야 한다.
- `constructor`에서 정의하면 에러가 발생한다.

### 25.7.5 static 필드 정의 제안

- static 키워드를 사용하여 정적 메서드를 정의할 수 있었지만 정적 필드는 정의할 수 없다.
- 새로운 표준 사양인 **Static class features**가 나온 후 static public/private 필드가 구현되어 있다.

```jsx
class MyMath {
  // static public 필드 정의
  static PI = 22 / 7;

  // static private 필드 정의
  static #num = 10;

  // static 메서드
  static increment() {
    return ++MyMath.#num;
  }
}

console.log(MyMath.PI); // 3.142857142857143
console.log(MyMath.increment()); // 11
```
