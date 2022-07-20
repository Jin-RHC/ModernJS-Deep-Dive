# 19. 프로토타입

Created: 2022년 6월 28일 오후 11:02
Tags: javascript

# 19.0. Intro

- `JavaScript`는 명령형, 함수형, 프로토 타입 기반 객체지향 프로그래밍을 지원하는 멀티 패러다임 프로그래밍 언어.
- 간혹 `public`, `private`, `protected`등 클래스 기반 객체 지향 언어에서 상속 캡슐화를 위해 제공하는 키워드 들이 없어 객체 지향 언어가 아니라고 오해하는 경우도 있다.
- 하지만 클래스 기반 객체지향 프로그래밍 언어보다 효율적이며 더 강력한 객체지향 프로그래밍 능력을 지니고 있는 **프로토타입 기반 객체 지향 언어**

> 💡 **클래스**

ES6에서 도입되었으나, 기존의 프로토타입 기반 객체 지향 모델에서 넘어간 것이 아니라, 클래스 처럼 사용할 수 있게 제공해 놓은 일종의 `syntatic sugar`(이기도 하고 아니기도 함). 사실 클래스도 함수이며, 프로토 타입 기반의 인스턴스를 생성한다는 면에서 동일하다.

하지만 정확히 동일하게 동작하지는 않는다. 클래스는 생성자 함수보다 엄격하며, 생성자 함수에서 제공하지 않는 기능도 제공.

따라서 새로운 객체 생성 매커니즘으로 보는 것이 좀 더 합당 (25장 클래스 참조)

>

`JavaScript`에서 원시값을 제외한 거의 모든 것 (함수, 배열, 정규 표현식 등)은 모두 객체인 만큼

**JS는 확실한 객체 지향 언어이다.**

# 19.1. 객체 지향 프로그래밍

> **❓ 객체 지향 프로그래밍이란**

명령어 혹은 함수의 나열인 전통적 명령형 프로그래밍 `imperative programming`의 절차지향적 관점에서 벗어나, 독립적 단위인 객체의 상호작용으로 프로그램을 표현하려는 프로그래밍 패러다임

실세계의 실체를 인식하려는 철학적 사고를 프로그래밍에 접목하려는 시도에서 시작

>

## 19.1.1. 속성과 추상화

- `속성`: 실체는 특징이나 성질을 나타내고, 구별하는 **속성**을 가짐
  - ex) 사람: 이름, 주소, 나이, 신장, 체중, 학력, 성격, 직업 등 다양한 속성을 가짐
- `추상화`: 다양한 속성 중 필요한 속성만 간추려 내어 표현하는 것
  - ex) 사람의 속성 중 이름과 주소에만 관심이 있을 때

## 19.1.2. 객체

```tsx
const person = {
  name: "Lee",
  address: "Seoul",
};

console.log(person); // {name: "Lee", address: "Seoul"}
```

이때 `person`은 이름과 주소 속성으로 표현된 객체이며, 다른 객체와 구별해서 인식할 수 있다. 객체는 이처럼 속성을 통해 다수의 값을 하나의 단위로 구성한 복합적인 자료구조라고 볼 수 있다.

**원의 예시**

```tsx
const circle = {
	radius: 5, // 반지름

	// 원의 지름: 2r
	getDiameter() {
		return 2 * this.radius;
	}

	// 원 둘레: 2πr
	getPerimeter() {
		return 2 * Math.PI + this.radius;
	}

	// 원의 넓이: πrr
	getArea() {
		return Math.PI * this.radius ** 2;
	}
};

console.log(circle);
// {radius: 5, getDiameter: ƒ, getPerimeter: ƒ, getArea: ƒ}

console.log(circle.getDiameter()); // 10
console.log(circle.getPerimeter()); // 31.4159...
console.log(circle.getArea()); // 78.5398...
```

- `radius`: 원의 **상태**를 나타내는 데이터
- `getDiameter`, `getPerimeter`, `getArea`: 지름, 둘레, 넓이를 구하는 **동작**

이처럼 객체지향 프로그래밍은 객체의 상태 `state`를 나타내는 데이터와 상태 데이터를 조작할 수 있는 동작 `behavior` 을 하나의 논리적인 단위로 묶어 생각한다.

> **따라서 객체는 상태 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조**라고 할 수 있다.

이 떄 객체의 상태 데이터를 `property`, 동작을 `method`라고 함

객체는 독립적으로 존재하며 다른 객체와 상호작용하는 관계성을 가진다.

# 19.2. 상속과 프로토타입

## 19.2.1. **상속 `inheritance` 이란**

객체 지향 프로그래밍의 핵심 개념으로,
어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것을 말한다.

`JavaScript`에서는 이 어떤 객체가 바로 `prototype`

이 프로토 타입을 기반으로 상속을 구현해 불필요한 중복을 제거한다.
중복을 제거하는 방법은 기존의 코드를 적극적으로 재사용하는 것.

코드 개발 비용을 현저하게 줄일 수 있는 잠재력이 있으므로 매우 중요

```tsx
function Circle(radius) {
  this.radius = radius;
  this.getArea = function () {
    return Math.PI * this.radius ** 2;
  };
}

// 반지름이 1인 객체 생성
const circle1 = new Circle(1);
// 반지름이 2인 객체 생성
const circle2 = new Circle(2);

// 동일한 동작을 하는 getArea 메서드를 중복 생성하고
// 모든 메서드가 중복 소유.
// 하나만 생성해서 공유하는 것이 바람직
console.log(circle1.getArea === circle2.getArea); // false

console.log(circle1.getArea()); // 3.14159265
console.log(circle2.getArea()); // 12.56637061
```

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fcd097753-0513-45d3-b5a4-d2f7ed61df5c%2FUntitled.png?table=block&id=37a15c87-9154-4532-92ea-747d95940f44&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=1420&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

**위 `Circle` 생성자 함수의 문제**

- `radius`는 다를 수 있지만, 항상 같은 기능을 수행하는 `getArea` 메서드를 중복 생성
- 또 모든 인스턴스가 개별적으로 소유
- 메모리의 불필요한 낭비가 발생
- 매 인스턴스마다 동일한 메서드 생성을 반복하므로 퍼포먼스에도 악영향

## 19.2.2. 프로토 타입 기반 상속 구현

```tsx
function Circle(radius) {
  this.radius = radius;
}

// Circle 생성자 함수가 생성한 모든 인스턴스가 getArea 메서드를
// 공유해서 사용할 수 있도록 프로토 타입에 추가한다.
// 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩 돼 있다.
Circle.prototype.getArea = function () {
  return Math.PI * this.radius ** 2;
};

// 인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

// Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체 역할을 하는
// 프로토타입 Circle.prototype으로부터 getArea 메서드를 상속 받는다.
// 즉, Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메서드를 공유한다.
console.log(circle1.getArea === circle2.getArea);

console.log(circle1.getArea());
console.log(circle2.getArea());
```

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F33332131-d108-4af3-be33-3acab0322d85%2FUntitled.png?table=block&id=b32a687c-ebca-4a29-8b02-7ff44f098857&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=1420&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fb149cc85-3dbd-434f-89b9-4b02881824dd%2FUntitled.png?table=block&id=afdb1388-04e5-4a93-860f-82eeea67e7c8&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

- `Circle` 생성자 함수가 생성한 모든 인스턴스는 자신의 프로토타입, 즉 상위 (부모) 객체 역할을 하는 `Circle.prototype`의 모든 프로퍼티 메서드를 상속 받는다.
- `getArea`메서드는 단 하나만 생성되어 프로토타입인 `Circle.prototype`의 메서드로 할당 돼 있다.
- 따라서 `Circle` 생성자 함수로 생성된 모든 인스턴스들은 동일한 `getArea`를 상속받아 사용할 수 있고, 인스턴스 자신의 고유값 `radius`만 따로 갖고 있으면 된다.

## 19.2.3. 상속의 장점 정리

- 별도의 구현 없이 상위 객체인 프로토 타입의 자산을 공유하여 사용 가능
- 즉, 코드의 양과 자산인 메모리 모두를 아낄 수 있다.

# 19.3. 프로토타입 객체

- 프로토타입 객체는 **상속**을 구현하기 위해 사용됨.
- 줄여서 프로토타입이므로 사실 프로토타입은 일종의 객체로 볼 수 있다.
- 어떤 객체의 부모 역할을 하는 객체
- 자신의 프로퍼티를 자식 객체에게 공유 프로퍼티로 제공
- 자식 객체는 프로토 타입 프로퍼티를 자신의 프로퍼티처럼 자유롭게 사용 가능

**이 프로토타입은 어디에 저장될까?**

- 모든 객체는 `[[Prototype]]` 이라는 내부 슬롯을 가진다
- 이 내부 슬롯의 값은 프로토타입 객체의 레퍼런스다.
- 여기에 저장되는 프로토타입은 객체 생성 방식마다 다름
  1. 객체 리터럴 `{}`로 생성된 경우
     1. `Object.prototype`
  2. 생성자 함수에 의해 생성된 객체의 프로토타입인 경우
     1. `Circle.prototype`에 바인딩 된 객체

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fbc783641-b356-4c09-b721-7c2bcc5d5e82%2FUntitled.png?table=block&id=0cf1153c-99c0-4def-90c2-faca01f94409&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

`[[Prototype]]`내부 슬롯에 직접 접근은 할 수 없지만, `__proto__` 프로퍼티를 통해 자신의 프로토타입, 즉 자신의 `[[Prototype]]` 내부 슬롯이 가리키는 프로토타입에 간접적으로 접근할 수 있다.

프로토 타입은 자신의 `constructor` 프로퍼티를 통해 생성자 함수에 접근할 수 있고, 생성자 함수는 자신의 `prototype` 프로퍼티를 통해 프로토타입에 접근할 수 있다.

