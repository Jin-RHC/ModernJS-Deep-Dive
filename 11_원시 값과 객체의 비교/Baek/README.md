# 11. 원시 값과 객체의 비교

Created: 2022년 6월 22일 오전 5:43
Tags: javascript

## 11.0. `Primitive type`과 `object/reference type`의 차이

| primitive type                | object/reference type              |
| ----------------------------- | ---------------------------------- |
| 변경 불가능 (immutable)       | 변경 가능 (mutable)                |
| 변수에 실제 값이 저장         | 변수에 참조 값(메모리 주소)이 저장 |
| pass by value로 복사가 일어남 | pass by reference로 복사가 일어남  |

## 11.1. 원시 값

### 11.1.1. 변경 불가능한 값

- **원시 값은 변경 불가능한 값이다. (immutable value)**
- 한 번 생성된 원시값은 `read-only`
- 변수가 변경 불가능하다는 것이 아님.
  - 변수: 할당된 메모리 공간과 이를 식별하기 위한 이름을 이르는 말
  - 값: 표현식이 평가되어 생성된 결과가 변수의 메모리 공간에 저장된 것
- 변수는 언제든지 재할당을 통해 값 변경이 가능하다.
- 그럼 상수는 변경이 안되니까 `immutable`인가?
  - No. 그냥 재할당이 불가능한 변수일 뿐 불변성과는 개념이 다름.

```tsx
let score;
score = 80;
score = 90;
```

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fa523aab2-84ef-4f68-8c9c-5a937ba2a102%2FUntitled.png?table=block&id=03701c2d-5eae-45f9-ae33-4b6b827a9f2d&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

원시 값은 변경 불가능하기 때문에 재할당을 통해 값 변경

만약 `mutable`이었다면 메모리 주소는 바뀌지 않고 한 주소에서 값만 변경 됨

**원시 값을 할당한 변수는 재할당 이외에는 변수 값을 변경할 수 있는 방법이 없다.**

### 11.1.2. 문자열과 불변성

- 문자열 타입의 저장공간은 **글자수 당 2바이트**
- 즉, 문자열 변수의 저장공간 크기는 글자 수에 따라 가변적
  - 문자의 종류에 따라서도 가변적인듯
- `javascript`에서는 편의를 위해 원시 타입으로 제공
- 원시타입이므로 `immutable`하다.

```tsx
let str = "hello";
str = "world";
```

- 위 코드에서 `hello`와 `world`는 모두 메모리에 있고, 변수 `str`이 재할당을 통해 가리키는 메모리가 변경되었을 뿐

> **유사 배열 객체**  
> 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고, `length`프로퍼티를 갖는 객체

`string`값은 위 조건을 만족하는 유사 배열 객체로, `for` 문으로 순회도 가능

>

```tsx
const str = "string";

// 배열과 유사하게 인덱스로 접근 가능
console.log(str[0]); // s

// 객체처럼 사용할 경우 래퍼 객체로 자동 변환
console.log(str.length); // 6
console.log(str.toUpperCase()); // STRING
```

- 아래와 같은 일부 문자 변경은 불가능

```tsx
str[0] = "S"; // 불가능
```

### 11.1.3. Pass by value

<aside>
💡 변수에 변수를 전달했을 때 무엇이 전달될까?에 대한 질문

</aside>

- 변수에 원시 값을 갖는 변수를 전달하면 **할당되는 변수의 원시값**이 복사되어 저장
- 이를 `pass by value`라고 이른다

```tsx
let score = 80;

let copy = score;

console.log(score, copy); // 80 80
console.log(score === copy); // true
```

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F582091ce-ea0b-418e-8eda-dd101e270b17%2FUntitled.png?table=block&id=da956495-9675-4a7d-a155-01450914186f&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

`score` 변수의 값 변경

```tsx
score = 100;

console.log(score, copy); // 100 80
console.log(score === copy); // false
```

- `score` 변수와 `copy` 변수의 값 80은 다른 메모리 공간에 저장된 별개의 값.
- 따라서 `score` 변수의 값을 변경해도 `copy`에는 어떠한 영향도 주지 않음

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fdab02ace-c06b-4964-b18e-547a182937ab%2FUntitled.png?table=block&id=1f27475b-0e93-4830-b04b-678c9be97a4a&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

- 메모리를 어떻게 관리해야 하는지는 `ECMAScript`에 정확하게 정의되어 있는 내용이 아니기 때문에 자바스크립트 엔진 마다 내부 동작 구조가 다를 수 있음
- 반드시 위 방식으로 동작하지 않을 수도 있는게, `python`의 경우에는 메모리 주소까지 같은 곳을 바라보다가 재할당 시점에 바뀐 변수가 떠나가게 구현 됨

> 💡 엄격하게 표현하면 `pass by value`도 사실은 값을 전달하는 게 아니라 메모리 주소를 전달하는 것. 메모리 공간을 참조하냐 메모리 공간의 값을 참조하느냐의 차이

중요한 것은 **두 변수의 원시 값은 결국에는 서로 다른 메모리에 있는 값으로 취급되어, 재할당 이후에 값이 변경 되더라도 서로 간섭할 수 없다**는 것

## 11.2. 객체