## 19.3.1. `__proto__` 접근자 프로퍼티

모든 객체는 `__proto__` 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 `[[ProtoType]]` 내부 슬롯에 간접적으로 접근할 수 있다.

```tsx
const person = { name: "Baek" };
```

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F63ae22cb-ea02-486e-8f66-9c0940101e43%2FUntitled.png?table=block&id=587c94ca-0a5f-4f0c-a449-b52cd123d424&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Facc5d0f5-7f11-41cf-bde5-8c5020b6e98f%2FUntitled.png?table=block&id=37bc2396-5c8a-468d-8e4b-26ecdd3f3102&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

- 두번째 사진은 `person`의 프로토타입인 `Object.prototype`이다. `person.__proto__`를 크롬 브라우저가 콘솔에 표시해준 것.

### 19.3.1.1. `**__proto__`는 값에 직접 접근할 수 없는 접근자 프로퍼티\*\*

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Ff00e8a72-7648-489d-a714-949d0da455c7%2FUntitled.png?table=block&id=21bc0a2c-6fc7-41ce-93be-bb42602b7afa&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

- `__proto__`를 통해 값에 접근하면 `get __proto__` 호출
- `__proto__`를 통해 값을 할당하면 `set __proto__` 호출

```tsx
const obj = {};
const parent = { x: 1 };

// getter 함수인 get __proto__가 호출되어 obj 객체의 prototype을 취득
obj.__proto__;
// setter 함수인 set __proto__가 호출되어 obj 객체의 prototype을 교체
obj.__proto__ = parent;

console.log(obj.x);
```

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Ffb412fc5-30a9-482b-bf0b-705041ca7d3f%2FUntitled.png?table=block&id=15578656-0c68-4608-a870-6d07f8ab5369&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

### 19.3.1.2. **`__proto__` 접근자 프로퍼티는 상속을 통해 사용된다.**

- `__proto__` 접근자 프로퍼티는 객체가 직접 소유한 프로퍼티가 아니라 `Object.prototype`의 프로퍼티다. 모든 객체는 상속을 통해 `Object.prototype.__proto__` 접근자 프로퍼티를 사용할 수 있다.

```tsx
const person = { name: "Lee" };

// person 객체는 __proto__ 프로퍼티를 소유하지 않는다.
console.log(person.hasOwnProperty("__proto__"));

// __proto__ 프로퍼티는 모든 객체의 프로토타입 객체인 Object.prototype의 접근자 프로퍼티다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, "__proto__"));

// 모든 객체는 Object.prototype의 접근자 프로퍼티 __proto__를 상속받아 사용할 수 있다.
console.log({}.__proto__ == Object.prototype);
```

### 19.3.1.3. 왜 이렇게 하나요

- 상호 참조에 의해 프로토타입 체인이 생기는 것을 방지하기 위함

> ⛓️ 프로토타입 체인
> 모든 객체는 프로토타입의 계층 구조인 프로토타입 체인에 묶여있다. `javascript`엔진은 접근하고자하는 프로퍼티가 해당 객체에 없다면 `__proto__` 접근자가 가리키는 참조에 따라 자신의 부모 역할을 하는 프로토 타입을 하나하나 순차적으로 검색. 프로토타입 체인의 종점, 즉 모든 프로토타입의 프로토타입은 `Object.prototype`이며, 이 객체의 프로퍼티와 메서드는 모든 객체에게 상속된다. `19.7 프로토타입 체인`에서 자세히 설명

```tsx
const parent = {};
const child = {};

// child의 프로토타입을 parent로 설정
child.__proto__ = parent;
// parent의 프로토타입을 child로 설정
parent.__proto__ = child;
```

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F9478ddc8-ff08-4273-acc8-47ab3db8d680%2FUntitled.png?table=block&id=ff88535c-89f4-49b0-a6f9-cf5ad28ab41e&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

![서로가 자신의 프로토타입이 되는 비정상적인 프로토타입 체인](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fdc28ec46-3797-4cdc-96ab-bc1f180a0473%2FUntitled.png?table=block&id=074b0dea-4bb4-40fe-a59b-3d2d920f2f3c&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

서로가 자신의 프로토타입이 되는 비정상적인 프로토타입 체인

**이렇게 막아 놓은 이유**

- 프로토타입 체인은 단방향 링크드 리스트로 구현돼야 한다. 즉, 검색 방향이 한 방향으로만 흘러야 한다.
- 하지만 위 그림이 허용이 된다면, 프로토타입체인 종점이 존재하지 않기 때문에 프로토타입 체인에서 자신의 프로퍼티에 없는 프로퍼티를 검색할 때 **무한 루프에 빠진다.**
- 따라서 검증을 거쳐 프로퍼티를 교체하기 위해서 `__proto__` 접근자 프로퍼티를 통해 프로토 타입에 접근하고 교체하도록 구현

### 19.3.1.3. 근데 `__proto__` 이거 쓰지 마세요.

- 원래 `__proto__`는 비표준이었는데, 일부 브라우저에서 이를 지원하고 있었기 때문에 호환성 이슈를 고려해서 ES6에서 표준으로 채택
- 하지만 코드 내에서 `__proto__`를 사용하는 것은 그럼에도 불구하고 권장하지 않는다.
  - 왜? `__proto__` 접근자 프로퍼티를 모든 객체에서 사용할 수 있는 것은 아니기 때문.
  - `19.11. 절`에서 나오는 `직접 상속`을 통해 `Object.prototype`을 상속 받지 않는 객체를 생성할 수도 있기 때문에 `__proto__` 접근자 프로퍼티를 사용할 수 없는 경우가 있다.

```tsx
// obj는 프로토타입체인의 종점이다. 따라서 object.__proto__를 상속받을 수 없다.
const obj = Object.create(null);

// obj는 Object.__proto__를 상속받을 수 없다.
console.log(obj.__proto__); // undefined

// 따라서, __proto__보다 Object.getProtoTypeOf 메서드를 사용하는 편이 좋다.
console.log(Object.getPrototypeOf(obj)); // null
```

**그럼 대안은?**

- 참조를 취득하고 싶을 때는 `Object.getPrototypeOf` 메서드를 사용
- 교체하고 싶을 때는 `Object.setPrototypeOf` 메서드를 사용

```tsx
const obj = {};
const parent = { x: 1 };

// getter 함수인 get __proto__가 호출되어 obj 객체의 prototype을 취득
// obj.__proto__;
Object.getPrototypeOf(obj);

// setter 함수인 set __proto__가 호출되어 obj 객체의 prototype을 교체
// obj.__proto__ = parent;
Object.setPrototypeOf(obj, parent);

console.log(obj.x); // 1
```

각 메서드는 접근자 프로퍼티 `__proto__`와 정확히 같이 동작한다.

- `getPrototypeOf`: ES5+
- `setPrototypeOf`: ES6+

## 19.3.2. 함수 객체의 `prototype` 프로퍼티

- 함수 객체만이 소유하는 `prototype` 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.

```tsx
(function () {}.hasOwnProperty("prototype")); // true
({}.hasOwnProperty("prototype")); // false
```

- 함수 객체에만 있고 일반 객체에는 없다.
- 더 정확히는 생성자 함수로서 호출할 수 있는 함수 객체에만 있다.
- `arrow function`, ES6 메서드 축약 표현은 `non-constructor`로 생성자함수가 될수 없기 때문에 `prototype` 프로퍼티를 소유하지 않으며, 프로토타입도 생성하지 않는다.

```tsx
// 화살표 함수는 non-constructor
const Person = (name) => {
  this.name = name;
};

console.log(Person.hasOwnProperty("prototype")); // false
console.log(Person.prototype); // undefined

// ES6의 메서드 축약표현 또한 non-constructor
const obj = {
  foo() {},
};

console.log(obj.foo.hasOwnProperty("prototype")); // false
console.log(obj.foo.prototype); // undefined
```

- `function ~`으로 생성한 일반 함수 선언문도 `prototype` 을 가지긴 하는데, 얘네는 아무 의미가 없음 ( 객체를 생성하지 않기 때문인 듯 합니다.)
- **함수 객체의 `prototype`과 다른 일반 객체의 `__proto__`접근자 프로퍼티는 결국 동일한 프로토타입을 가리킨다.**

| 구분                     | 소유        | 값                 | 사용 주체   | 사용 목적                                                          |
| ------------------------ | ----------- | ------------------ | ----------- | ------------------------------------------------------------------ |
| **proto**접근자 프로퍼티 | 모든 객체   | 프로토 타입의 참조 | 모든 객체   | 객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용            |
| prototype 프로퍼티       | constructor | 프로토타입의 참조  | 생성자 함수 | 생성자 함수가 자신이 생성할 객체의 프로토타입을 할당하기 위해 사용 |

```tsx
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

console.log(Person.prototype === me.__proto__); // true
```

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F692ee30f-fd16-4678-b2d1-2aadbab4254f%2FUntitled.png?table=block&id=bd00d28a-5e66-4613-ac13-11bce5be2d8b&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

- new로 생성한 인스턴스도 생성자 함수 객체의 `prototype`과 같은 곳을 바라본다.

## 19.3.3. 프로토타입의 constructor 프로퍼티와 생성자 함수

- 모든 prototype은 `constructor` 프로퍼티를 갖는다.
- `constructor` 프로퍼티는 `prototype` 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다.
- 이 연결은 생성자 함수가 생성될 때 이뤄진다.

```tsx
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// me의 생성자 함수는 Person이다.
console.log(me.constructor === Person); // true
```

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F8c26410b-4f42-4353-ba44-479730493bd8%2FUntitled.png?table=block&id=6a73dbbf-f360-42ac-a24f-07931459c482&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

- 이 때 `Person` 생성자 함수는 `me` 객체를 생성. `me` 객체는 프로토 타입의 `constructor`를 통해 생성자함수와 연결된다. `me`에는 `constructor` 프로퍼티가 없지만, `prototype`에는 `constructor` 프로퍼티가 있다. 따라서 me 객체는 프로토 타입인 `Person.prototype`의 `constructor` 프로퍼티를 상속받아 사용할 수 있다.

# 19.4. 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

- 생성자 함수에 의해 생성된 인스턴스는 `constructor` 프로퍼티에 의해 생성자 함수와 연결된다.
- `constructor`는 생성자 함수

```tsx
// obj 객체를 생성한 생성자 함수는 Object이다.
const obj = new Object();
console.log(obj.constructor === Object); // true

// add 객체를 생성한 생성자 함수는 Function이다.
const add = new Function("a", "b", "return a + b");
console.log(add.constructor === Function); // true

// 생성자 함수
function Person(name) {
  this.name = name;
}

// me 객체를 생성한 생성자 함수는 Person이다.
const me = new Person("Lee");
console.log(me.constructor === Person); // true
```

- 하지만 `리터럴 표기법에 의한 객체 생성 방식`과 같이 명시적으로 `new` 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하지 않는 객체 생성 방식도 있다.

```tsx
// 객체 리터럴
const obj = {};

// 함수 리터럴
const add = function (a, b) {
  return a + b;
};

// 배열 리터럴
const arr = [1, 2, 3];

// 정규 표현식 리터럴
const regexp = /is/gi;
```

- 리터럴 표기법으로 생성된 객체도 프로토타입 존재
- 하지만 `constructor` 프로퍼티가 반드시 객체를 생성한 생성자 함수라고 단정할 수는 없다.

```tsx
// obj 객체는 Object 생성자 함수가 아닌 객체 리터럴로 생성
const obj = {};

// 하지만 obj 객체의 생성자 함수는 Object 생성자 함수다
console.log(obj.constructor === Object); // true
```

- 객체 리터럴로 생성했는데도 `constructor` 프로퍼티와 `Object` 생성자 함수가 연결.
- 그럼 객체 리터럴로 생성한 객체는 사실 `Object` 생성자 함수로 생성되도록 구현 된 것은 아닐까?

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F111ffddc-79ab-424c-ba4d-6cd2b2a870a1%2FUntitled.png?table=block&id=b02ee104-9207-4e0a-8760-fb49e210cfda&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

[https://262.ecma-international.org/10.0/#sec-object-value](https://262.ecma-international.org/10.0/#sec-object-value)

`2`를 보면, `Object` 생성자 함수에 인수를 전달하지 않거나 `undefined` 또는 `null`을 인수로 전달하면서 호출하면 내부적으로 추상 연산 `OrdinaryObjectCreate`를 호출하여 `Object.prototype`을 프로토 타입으로 갖는 빈 객체를 생성

> ❓ **추상 연산 abstract operation**
> 추상 연산은 ECMAScript 사양에서 내부 동작의 구현 알고리즘을 표현한 것. ECMAScript 사양에서 설명을 위해 사용되는 함수와 유사한 의사 코드라고 이해

```tsx
// 2. Object 생성자 함수에 의한 객체 생성
// 인수가 전달되지 않았을 때 추상 연산 OrdinaryObjectCreate를 호출하여 빈 객체를 생성한다.
let obj = new Object();
console.log(obj); // {}

// 1. new.target이 undefined나 Object가 아닌 경우
// 인스턴스 → Foo.prototype → Object.prototype 순으로 프로토타입 체인이 생성된다.
class Foo extends Object {}
new Foo(); // Foo {}

// 3. 인수가 전달된 경우에는 인수를 객체로 변환한다.
// Number 객체 생성
obj = new Object(123);
console.log(obj); // Number {123}

// String 객체 생성
obj = new Object("123");
console.log(obj); // String {'123'}
```

객체 리터럴이 평가될 때는 아래와 같이 추상연산 `OrdinaryObjectCreate` 를 호출하여 빈 객체를 생성하고 프로퍼티를 추가하도록 정의돼 있음

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F7f20cc36-d602-4197-a537-6f0919b8c9bc%2FUntitled.png?table=block&id=517705c6-96dc-4dd9-b518-a2266ace91f1&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

이처럼 `Object` 생성자 함수 호출과 객체 리터럴의 평가는 추상 연산 `OrdinaryObjectCreate`를 호출하여 빈 객체를 생성하는 점에서 동일하나 `new.target`의 확인이나 프로퍼티를 추가하는 처리 등 세부 내용은 다르다. 따라서 객체 리터럴에 의해 생성된 객체는 `Object` 생성자 함수가 생성한 객체가 아니다.

함수 객체의 경우 차이가 더 명확. `Function` 생성자 함수를 호출하여 생성한 함수는 렉시컬 스코프를 만들지 않고 전역 함수인 것처럼 스코프를 생성하며 클로저도 만들지 않음. 따라서 함수 선언문과 함수 표현식을 평가하여 함수 객체를 생성한 것은 `Function` 생성자 함수가 아니다. 하지만 `constructor` 프로퍼티를 통해 확인해 보면 `foo` 함수의 생성자 함수는 `Function` 생성자 함수다

```tsx
// foo 함수는 Function 생성자 함수로 생성한 함수 객체가 아니라 함수 선언문으로 생성했다.
function foo() {}

// 하지만 constructor 프로퍼티를 통해 확인해보면 함수 foo의 생성자 함수는 Function 생성자 함수다.
console.log(foo.constructor === Function); // true
```

리터럴 표기법에 의해 생성된 객체도 상속을 위해 프로토타입이 필요하다. 따라서 리터럴 표기법에 의해 생성된 객체도 가상적인 생성자 함수를 갖는다. 프로토타입은 생성자 함수와 더불어 생성되며 `prototype`, `constructor` 프로퍼티에 의해 연결돼 있기 때문이다. 다시 말해, **프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍`pair`으로 존재한다.**

### 19.4.1. 정리

리터럴 표기법(객체 리터럴, 함수 리터럴, 배열 리터럴, 정규 표현식 리터럴 등)에 의해 생성된 객체는 생성자 함수에 의해 생성된 객체는 아니다.

하지만 큰 틀에서 생각해 보면 리터럴 표기법으로 생성한 객체도 생성자 함수로 생성한 객체와 본질적인 면에서는 큰 차이가 없다.

예를 들어, 객체 리터럴`{}`에 의해 생성한 객체와 `Object` 생성자 함수에 의해 생성한 객체는 생성 과정에 미묘한 차이는 있지만 결국 객체로서 동일한 특성을 가짐. 함수 리터럴에 의해 생성한 함수와 `Function` 생성자 함수에 의해 생성한 함수는 생성과정과 스코프, 클로저 등의 차이가 있지만 결국 함수로서 동일한 특성을 갖는다.

따라서 프로토타입의 `constructor` 프로퍼티를 통해 연결돼 있는 생성자 함수를 리터럴 표기법으로 생성한 객체를 생성한 생성자 함수로 생각해도 큰 무리는 없다. 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토 타입은 다음과 같다.

| 리터럴 표기법      | 생성자 함수 | 프로토타입         |
| ------------------ | ----------- | ------------------ |
| 객체 리터럴        | Object      | Object.prototype   |
| 함수 리터럴        | Function    | Function.prototype |
| 배열 리터럴        | Array       | Array.prototype    |
| 정규 표현식 리터럴 | RegExp      | RegExp.prototype   |

# 19.5. 프로토타입의 생성 시점

위의 결론은 리터럴 표기법도 결국 생성자 함수와 연결. 객체 생성 방법은 리터럴 표기법 또는 생성자 함수에 의해 생성. 결국 모든 객체는 생성자 함수와 연결돼 있다.

> **`Object.create` 메서드와 클래스에 의한 객체 생성** > `Object.create` 메서드와 클래스로 객체를 생성하는 방법도 있다. 이 둘도 생성자 함수와 연결. 이에 대해서는 19.11.11절 `Object.create`에 의한 직접 상속과 25장 클래스에서 살펴봄

**프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.** 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재하기 때문!

- 생성자 함수의 종류
  1. 사용자가 직접 정의한 사용자 정의 생성자 함수
  2. 자바스크립트가 기본 제공하는 빌트인 생성자 함수

이 둘을 구분해서 생성 시점에 대해 살펴봄

## 19.5.1. 사용자 정의 생성자 함수와 프로토타입 생성 시점

`constructor` vs `non-constructor` 중 `constructor`로서 내부 메서드 `[[Construct]]`를 갖는 함수 객체, 즉 화살표 함수나 ES6의 메서드 축약 표현으로 정의하지 않고 일반 함수로 정의한 함수 객체는 `new` 연산자와 함께 생성자 함수로서 호출할 수 있다.

**생성자 함수로서 호출할 수 있는 함수, 즉 `constructor`는 함수 정의가 평가돼 함수 객체를 생성하는 시점에 프로토타입도 더불어서 생성된다.**

```tsx
// 함수 정의(constructor)가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.
console.log(Person.prototype); // {constructor: ƒ}

// 생성자 함수
function Person(name) {
  this.name = name;
}
```

생성자 함수로서 호출할 수 없는 함수, 즉 `non-constructor`는 프로토타입이 생성되지 않는다.

```tsx
// 화살표 함수는 non-constructor
const Person = (name) => {
  this.name = name;
};

// non-constructor는 prototype이 생성되지 않는다.
console.log(Person.prototype); // undefined
```

함수 선언문은 런타임 이전에 자바스크립트 엔진에 의해 먼저 실행. 따라서 함수 선언문으로 정의된 `Person` 생성자 함수는 어떤 코드보다 먼저 평가되어 함수 객체가 된다. 이때 프로토타입도 더불어 생성된다. 생성된 프로토타입은 `Person` 생성자 함수의 `prototype` 프로퍼티에 바인딩된다. 이때 이 프로토 타입을 설펴보면,

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F2a4995ab-b6a7-42c2-ac8a-0c676b4dd05b%2FUntitled.png?table=block&id=fd2db010-ad84-42d6-b993-343da6aeb748&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

오직 `constructor` 프로퍼티를 갖는 객체. 그리고 프로토타입도 객체이고, 모든 객체는 프로토타입을 가지므로 프로토타입도 프로토 타입을 가짐. 생성된 프로토타입의 프로토타입은 `Object.prototype`이다.

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F6e071b96-9084-4c93-9a2e-98e3867f9ca7%2FUntitled.png?table=block&id=a4eacabf-3396-426c-bf68-f3a47de1851b&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

빌트인 생성자 함수가 아닌 사용자 정의 생성자 함수는 자신이 평가돼 함수객체로 생성되는 시점에 프로토타입도 더불어 생성되며, 생성된 프로토타입의 프로토타입은 언제나 `Object.prototype`이다.

## 19.5.2. 빌트인 생성자 함수와 프로토타입 생성 시점

`Object`, `String`, `Number`, `Function`, `Array`, `RegExp`등과 같은 빌트인 생성자 함수도 일반 함수와 마찬가지로 빌트인 생성자 함수가 생성되는 시점에 프로토 타입이 생성.

이 빌트인 생성자 함수들은 전역 객체가 생성되는 시점에 생성된다. 생성된 프로토타입은 빌트인 생성자 함수의 `prototype` 프로퍼티에 바인딩 된다.

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fb624735f-6564-4b0b-8f5c-c96cf8403321%2FUntitled.png?table=block&id=298db996-e4ca-4f5c-b0df-7ca619f138f7&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

> 🤫  **전역 객체 `global object`**  
> 전역 객체는 코드가 실행되기 전 자바스크립트 엔진에 의해 생성되는 특수한 객체다. 전역 객체는 클라이언트 사이드 환경에서는 `window`, 서버사이드 (`Node.js`) 환경에서는 `global` 객체를 의미한다.
> 전역 객체는 표준 빌트인 객체들과 환경에 따른 호스트 객체(클라이언트 웹 api 또는 `Node.js`의 호스트 api), 그리고 `var` 키워드로 선언한 전역 변수와 전역 함수를 프로퍼티로 갖는다 `Math`, `Reflect`, `JSON`을 제외한 표준 빌트인 객체는 모두 생성자 함수다.

```tsx
// 전역 객체 window는 브라우저에 종속적이므로 아래 코드는 브라우저 환경에서 실행해야한다.
// 빌트인 객체인 Object는 전역 객체 window의 프로퍼티다.
window.Object === Object; // true
```

표준 빌트인 객체인 `Object`도 전역 객체의 프로퍼티이며, 전역 객체가 생성되는 시점에 생성된다. 전역 객체와 표준 빌트인 객체에 대해서는 21장 “빌트인 객체"에서 알아봄

### 정리

- 객체가 생성되기 이전에 생성자 함수와 프로토타입은 이미 객체화되어 존재.
- 이후 **생성자 함수 또는 리터럴 표기법으로 객체를 생성하면 프로토타입은 생성된 객체의 `[[Prototype]]` 내부 슬롯에 할당된다.**
- 이로써 생성된 객체는 프로토타입을 상속받는다.

# 19.6. 객체 생성 방식과 프로토타입의 결정

## 19.6.0. 다양한 객체 생성 방식들

- 객체 리터럴 `{}`
- `Object` 생성자 함수
- 생성자 함수
- `Object.create` 메서드
- `class` (ES6+)

앞서 설명한 것 처럼 각 방식마다 세부적인 객체 생성 방식의 차이는 있으나, 추상연산 `OrdinaryObjectCreate`에 의해 생성된다는 공통점이 있다.

`OrdinaryObjectCreate` 는 **필수적으로** 자신이 생성할 객체의 프로토타입을 인수로 전달 받는다. 그리고 객체에 추가할 프로퍼티는 옵션으로 전달할 수 있다.

추상연산 `OrdinaryObjectCreate`는 빈 객체를 생성한 후, 객체에 추가할 프로퍼티 목록이 인수로 전달된 경우, 프로퍼티를 객체에 추가한다. 그리고 인수로 전달받은 프로토타입을 자신이 생성한 객체의 `[[Prototype]]` 내부 슬롯에 할당한 다음, 생성한 객체를 반환한다.

즉, 프로토타입은 추상연산 `OrdinaryObjectCreate`에 전달되는 인수에 의해 결정된다. 이 인수는 객체가 생성되는 시점에 객체 생성 방식에 의해 좌우된다.

## 19.6.1. 객체 리터럴에 의해 생성된 객체의 프로토타입

JS엔진은 객체 리터럴을 평가해서 객체를 생성할 때 `OrdinaryObjectCreate`에 `Object.prototype`를 프로토타입으로 전달한다.

```tsx
const obj = { x: 1 };
```

위 객체 리터럴이 평가되면 추상 연산 `OrdinaryObjectCreate`에 의해 다음과 같이 `Object` 생성자 함수와 `Object.prototype`과 생성된 객체 사이에 연결이 만들어진다.

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F7adfb4d9-472c-4eb5-b4fe-71f48d6b2b32%2FUntitled.png?table=block&id=d1f38ad8-be1d-45c2-b761-39506f9b0118&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

이처럼 객체 리터럴에 의해 생성된 `obj` 객체는 `Object.prototype`을 프로토타입으로 갖게 되며, 이로써 `Object.prototype`을 상속받는다. `obj` 객체가 `hasOwnProperty` 등을 직접 소유하지는 않지만, 자신의 프로토타입은 `Object.prototype`의 `constructor` 프로퍼티와 나머지 프로퍼티들을 자신의 것처럼 자유롭게 사용할 수 있다. 이는 `obj`가 `Object.prototype`을 상속 받았기 때문

```tsx
const obj = { x: 1 };

// 객체 리터럴에 의해 생성된 obj 객체는 Object.prototype을 상속 받는다.
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty("x")); // true
```

## 19.6.2. `Object` 생성자 함수에 의해 생성된 객체의 프로토타입

`new Object()`를 인수 없이 호출하면 빈 객체 생성. 객체 리터럴과 마찬가지로 `OrdinaryObjectCreate`가 호출된다. 이 때 이 추상연산에 전달되는 프로토타입은 `Object.prototype` 이다.

```tsx
const obj = new Object();
obj.x = 1;
```

위 코드를 실행하면, `OrdinaryObjectCreate`에 의해 `Object` 생성자 함수와 `Object.prototype`에 연결관계가 생기게 된다. 결론적으로 객체리터럴에 의해 생성된 객체와 동일한 구조를 갖는다

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fb10e8bb9-7121-4b26-86ec-857bf6d5f027%2FUntitled.png?table=block&id=06caf6c9-c38b-4355-945e-f7cb88d6c0c6&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

이처럼 `Object` 생성자 함수에 의해 생성된 `obj` 객체는 `Object.prototype`을 프로토타입으로 갖게 되며, 이로써 `Object.prototype`을 상속 받는다.

```tsx
const obj = new Object();
obj.x = 1;

// Object 생성자 함수에 의해 생성된 obj 객체는 Object.prototype을 상속 받는다.
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty("x")); // true
```

### 19.6.2.1. 객체 리터럴에 의한 생성 방식과의 차이

- 둘은 프로퍼티를 추가하는 방식이 다르다.
- 객체 리터럴 방식은 객체 리터럴 내부에 프로퍼티를 추가하지만,
- `Object` 생성자 함수 방식은 일단 빈 객체를 생성한 이후 프로퍼티를 추가해야 한다.

## 19.6.3. 생성자 함수에 의해 생성된 객체의 프로토타입

`new` 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하면 다른 객체 생성 방식과 마찬가지로 `OrdinaryObjectCreate`가 호출된다. 이 때, `OrdinaryObjectCreate`에 전달되는 프로토타입은 생성자 함수의 `prototype` 프로퍼티에 바인딩 되어 있는 객체다. 즉, 생성자 함수에 의해 생성되는 객체의 프로토타입은 생성자 함수의 `prototype` 프로퍼티에 바인딩 된 객체다.

```tsx
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");
```

위 코드가 실행되면 추상 연산 `OrdinaryObjectCreate`에 의해 다음과 같이 생성자 함수와 생성자 함수의 `prototype` 프로퍼티에 바인딩 된 객체와 생성된 객체 사이에 연결이 만들어진다.

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F123e83f7-d94a-4cee-8ba4-2eba0f6b1535%2FUntitled.png?table=block&id=b7aaf1a1-756e-4ade-874b-2b322a04a25a&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

표준 빌트인 객체인 `Object` 생성자 함수와 더불어 생성된 프로토타입 `Object.prototype`은 다양한 빌트인 메서드(`hasOwnProperty`, `propertyIsEnumerable`등)를 갖고 있다. 하지만 사용자 정의 생성자 함수와 더불어 생성된 프로토 타입의 프로퍼티는 `constructor` 뿐이다.

프로토타입 `Person.prototype`에 프로퍼티를 추가하여 자식 객체가 상속 받을 수 있도록 구현해보면, 프로토타입은 객체이므로 일반 객체와 같이 프로토 타입에도 프로퍼티를 추가 / 삭제할 수 있다. 그리고 이렇게 추가 / 삭제된 프로퍼티는 프로토타입 체인에 즉각 반영된다.

```tsx
function Person(name) {
  this.name = name;
}

// 프로토타입의 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi, My name is ${this.name}`);
};