- 동적으로 `property`를 관리할 수 있음. 수가 정해져 있지 않고, 추가 삭제 가능
- 따라서 메모리 공간의 크기를 사전에 정해둘 수는 없다.
- 객체는 경우에 따라 크기가 매우 클 수 있기 때문에 프로퍼티에 접근하는 것도 원시값에 비해 비용이 많이 든다.
- 따라서 그 동작 방식이 원시값과는 다르게 구현

> 💡 **자바스크립트 객체의 관리 방식**  
> 자바스크립트의 객체 관리 방식은 C++, JAVA와 같은 클래스 기반 객체지향 언어와는 조금 다르다. 이 둘은 미리 클래스에 프로퍼티와 메서드가 정의되어 있으며, 그대로 객체를 찍어낸다. 그리고 동적인 변경이 불가능하다. 하지만 자바스크립트는 클래스 없이도 객체를 생성할 수 있고, 객체 생성 이후에도 프로퍼티와 메서드를 추가할 수 있다.  
> 이는 매우 편리하지만 성능면에서는 클래스 기반 객체지향 언어의 객체에 비해 불리하다.  
> 따라서 V8 자바스크립트 엔진에서는 `hidden class`라는 방식을 사용해서 C++에서 사용하는 수준 정도의 객체 프로퍼티 접근 성능을 보장한다고 함. 히든 클래스는 자바의 고정된 클래스 레이아웃과 유사하게 동작한다고 함.

### 11.2.1. 변경 가능한 값

- **객체는 변경 가능한 값이다.**
- 변수에 객체를 할당하면 변수는 해당 객체의 메모리 공간의 주소(참조값)를 저장
- 변수에 저장된 참조값을 통해 실제 객체로 접근

```tsx
const person = {
  name: "Lee",
};
```

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F25fd00b2-7b18-4b5a-af07-0954ce1e7958%2FUntitled.png?table=block&id=3858e859-1fba-4bb6-b54c-2638f1333d19&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

- 원시값은 변경 불가능하므로 재할당 이외에는 변경 방법이 없음
- 하지만 객체는 변경 가능.
- 객체를 할당한 변수는 재할당 없이 객체를 직접 변경 가능.
- 즉, 재할당 없이 ( = 메모리 주소를 변경하지 않고) 프로퍼티를 동적으로 추가할 수 있고, 프로퍼티 값을 갱신할 수도 있으며, 프로퍼티 자체를 지울 수도 있다.

```tsx
// 갱신
person.name = "Kim";

// 동적 생성
person.address = "Seoul";
```

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F191b2b51-f50e-4f0e-b408-d16c7c4ccaea%2FUntitled.png?table=block&id=cb02fbc1-263e-44d0-b7e1-793d54ee3227&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

- 왜 이렇게 하냐

> 💡 재할당 = 복사  
> 크기를 매번 일정하게 복사하기도 어렵고, 프로퍼티 값으로 객체를 가질 수도 있기 때문에 그 크기가 커서 복사(deep copy)해서 생성하는 비용이 많이 든다. 메모리의 효율적 소비가 어렵고 성능이 나빠진다.

따라서 메모리를 효율적으로 사용하기 위해, 그리고 객체를 복사해 생성하는 비용을 절약해서 생성하기 위해 변경가능한 값으로 설계돼 있음.

메모리 사용의 효율성, 성능 ↔  **다수 식별자가 하나의 객체 공유**

> 💡 **shallow copy vs deep copy**  
> 객체 프로퍼티의 한 단계까지만 복사하고 나머지는 참조값을 가져오는 경우: `shallow copy`
> 객체에 중첩돼 있는 모든 객체를 전부 복사하는 경우: `deep copy`

</aside>

```tsx
const obj = { x: { y: 1 } };

// shallow copy
const c1 = { ...obj };
console.log(c1 === obj); // false
console.log(c1.x === obj.x); // true

// lodash - cloneDeep을 사용한 deep copy
const _ = require("lodash");
console.log(c2 === obj); // false
console.log(c2.x === obj.x); // false
```

- 또다른 deep copy, shallow copy

```tsx
const v = 1;

const deep = v;

const o = { p: 1 };

const shallow = o;
```

- 원시값을 복사하면 deep copy
- 참조값을 복사하면 shallow copy

### 11.2.2. Pass by reference?

- 겉보기엔 pass by reference 처럼 생김

```tsx
const person = { name: "baek" };

// 무슨 카피?
const copy = person;

console.log(copy === person); // true

copy.name = "Kim";
copy.address = "Seoul";

console.log(person); // {name: "Kim", address: "Seoul"}
```

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F75878acd-8228-4c81-abf1-faafb804ef81%2FUntitled.png?table=block&id=270f2112-d3c5-4051-b5a0-7088c2c9e8da&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

<aside>
💡 `javascript`에는 pass by reference는 존재하지 않고, 오직 pass by value만이 존재한다.

</aside>

- 복사를 한 것도 참조값이라는 `값`을 복사한 것
- pass by reference, pass by value 모두 `ECMAScript`에 구체적으로 정의된 내용은 아니기 때문에 이해하는데 참고용으로만 사용하면 될 듯

## Quiz

```tsx
var person1 = {
  name: "baek",
};

var person2 = {
  name: "baek",
};

console.log(person1 === person2); // false
console.log(person1.name === person2.name); // true
```