const me = new Person("Lee");
const you = new Person("Kim");

me.sayHello(); // Hi, My name is Lee
you.sayHello(); // Hi, My name is Kim
```

`Person` 생성자 함수를 통해 생성된 모든 객체는 프로토타입에 추가 된 `sayHello` 메서드를 상속 받아 자신의 메서드처럼 사용할 수 있다.

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fcb9f08aa-5ab9-483b-9aca-e0d652ebf12f%2FUntitled.png?table=block&id=1215befc-a226-4a34-a43b-47ad5d979094&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

# 19.7. 프로토타입 체인

```tsx
function Person(name) {
  this.name = name;
}

// 프로토타입의 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi, My name is ${this.name}`);
};

const me = new Person("Lee");

// hasOwnProperty는 Object.prototype의 메서드다.
console.log(me.hasOwnProperty("name")); // true
```

`Person` 생성자 함수에 의해 생성된 `me` 객체는 `Object.prototype` 의 메서드인 `hasOwnProperty`를 호출할 수 있다. 이는 `me` 객체가 `Person.prototype` 뿐만 아니라 `Object.prototype`도 상속 받았음을 의미.

```tsx
Object.getPrototypeOf(me) === Person.prototype; // true
```

`Person.prototype`의 프로토타입은 `Object.prototype`이다. 프로토타입의 프로토타입은 언제나 `Object.prototype`이다.

```tsx
Object.getPrototypeOf(Person.prototype) === Object.prototype; // true
```

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F6a8a277a-845c-4b00-932a-42e6a8ef6523%2FUntitled.png?table=block&id=1280bb37-30b0-4c2f-948d-d23b719ec59a&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

**자바스크립트는 객체의 프로퍼티와 메서드에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 `[[Prototype]]` 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다. 이를 프로토타입 체인이라 한다. 프로토 타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘이다.**

```tsx
// hasOwnProperty는 Object.prototype의 메서드다.
// me 객체는 프로토타입 체인을 따라 hasOwnProperty 메서드를 검색하여 사용한다.
me.hasOwnProperty("name"); // true
```

`me.hasOwnProperty('name')`과 같이 메서드를 호출하면 자바스크립트 엔진이 메서드와 프로퍼티를 검색하는 과정

1. `hasOwnProperty` 를 호출한 `me` 객체에서 먼저 검색. `me` 객체에는 `hasOwnProperty` 메서드가 없으므로, 프로토타입 체인을 따라, 다시 말해 `[[Prototype]]` 내부 슬롯에 바인딩 돼 있는 프로토타입인 `Person.prototype`으로 이동하여 해당 메서드를 다시 검색한다.
2. `Person.prototype`에도 해당 메서드가 없으므로 프로토타입 체인을 따라, 다시 말해 `[[Prototype]]` 내부 슬롯에 바인딩 돼 있는 `Object.prototype`으로 이동하여 `hasOwnProperty` 메서드를 검색
3. `Object.prototype`에는 `hasOwnProperty` 메서드가 존재한다. 자바스크립트 엔진은 `Object.prototype.hasOwnProperty` 메서드를 호출한다. 이 메서드의 `this`에는 `me` 객체가 바인딩 된다.

   ```tsx
   Object.prototype.hasOwnProperty.call(me, "name"); // true
   ```

> ❓ **call 메서드**  
> call 메서드는 `this`로 사용할 객체를 전달하면서 함수를 호출한다. 이에 대해서는 22.2.4절 `Function.prototype.apply/call/bind` 메서드에 의한 간접 호출에서 자세히 살펴봄. 지금은 `this`로 사용할 `me` 객체를 전달하면서 `Object.prototype.hasOwnProperty` 메서드를 호출한다고 이해

## 19.7.1. 프로토타입의 최상위 객체

- 프로토 타입의 최상위에 위치하는 객체는 언제나 `Object.prototype`
- 따라서 모든 객체는 `Object.prototype`을 상속 받음
- `**Object.prototype`을 프로토타입 체인의 종점이라 이른다.\*\*

그리고 `Object.prototype`의 프로토타입, 즉 `[[Prototype]]` 내부 슬롯의 값은 `null` 이다.

종점에서도 프로퍼티를 검색할 수 없는 경우 `undefined`를 반환한다. 이 때 에러가 발생하지 않는 점에 유의

```tsx
console.log(me.foo); // undefined
```

## 19.7.2. 프로토타입 체인과 스코프 체인

- **프로토타입 체인은 상속과 프로퍼티 검색을 위한 메커니즘**
- **스코프 체인은 식별자 검색을 위한 메커니즘**

```tsx
me.hasOwnProperty("name");
```

이 코드의 경우 먼저, 스코프 체인에서 `me` 식별자를 검색. `me` 식별자는 전역에서 선언 되었으므로 전역 스코프에서 검색. `me` 식별자를 검색한 다음, `me` 객체의 프로토타입 체인에서 `hasOwnProperty` 메서드를 검색한다.

> **이처럼 스코프 체인과 프로토타입 체인은 서로 연관 없이 별도로 동작하는 것이 아니라 서로 협력하여 식별자와 프로퍼티를 검색하는 데 사용된다.**

# 19.8. 오버라이딩과 프로퍼티 섀도잉

```tsx
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  // 프로토타입의 메서드
  Person.prototype.sayHello = function () {
    console.log(`Hi, My name is ${this.name}`);
  };

  // 생성자 함수를 반환
  return Person;
})();

const me = new Person("Lee");

// 인스턴스의 메서드
me.sayHello = function () {
  console.log(`Hey, My name is ${this.name}`);
};

// 인스턴스 메서드가 호출된다. 프로토타입 메서드는 인스턴스 메서드에 의해 가려진다.
me.sayHello(); // Hey, My name is Lee
```

생성자 함수로 객체를 생성한 다음, 인스턴스에 메서드를 추가했다. 이를 그림으로 나타내면,

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F47f08048-f54e-4c16-9142-ac6afa99d7f4%2FUntitled.png?table=block&id=35c205d6-5919-468f-a046-be18760487c6&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

- 프로토타입이 소유한 프로퍼티는 프로토타입 프로퍼티, 인스턴스가 소유한 프로퍼티는 인스턴스 프로퍼티
- 프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면, 프로토타입 체인을 따라 프로토타입 프로퍼티를 검색해서 프로토타입 프로퍼티를 덮어쓰는 것이 아니라, 인스턴스 프로퍼티로 추가한다.
- 이 때 인스턴스 메스드 `sayHello`는 프로토타입 메서드 `sayHello`를 오버라이딩 했고 프로토타입 메서드 `sayHello`는 가려진다.
- 이처럼 상속관계에 의해 프로퍼티가 가려지는 현상을 **프로퍼티 섀도잉 `property shadowing`이라 한다.**

> **오버라이딩 `overriding`**  
> 상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용하는 방식

> **오버로딩 `overloading`**  
> 함수 이름은 동일하지만 매개변수의 타입 또는 개수가 다른 메서드를 구현하고 매개변수에 의해 메서드를 구별하여 호출하는 방식. 자바스크립트는 오버로딩을 지원하지 않지만 `arguments` 객체를 사용해 구현할 수는 있다.

프로퍼티를 삭제하는 경우도 마찬가지.

```tsx
// 인스턴스 메서드 삭제
delete me.sayHello;
// 인스턴스에는 sayHello 메서드가 없으므로 프로토타입 메서드가 호출.
me.sayHello(); // Hi, My name is Lee
```

프로토타입 메서드가 아닌 인스턴스 메서드 `sayHello`가 삭제 됨.
다시 한 번 `delete` 를 호출해서 프로토타입 메서드 삭제 시도

```tsx
// 프로토타입 체인을 통해 프로토타입 메서드 삭제 시도
delete me.sayHello;
// 프로토타입 메서드가 호출.
me.sayHello(); // Hi, My name is Lee
```

하위 객체를 통해 프로토타입의 프로퍼티를 변경 또는 삭제하는 것은 불가능.
하위 객체를 통해 `get` 액세스는 허용되나, `set` 액세스는 허용되지 않음.

프로토타입의 프로퍼티를 변경 또는 삭제하려면 하위 객체를 통해 접근하는 것이 아니라, 프로토 타입에 직접 접근해야 한다.

```tsx
// 프로토타입 메서드 변경
Person.prototype.sayHello = function () {
	console.log(`Hey, My name is ${this.name})
};
me.sayHello(); // Hey, My name is Lee

// 프로토타입 메서드 삭제
delete Person.prototype.sayHello
me.sayHello(); // TypeError: me.sayHello is not a function
```

# 19.9 프로토타입의 교체

프로토타입은 임의의 다른 객체로 변경할 수 있음. 부모 객체인 프로토타입을 동적으로 변경할 수 있다는 것을 의미. 이러한 특징을 활용해 객체 간의 상속 관계를 동적으로 변경할 수 있다. 프로토타입은 생성자 함수 또는 인스턴스에 의해 교체할 수 있다.

## 19.9.1. 생성자 함수에 의한 프로토타입의 교체

```tsx
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  // 1. 생성자 함수의 prototype 프로퍼티를 통해 프로토타입 교체
  Person.prototype = {
    sayHello() {
      console.log(`Hi, My name is ${this.name}`);
    },
  };

  // 생성자 함수를 반환
  return Person;
})();

const me = new Person("Lee");
```

1에서 `Person.prototype`에 객체 리터럴을 할당했다. 이는 `Person` 생성자 함수가 생성할 객체의 프로토타입을 객체 리터럴로 교체한 것이다. 이를 그림으로 나타내면,

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F1d37b885-82d3-4d84-b289-d7b46b82c612%2FUntitled.png?table=block&id=57b86277-ab27-4239-b2fa-eca0a1e3faf1&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

프로토타입으로 교체한 객체 리터럴에는 `constructor` 프로퍼티가 없다. `constructor` 프로퍼티는 자바스크립트 엔진이 프로토타입을 생성할 때 암묵적으로 추가한 프로퍼티다. 따라서 `me` 객체의 생성자 함수를 검색하면 `Person`이 아닌 `Object`가 나온다.

```tsx
// 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.
console.log(me.constructor === Person); // false
// 프로토타입 체인을 따라 Object.prototype의 constructor 프로퍼티가 검색된다.
console.log(me.constructor === Object); // true
```

이처럼 프로토타입을 교체하면 `constructor` 프로퍼티와 생성자 함수 간의 연결이 파괴된다. 이 연결을 되살릴 수도 있다.

```tsx
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  // 생성자 함수의 prototype 프로퍼티를 통해 프로토타입 교체
  Person.prototype = {
    // constructor 프로퍼티와 생성자 함수 간의 연결을 설정
    constructor: Person,
    sayHello() {
      console.log(`Hi, My name is ${this.name}`);
    },
  };

  return Person;
})();

const me = new Person("Lee");

// constructor 프로퍼티가 생성자 함수를 가리킨다.
console.log(me.constructor === Person); // true
console.log(me.constructor === Object); // false
```

## 19.9.2. 인스턴스에 의한 프로토타입 교체

프로토타입은 생성자 함수의 `prototype` 프로퍼티 뿐만 아니라 인스턴스의 `__proto__` 접근자 프로퍼티(혹은 `Object.getPrototypeOf` 메서드)를 통해 접근할 수 있다. 따라서 인스턴스의 `__proto__` 접근자 프로퍼티(혹은 `Object.setPrototypeOf` 메서드)를 통해 프로토타입을 교체할 수 있다.

생성자 함수의 `prototype` 프로퍼티에 다른 임의의 객체를 바인딩하는 것은 **미래에 생성할 인스턴스**의 프로토타입을 교체하는 것이다. `__proto__` 접근자 프로퍼티를 통해 프로토타입을 교체하는 것은 **이미 생성된 객체**의 프로토타입을 교체하는 것이다.

```tsx
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// 프로토타입으로 교체할 객체
const parent = {
  sayHello() {
    console.log(`Hi, My name is ${this.name}`);
  },
};

// 1. me 객체의 프로토타입을 parent 객체로 교체한다.
Object.setPrototypeOf(me, parent);
// 위 코드는 아래와 동일하게 동작하는 코드
me.__proto__ = parent;

me.sayHello(); // Hi, My name is Lee
```

1에서 `me` 객체의 프로토타입을 `parent` 객체로 교체했다. 이를 그림으로 나타내면,

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F384d7a61-9741-46c0-b7c1-6f6a5fc732c9%2FUntitled.png?table=block&id=93ae3442-08df-4708-b0b7-40788005831e&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

생성자 함수에 의한 프로토타입의 교체와 마찬가지로 프로토타입으로 교체한 객체에는 `constructor` 프로퍼티가 없으므로 `constructor` 프로퍼티와 생성자 함수 간의 연결이 파괴된다. 따라서 프로로타입의 `constructor` 프로퍼티로 `me` 객체의 생성자 함수를 검색하면 `Person`이 아닌 `Object`가 나온다.

```tsx
// 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.
console.log(me.constructor === Person); // false
// 프로토타입 체인을 따라 Object.prototype의 constructor 프로퍼티가 검색된다.
console.log(me.constructor === Object); // true
```

생성자 함수에 의한 프로토타입의 교체와 인스턴스에 의한 프로토타입 교체는 큰 차이가 없어보이지만, 미묘한 차이가 있다.

- 생성자 함수를 통해 교체한 경우
  - `Person` 생성자 함수의 `prototype` 프로퍼티가 교체된 프로토타입을 가리킨다.
- 인스턴스를 통해 교체한 경우
  - `Person` 생성자 함수의 `prototype` 프로퍼티가 교체된 프로토타입을 가리키지 않는다.

프로토타입으로 교체한 객체 리터럴에 `constructor` 프로퍼티를 추가하고, 생성자 함수의 `prototype` 프로퍼티를 재설정하여 파괴된 생성자 함수와 프로토타입 간의 연결을 되살려 보면,

```tsx
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// 프로토타입으로 교체할 객체
const parent = {
  // constructor 프로퍼티와 생성자 함수 간의 연결을 설정
  constructor: Person,
  sayHello() {
    console.log(`Hi, My name is ${this.name}`);
  },
};

// 생성자 함수의 prototype 프로퍼티와 프로토타입 간의 연결을 성정
Person.parent = parent;

// me 객체의 프로토타입을 parent 객체로 교체
Object.setPrototypeOf(me, parent);
// 위 코드는 아래와 동일하게 동작하는 코드
me.__proto__ = parent;

me.sayHello(); // Hi, My name is Lee

// constructor 프로퍼티가 생성자 함수를 가리킨다.
console.log(me.constructor === Person); // true
console.log(me.constructor === Object); // false

// 생성자 함수의 prototype 프로퍼티가 교체된 프로토타입을 가리킨다.
console.log(Person.prototype === Object.getPrototypeOf(me)); // true
```

이처럼 프로토타입 교체를 통해 객체간 상속 관계를 동적으로 변경하는 것은 상당히 번거로움. 따라서 프로토타입은 직접 교체하지 않는 것이 좋다. 상속 관계를 인위적으로 설정하려면, 19.11절 직접 상속에서 살펴볼 직접 상속이 더 편리하고 안전하다. 또는 ES6에서 도입된 클래스를 사용하면 간편하고 직관적으로 상속 관계를 구현할 수 있다. 이에 대해서는 25장 클래스에서 살펴본다.

# 19.10. `instanceof` 연산자

`instanceof` 연산자는 이항 연산자로서 좌변에 인스턴스 식별자, 우변에 생성자 함수 식별자를 피연산자로 가짐. 만약 우변의 피연산자가 함수가 아닌 경우 `TypeError`

```tsx
객체 instanceof 생성자함수;
```

우변의 생성자 함수의 `prototype`에 바인딩 된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 `true`로 평가되고, 그렇지 않은 경우에는 `false`로 평가된다.

```tsx
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// Person.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가
console.log(me instanceof Person); // true
// Object.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가
console.log(me instanceof Object); // true
```

`instanceof` 연산자의 작동 방식을 알아보기 위해 프로토타입을 교체

```tsx
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// 프로토타입으로 교체할 객체
const parent = {};

// 프로토타입 교체
Object.setPrototypeOf(me, parent);

// Person 생성자 함수와 parent 객체는 연결돼 있지 않다.
console.log(Person.prototype === parent); // false
console.log(parent.constructor === Person); // false

// Person.prototype이 me 객체의 프로토타입 체인 상에 존재하지 않기 때문에 false로 평가
console.log(me instanceof Person); // false
// Object.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가
console.log(me instanceof Object); // true
```

`me` 객체는 비록 프로토타입이 교체돼 프로토타입과 생성자 함수의 연결이 파괴됐지만, `Person` 생성자 함수에 의해 생성된 인스턴스임에는 틀림이 없음. 그러나 `me instanceof Person`은 `false`로 평가

`Person.prototype`이 `me` 객체의 프로토타입 체인 상에 존재하지 않기 때문. 따라서 프로토타입으로 교체한 `parent` 객체를 `Person` 생성자 함수의 `prototype` 프로퍼티에 바인딩하면 `me instanceof Person`은 `true`로 평가될 것.

```tsx
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// 프로토타입으로 교체할 객체
const parent = {};

// 프로토타입 교체
Object.setPrototypeOf(me, parent);

// Person 생성자 함수와 parent 객체는 연결돼 있지 않다.
console.log(Person.prototype === parent); // false
console.log(parent.constructor === Person); // false

// parent 객체를 Person 생성자 함수의 prototype 프로퍼티에 바인딩
Person.prototype = parent;

// Person.prototype이 me 객체의 프로토타입 체인 상에 존재하지 않기 때문에 false로 평가
console.log(me instanceof Person); // true
// Object.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가
console.log(me instanceof Object); // true
```

이처럼 `instanceof` 연산자는 프로토타입의 `constructor` 프로퍼티가 기리키는 생성자 함수를 찾는 것이 아니라, 생성자 함수의 `prototype`에 바인딩 된 객체가 프로토타입 체인 상에 존재하는지 확인한다.

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F40444e25-0c48-4f4c-ac49-f86d4fef0b25%2FUntitled.png?table=block&id=2da13dd9-36c4-4902-84d1-5ef9369ca479&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

`me instanceof Person`의 경우 `me` 객체의 프로토타입 체인 상에 `Person.prototype`에 바인딩된 객체가 존재하는 지 확인

`me instanceof Object`의 경우 `me` 객체의 프로토타입 체인 상에 `Object.prototype`에 바인딩된 객체가 존재하는지 확인.

`instanceof`를 함수료 표현하면 다음과 같다.

```tsx
function isInstanceof(instance, constructor) {
	// 프로토타입 취득
	const prototype = Object.getPrototypeOf(instance);

	// 재귀 탈출 조건
	// prototype이 null이면 프로토타입 체인의 종점에 다다른 것으로 종료.
	if (prototype === null) return false;

	// 프로토타입이 생성자 함수의 prototype에 바인딩 된 객체라면 true를 반환한다.
	// 그렇지 않다면 재귀 호출로 프로토타입 체인 상의 상위 프로토타입으로 이동하여 확인한다.
	return prototype === constructor.prototype || isInstanceOf(prototype, constructor);

console.log(isInstanceOf(me, Person)); // true
console.log(isInstanceOf(me, Object)); // true
console.log(isInstanceOf(me, Array)); // false
```

따라서 생성자 함수에 의해 프로토타입이 교체되어 `constructor` 프로퍼티와 생성자 함수 간의 연결이 파괴되어도, 생성자 함수의 `prototype` 프로퍼티와 프로토타입 간의 연결은 파괴되지 않으므로 `instanceof`는 아무런 영향을 받지 않는다.

```tsx
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  // 생성자 함수의 prototype 프로퍼티를 통해 프로토타입 교체
  Person.prototype = {
    sayHello() {
      console.log(`Hi, My name is ${this.name}`);
    },
  };

  return Person;
})();

const me = new Person("Lee");

// constructor 프로퍼티와 생성자 함수 간의 연결이 파괴돼도 instanceof는 아무런 영향을 받지 않는다.
console.log(me.constructor === Person); // false

// Person.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가된다.
console.log(me instanceof Person); // true
// Object.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가된다.
console.log(me instanceof Object); // true
```

# 19.11. 직접 상속

## 19.11.1. `Object.create`에 의한 직접 상속

`Object.create` 메서드는 명시적으로 프로토타입을 지정하여 새로운 객체를 생성한다. `Object.create` 메서드도 다른 객체 생성 방식과 마찬가지로 추상 연산 `OrdinaryObjectCreate`를 호출한다.

`Object.create` 메서드의 첫번째 매개변수에는 생성할 객체의 프로토타입으로 지정할 객체를 전달한다.

두번째 매개변수에는 생성할 객체의 프로퍼티 키와 프로퍼티 디스크립터 객체로 이뤄진 객체를 전달한다. 이 객체의 형식은 `Object.defineProperties` 메서드의 두 번째 인수와 동일하다. 두 번째 인수는 옵션이므로 생략 가능하다.

```tsx
/*
	지정된 프로토타입 및 프로퍼티를 갖는 새로운 객체를 생성하여 반환
	@param {Object} prototype - 생성할 객체의 프로토타입으로 지정할 객체
	@param {Object} propertiesObject - 생성할 객체의 프로퍼티를 갖는 객체
	@returns {Object} 지정된 프로토타입 및 프로퍼티를 갖는 새로운 객체
*/
Object.create(prototype[, propertiesObject]);
```

```tsx
// 프로토타입이 null인 객체를 생성한다. 생성된 객체는 프로토타입 체인의 종점에 위치한다.
// obj → null
let obj = Object.create(null);
console.log(Object.getPrototypeOf(obj) === null); // true
// Object.prototype을 상속받지는 못한다.
console.log(obj.toString()); // TypeError: obj.toString is not a function

// obj → Object.prototype → null
// obj = {}; 와 동일하다.
obj = Obj.create(Object.prototype);
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

// obj → Object.prototype → null
// obj = { x: 1 }; 와 동일하다.
obj = Obj.create(Object.prototype, {
  x: { value: 1, writable: true, enumarable: true, configurable: true },
});
// 위 코드는 아래와 동일하다.
// obj = Obj.create(Object.prototype);
// obj.x = 1
console.log(obj.x); // 1
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

// 임의의 객체
const myProto = { x: 10 };
// 임의의 객체 직접 상속
obj = Object.create(myProto);
console.log(obj.x); // 10
console.log(Object.getPrototypeOf(obj) === myProto); // true

// 생성자 함수
function Person(name) {
  this.name = name;
}

// obj → Person.prototype → Object.prototype → null
// obj = new Person('Lee')와 동일
obj = Object.create(Person.prototype);

obj.name = "Lee";
console.log(obj.name); // Lee
console.log(Object.getPrototypeOf(obj) === Person.prototype); // true
```

정리하면, `Object.create` 메서드는 첫 번째 매개변수에 전달한 객체의 프로토타입 체인에 속하는 객체를 생성한다. 즉, 객체를 생성하면서 직접적으로 상속을 구현하는 것이다.

### 19.11.1.1. `Object.create`의 장점

- `new` 연산자가 없이도 객체를 생성할 수 있다.
- 프로토타입을 지정하면서 객체를 생성할 수 있다.
- 객체 리터럴에 의해 생성된 객체도 상속받을 수 있다.

### 19.11.1.2. `Object.prototype`의 빌트인 메서드의 사용

참고로 `Object.prototype`의 빌트인 메서드인 `Object.prototype.hasOwnproperty`, `Object.prototype.isPrototypeOf`, `Object.prototype.propertyIsEnumerable` 등은 모든 객체의 프로토타입 체인의 종점, 즉 `Object.prototype`의 메서드이므로 모든 객체가 상속받아 호출할 수 있다.

```tsx
const obj = { a: 1 };

obj.hasOwnProperty("a"); // true
obj.propertyUsEnumerable("a"); // true
```

그런데 이렇게 직접 `Object.prototype`의 빌트인 메서드를 객체에서 직접 호출하는 것은 `ESLint` 에서는 권장하지 않음

`Object.create`를 통해 프로토타입 체인 종점에 위치하는 객체를 생성할 수 있기 때문

```tsx
const obj = Object.create(null);
obj.a = 1;

console.log(Object.getPrototypeOf(obj) === null); // true

// obj는 Object.prototype의 빌트인 메서드를 사용할 수 없다.
console.log(obj.hasOwnProperty("a"));
// TypeError: obj.hasOwnProperty is not a function
```

따라서 이 같은 에러를 발생시킬 위험을 줄이기 위해 `Object.prototype`의 빌트인 메서드는 아래처럼 간접 호출하는 것을 권장

```tsx
const obj = Object.create(null);
obj.a = 1;

// Object.prototype의 빌트인 메서드는 객체로 직접 호출하지 않는다.
console.log(Object.prototype.hasOwnProperty.call(obj, "a")); // true
```

## 19.11.2. 객체 리터럴 내부에서 `__proto__`에 의한 직접 상속

`Object.create` 메서드의 장점에도 불구하고, 두번째 인자로 프로퍼티를 정의하는 점은 번거로움. 일단 객체를 생성한 이후, 동적으로 프로퍼티를 추가하는 방법도 깔끔하지는 않다.

이때 `__proto__`를 활용해 직접 상속을 구현할 수 있다.

```tsx
const myProto = { x: 10 };

// 객체 리터럴에 의해 객체를 생성하면서 프로토타입을 지정하여 직접 상속받을 수 있다.
const obj = {
  y: 20,
  // 객체를 직접 상속 받는다.
  // obj → myProto → Object.prototype → null
  __proto__: myProto,
};
/*
const obj = Object.create(myProto, {
	y: { value: 20, writable: true, enumerable: true, configurable: true }
});
*/

console.log(obj.x, obj.y); // 10 20
console.log(Object.getPrototypeOf(obj) === myProto); // true
```

# 19.12. 정적 프로퍼티 / 메서드

- 인스턴스를 생성하지 않고도 참조 / 호출할 수 있는 프로퍼티 / 메서드

```tsx
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi, My name is ${this.name}`);
};

// 정적 프로퍼티
Person.staticProp = "static prop";

// 정적 메서드
Person.staticMethod = function () {
  console.log("static method");
};

const me = new Person("Lee");

// 생성자 함수에 추가한 정적 프로퍼티/메서드는 생성자 함수로 참조/호출한다.
Person.staticMethod(); // staticMethod

// 정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조/호출할 수 없다.
// 인스턴스로 참조/호출할 수 있는 프로퍼티/메서드는 프로토타입 체인 상에 존재해야 한다.
me.staticMethod(); // TypeError: me.staticMethod is not a function
```

`Person` 생성자 함수는 객체이므로 자신의 프로퍼티 / 메서드를 소유할 수 있고, 이를 정적 메서드 / 프로퍼티라고 한다. 정적 프로퍼티 / 메서드는 생성자 함수가 생성한 인스턴스로 참조 / 호출할 수 없다.

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F2fb7cb14-1d9c-4510-b588-41c4b92772bd%2FUntitled.png?table=block&id=4e523761-cfd1-4b41-a916-b706acaef1e9&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

- 생성자 함수가 생성한 인스턴스는 프로토타입 체인에 속한 프로퍼티 / 메서드에는 접근할 수 있지만 정적 프로퍼티 / 메서드는 프로토타입 체인에 속하지 않았으므로 접근할 수 없다.

## 19.12.1. `Object.create` vs `Object.hasOwnProperty`

- `Object.create`: `Object`의 정적 메서드
- `Object.hasOwnProperty`: `Object.prototype`의 메서드

따라서 `Object.create` 는 메서드 인스턴스, 즉 `Object` 생성자 함수가 생성한 객체는 호출할 수 없다. 하지만 `Object.prototype.hasOwnProperty` 메서드는 모든 객체의 프로토타입 체인의 종점, 즉 `Object.prototype`의 메서드이므로 모든 객체가 호출할 수 있다.

```tsx
// Object.create는 정적 메서드다.
const obj = Object.create({ name: "Lee" });

obj.hasOwnProperty("name"); // → false
```

## 19.12.2. `this`와 정적 프로퍼티

만약 인스턴스 / 프로토타입 메서드 내에서 `this`를 사용하지 않는다면, 그 메서드는 정적 메서드로 변경할 수 있다. 인스턴스가 호출한 인스턴스 / 프로토타입 메서드 내에서 `this`는 인스턴스를 가리킨다. 메서드 내에서 인스턴스를 참조할 필요가 없다면, 정적 메서드로 변경하여도 동작한다. 프로토타입 메서드를 호출하려면 인스턴스를 생성해야 하지만 정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있다.

```tsx
function Foo() {}

// 프로토타입 메서드
// this를 참조하지 않는 프로토타입 메서드는 정적 메서드로 변경하여도 동일한 효과를 얻을 수 있다.
Foo.prototype.x = function () {
  console.log("x");
};

const foo = new Foo();
// 프로토타입 메서드를 호출하려면 인스턴스를 생성해야 한다.
foo.x(); // x

// 정적 메서드
Foo.x = function () {
  console.log("x");
};
// 정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있다.
Foo.x(); // x
```

`MDN` 문서를 보면 아래와 같이 정적 프로퍼티 / 메서드와 프로토타입 프로퍼티 / 메서드를 구분해 놓고 있음. 따라서 표기만으로도 정적 메서드와 프로토타입 메서드를 구분할 수 있음

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F59f7babf-3d27-4916-910b-405e1b73ea9a%2FUntitled.png?table=block&id=48bd5b32-5e91-44c8-a07f-c24a1a5b757a&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

- 프로토타입 메서드를 표기할 때 `prototype` 을 `#`으로 표기하는 경우도 있으므로 유의
- ex) `Object.prototype.isPrototypeOf` → `Object#isPrototypeOf`

# 19.13. 프로퍼티 존재 확인

## 19.13.1. `in` 연산자

`in` 연산자는 객체 내에 특정 프로퍼티가 존재하는지 여부를 확인한다

```tsx
// 사용법
key in object;
```

```tsx
const person = {
  name: "Baek",
  address: "Seoul",
};

// person 객체에 name 프로퍼티가 존재한다.
console.log("name" in person); // true
// person 객체에 address 프로퍼티가 존재한다.
console.log("address" in person); // true
// person 객체에 age 프로퍼티가 존재하지 않는다.
console.log("age" in person); // false
```

⚠️  주의점

`in` 연산자는 상속받은 모든 프로토타입의 프로퍼티를 확인하므로 주의 필요.

```tsx
console.log("toString" in person); // true
```

이는 `in` 연산자가 `person` 객체가 속한 프로토타입 체인 상에서 존재하는 모든 프로토타입에서 `toString` 프로퍼티를 검색했기 때문. `toString`은 `Object.prototype`의 메서드

### 19.3.1.1. `Reflect.has`

```tsx
const person = { name: "Baek" };

console.log(Reflect.has(person, "name")); // true
console.log(Reflect.has(person, "toString")); // true
```

`Reflect.has` 메서드는 `in`연산자와 동일하게 작동하므로 `in` 대신 사용할 수도 있다.

## 19.13.2. `Object.prototype.hasOwnProperty` 메서드

`Object.prototype.hasOwnProperty` 메서드를 사용해도 객체에 특정 프로퍼티가 존재하는지 확인할 수 있다.

```tsx
console.log(person.hasOwnProperty("name")); // true
console.log(person.hasPwnProperty("age")); // false
```

### 19.13.2.1. `in`과의 차이점

이름에서 알 수 있듯, 인수로 전달받은 프로퍼티 키가 객체 고유의 프로퍼티 키인 경우에만 `true`를 반환하고 프로토타입 프로퍼티 키인 경우 `false`를 반환

```tsx
console.log(person.hasOwnProperty("toString")); // false
```

# 19.14. 프로퍼티 열거

## 19.14.1. `for ... in` 문

객체의 모든 프로퍼티를 순회하며 열거 `enumeration` 하려면 `for ... in` 문을 사용한다.

```tsx
for (변수선언문 in 객체) { ... }
```

```tsx
const person = {
  name: "Lee",
  address: "Seoul",
};

// for ... in 문의 변수 prop에 person 객체의 프로퍼티 키가 할당된다.
for (const key in person) {
  console.log(key + ": " + person[key]);
}
// name: Lee
// address: Seoul
```

- `for ... in` 문은 객체의 프로퍼티 개수만큼 순회
- 선언한 `key` 변수에 프로퍼티 키를 할당
- 위의 경우 `person` 객체에는 두 개의 프로퍼티 키가 있으므로 객체를 2번 순회 하면서 프로퍼티 키를 `key` 변수에 할당한 후 코드 블록을 실행
- `for ... in` 문은 `in`연산자 처럼 순회 대상 객체의 프로퍼티 뿐만 아니라 상속받은 프로토타입의 프로퍼티까지 열거
- 하지만 위 예제의 경우 `toString`과 같은 `Object.prototype`의 프로퍼티가 열거되지 않는다.

```tsx
const person = {
  name: "Lee",
  address: "Seoul",
};

// in 연산자는 객체가 상속받은 모든 프로토타입의 프로퍼티를 확인한다.
console.log("toString" in person); // true

// for ... in 문도 객체가 상속받은 모든 프로토타입의 프로퍼티를 확인한다.
// 하지만 toString과 같은 Object.prototype의 프로퍼티가 열거되지 않는다.
for (const key in person) {
  console.log(key + ": " + person[key]);
}
// name: Lee
// address: Seoul
```

- 이는 `toString` 메서드가 열거할 수 없도록 정의된 프로퍼티이기 때문.
- 즉, `Objec.prototype.string` 프로퍼티의 프로퍼티 어트리뷰트 `[[Enumerable]]`의 값이 `false`이기 때문.

```tsx
console.log(Object.getOwnPropertyDescriptor(Object.prototype, "toString"));
// {value: ƒ, writable: true, enumerable: false, configurable: true}
```

- 따라서,
- **`for ... in` 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트** `**[[Enumerable]]`의 값이 `true`인 프로퍼티를 순회하며 열거 `enumeration` 한다.\*\*

```tsx
const person = {
	name: 'Lee',
	address: 'Seoul'
	__proto__: { age: 20 }
};

for (const key in person) {
	console.log(key + ': ' + person[key]);
}
// name: Lee
// address: Seoul
// age: 20 -> 프로토타입도 된다!
```

- 키가 심벌인 프로퍼티는 열거하지 않는다. (왜요…?)

```tsx
const sym = Symbol();
const obj = {
  a: 1,
  [sym]: 10,
};

for (const key in obj) {
  console.log(key + ": " + obj[key]);
}
// a: 1
```

- 상속받은 프로퍼티는 제외하고 객체 자신의 프로퍼티만 열거하려면 `Object.prototype.hasOwnProperty` 메서드를 사용하여 객체 자신의 프로퍼티인지 확인

```tsx
const person = {
	name: 'Lee',
	address: 'Seoul'
	__proto__: { age: 20 }
};

for (const key in person) {
	// 객체 자신의 프로퍼티인지 확인
	if (!person.hasOwnProperty(key)) continue;
	console.log(key + ': ' + obj[key]);
}
// name: Lee
// address: Seoul
```

### 19.4.1.1. 정렬

여기서 결과는 `person` 객체에 정의된 순서대로 열거됐다. 하지만 `for ... in` 문은 프로퍼티를 열거할 때 순서를 보장하지 않으므로 주의하기 바란다. 하지만 대부분의 모던 브라우저는 순서를 보장하고 숫자(형태를 한 문자열)인 프로퍼티 키에 대해서는 정렬을 실시한다.

```tsx
const obj = {
  2: 2,
  3: 3,
  1: 1,
  b: "b",
  a: "a",
};

for (const key in obj) {
  if (!obj.hasOwnProperty(key)) continue;
  console.log(key + ": " + obj[key]);
}

/*
1: 1
2: 2
3: 3
b: b
a: a
*/
```

배열에서는 `for ... in` 문은 그다지 권장하지 않음.

일반적인 `for` 문이나 `for ... of` 문 또는 `Array.prototype.forEach` 메서드를 사용하기를 권장. 사실 배열도 객체이므로 프로퍼티와 상속받은 프로퍼티가 포함될 수 있기 때문

```tsx
const arr = [1, 2, 3];
arr.x = 10; // 배열도 객체이므로 프로퍼티를 가질 수 있다.

// BAD
for (const i in arr) {
  // 프로퍼티 x도 출력된다.
  console.log(arr[i]); // 1 2 3 10
}

// arr.length = 3
for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]); // 1 2 3
}

// forEach 메서드는 요소가 아닌 프로퍼티는 제외한다.
arr.forEach((v) => console.log(v)); // 1 2 3

// for ... of는 변수 선언문에서 선언한 변수에 키가 아닌 값을 할당한다.
for (const value of arr) {
  console.log(value); // 1 2 3
}
```

`forEach` 메서드에 대해서는 27.9.2절 `Array.prototype.forEach`에서, `for ... of` 문에 대해서는 34.3절 `for ... of` 문에서 자세히 살펴봄

## 19.4.2. `Object.keys/values/entries` 메서드

이때까지 살펴본 것 처럼 `for ... in` 문은 객체 자신의 고유 프로퍼티 외에 상속받은 프로퍼티도 열거.

- `Object.prototype.hasOwnProperty` 메서드를 사용하여 객체 자신의 프로퍼티인지 확인하는 추가 처리가 필요
- 객체 자신의 프로퍼티만 열거하기 위해서는 `Object.keys/values/entries` 메서드를 사용하는 것을 권장

```tsx
const person = {
	name: 'Lee',
	address: 'Seoul'
	__proto__: { age: 20 }
};

console.log(Object.keys(person)); // ["name", "address"]
```

- `ES6`에서 도입된 `Object.values` 메서드는 객체 자신의 열거 가능한 프로퍼티 값을 배열로 반환

```tsx
console.log(Object.values(person)); // ["Lee", "Seoul"]
```

- `ES8`에서 도입된 `Object.entries` 메서드는 객체 자신의 열거 가능한 프로퍼티 키와 값의 쌍의 배열을 배열에 담아 반환

```tsx
console.log(Object.entries(person)); // [["name", "Lee"], ["address", "Seoul"]]

Object.entries(person).forEach(([key, value]) => console.log(key, value));
/*
name Lee
address Seoul
*/
```
