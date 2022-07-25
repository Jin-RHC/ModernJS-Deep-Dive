### 27.1 배열이란

- 배열은 순차적인 값을 나열한 자료구조이며 가장 기본적인 자료구조이다.
- 배열이 가지고 있는 값을 요소(element)라고 부른다.
- 자바스크립트 모든 값은 배열의 요소가 될 수 있다. (원시값, 객체, 함수, 배열 등)
- 자신의 위치를 나타내는 0 이상의 정수인 인덱스(index)를 갖는다.

| 구분            | 객체                      | 배열          |
| --------------- | ------------------------- | ------------- |
| 구조            | 프로퍼티 키와 프로퍼티 값 | 인덱스와 요소 |
| 값의 참조       | 프로퍼티 키               | 인덱스        |
| 값의 순서       | X                         | O             |
| length 프로퍼티 | X                         | O             |

- 객체와 배열을 구분하는 가장 명확한 차이는 **값의 순서**와 **length 프로퍼티**이다.

```jsx
const arr = ['apple', 'banana', 'orange'];
for (let i = 0; i < arr.length; i++) {
  // i는 0부터 arr.length(3)까지 반복
  console.log(arr[i]);
  // console.log(arr[0]);
  // console.log(arr[1]);
  // console.log(arr[2]);
}
```

### 27.2 **자바스크립트 배열은 배열이 아니다**

- 자바스크립트의 배열은 일반적인 배열과 달리 동일한 메모리 크기를 갖지 않아도 되며, 연속적으로 이어져 있지 않을 수 있다. ⇒ [희소 배열 (sparse array)](https://itprogramming119.tistory.com/entry/JavaScript-10-%EB%B0%B0%EC%97%B4%EC%9D%98-%ED%99%9C%EC%9A%A9-%ED%9D%AC%EC%86%8C-%EB%B0%B0%EC%97%B4-%EB%8B%A4%EC%B9%98%EC%9B%90-%EB%B0%B0%EC%97%B4-%EC%97%B0%EA%B4%80-%EB%B0%B0%EC%97%B4)

```jsx
const arr = [
  'string',
  10,
  true,
  null,
  undefined,
  NaN,
  Infinity,
  [],
  {},
  function () {},
];
// 서로 다른 자료형을 자바스크립트 배열로 선언할 수 있음
```

- 인덱스로 배열 요소에 접근하는 경우에 일반적인 배열보다 느리지만, 특정 요소를 검색 및 삽입, 삭제하는 경우 일반 배열보다 빠르다.

### 27.3 length \***\*property\*\***와 희소배열

- length 프로퍼티는 배열의 길이를 나타내어 0 이상의 정수 값을 갖는다.
- length 프로퍼티 값은 빈 배열이면 0이고, 빈 배열이 아니면 가장 큰 인덱스 + 1이다.
- length 프로퍼티 값은 0 ~ 2^32 미만의 양의 정수이다.
- length 값은 배열에 요소를 추가 하거나 삭제하면 똑같이 변하게 된다.

```jsx
const arr = [1, 2, 3];
console.log(arr.length); // 3

// 배열 추가
arr.push(4);
console.log(arr.length); // 4

// 배열 삭제
arr.pop();
console.log(arr.length); // 3
```

- length의 값을 명시적으로 할당하면 그 요소의 값이 바뀌게 된다.

```jsx
const arr = [1, 2, 3, 4, 5];
arr.length = 3;
console.log(arr); // [1, 2, 3]
```

- 만일 요소 값을 하나만 선언하고, length의 값을 요소값의 인덱스보다 크게 설정하게 되면 프로퍼티 값은 변경되지만 배열의 길이는 늘어나지 않음

```jsx
const arr = [1];
arr.length = 3;
console.log(arr.length); // 3
console.log(arr); // [ 1, <2 empty items> ]
```

- 여기서 empty는 추가된 배열 요소가 아니고, arr[1], arr[2]에는 값이 존재하지 않음 값 없이 비어 있는 요소를 위해 메모리 공간을 확보하지 않으며 빈 요소를 생성하지도 않는다.
- 배열의 요소가 연속적으로 위치하지 않고 일부가 비어 있는 배열을 **희소배열**이라 한다.

```jsx
const sparse = [, 2, , 4];
console.log(sparse.length); // 4
console.log(sparse); // [empty, 2, empty, 4]
```

- 희소배열은 length와 배열 요소의 개수가 일치하지 않는다. 희소 배열의 length는 희소 배열의 실제 요소 개수보다 언제나 크다.
- 배열에서는 희소배열을 쓰지 않고 일반적인 배열 형식으로 사용해야, 자바스크립트 엔진에서 일반적인 배열과 같이 연속된 메모리 공간을 확보한다.

### 27.4 배열 생성

**27.4.1 배열 리터럴**

- 배열에 다양한 생성 방식이 있는데 가장 간편한 배열 방식 : 배열 리터럴
- 0개 이상의 요소를 쉼표로 구분하고 대괄호로 묶어줌.

```jsx
const arr1 = [1, 2, 3];
console.log(arr1); // 1, 2, 3

const arr2 = [];
console.log(arr2.length); // 0

const arr3 = [1, , 3]; // 희소배열
console.log(arr3.length);
console.log(arr3); // [1, empty, 3]
console.log(arr3[1]); // undefined
```

\***\*27.4.2 Array 생성자 함수\*\***

- Array 생성자 함수를 통해 배열을 생성 할 수 있다.
- Array 생성자 함수는 전달된 인수의 개수에 따라 다르게 동작하여 주의가 필요하다.

```jsx
const arr = new Array(10);
console.log(arr); // [empty * 10]
console.log(arr.length); // 10
```

→ 희소 배열이 생성되었다. 실제 배열의 요소가 존재하지 않지만 length 프로퍼티가 10이다.

- 배열은 최대 2^32-1개 까지 가질 수 있고 범위를 벗어나면 RangeError가 발생한다.
- 전달된 인수가 없으면 빈 배열로 생성됨
- 전달된 인수가 2개 이상이면 인수를 요소로 갖는 배열을 생성한다.

```jsx
new Array(1, 2, 3); // [1,2,3]

new Array({}); // [{}]
// 전달된 인수가 1개지만 숫자가 아니면 인수를 요소로 갖는 배열을 생성한다.
```

- Array 생성자 함수는 new연산자와 함께 호출하지 않더라도 배열을 생성하는 생성자 함수로 동작한다. Array 생성자 함수 내부에서 [new.target](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/new.target)을 확인하기 때문이다.

\***\*27.4.3 Array.of\*\***

- **`ES6`**에서 도입된 전달된 인수를 요소로 갖는 배열을 생성
- `Array`생성자 함수와 다르게 전달된 인수가 1개이고 숫자이더라도 인수를 요소로 갖는 배열을 생성

```jsx
Array.of(1);
Array.of(1, 2, 3);
Array.of('string');
```

\***\*27.4.4 Array.from\*\***

- 유사 배열 객체 또는 이터러블 객체를 인수로 전달받아 배열로 변환하여 반환

```jsx
Array.from({ length: 2, 0: 'a', 1: 'b' }); // ['a','b']
```

- 유사 배열 객체를 배열로 생성

```jsx
Array.from('Hello'); // [ 'H', 'e', 'l', 'l', 'o' ]
```

### 27.5 배열 요소의 참조

- 배열을 참조할 때 대괄호 표기법(`[]`)을 사용한다.
- 대괄호 안에 인덱스가 와야한다.

```jsx
const arr = [1, 2, 3];
console.log(arr[0]); // 1
console.log(arr[1]); // 2
console.log(arr[2]); // 3
console.log(arr[3]); // undefined, 인덱스 3이 존재하지 않아 undefined 출력
```

### 27.6 배열 요소의 추가와 갱신

- 존재하지 않는 인덱스를 사용해서 할당하여 동적으로 추가할 수 있다. ⇒ length 프로퍼티 자동 갱신
- length 프로퍼티보다 큰 값을 인덱스로 주면 희소배열이 된다.
- 명시적으로 값을 할당하지 않은 요소는 생성되지 않는다.

```jsx
const arr = [0];
arr[1] = 1; // 배열 추가
console.log(arr); // [0, 1]
console.log(arr.length); // 2

arr[100] = 100;
console.log(arr); // [ 0, 1, <98 empty items>, 100 ]  희소 배열
console.log(arr.length); // 101
```

- 존재하는 요소에 재할당하면 갱신된다.
- 만일 인덱스 위치에 정수가 아닌 다른 값이 들어가면 요소가 생성되는 것이 아닌 프로퍼티 생성되며 이때 추가된 프로퍼티는 length에 영향 주지 않는다.

```jsx
// 배열 요소 추가
const arr = [];
arr[0] = 1;
arr['1'] = 2;

// 프로퍼티 추가
arr['foo'] = 3;
arr.bar = 4;
arr[1.1] = 5;
arr[-1] = 6;
console.log(arr); // [ 1, 2, foo: 3, bar: 4, '1.1': 5, '-1': 6 ]

console.log(arr.length); // 2
```

### 27.7 배열 요소의 삭제

- 배열은 객체이기때문에 delete연산자 사용이 가능하지만, delete 연산자를 사용하게되면 length 프로퍼티 값은 변하지 않고 희소 배열이 된다.

⇒ `delete` 연산자를 삭제하지 않는 것이 좋음

```jsx
const arr = [1, 2, 3];
delete arr[1];
console.log(arr); // [1, empty, 3]

// length에 영향을 주지 않고 요소만 삭제
console.log(arr.length); // 3
```

- 요소와 length를 삭제 하려면 `splice`를 사용해야 한다.

```jsx
const arr = [1, 2, 3];

arr.splice(1, 1); // Array.prototype.splice(삭제를 시작할 인덱스, 삭제할 요소 수)
// arr[1]부터 1개의 요소를 제거

console.log(arr); // [ 1, 3 ]

console.log(arr.length); // 2
```

### 27.8 배열 메서드

- 자바스크립트는 **배열**을 다룰 때 유용한 **다양한 빌트인 메서드**를 제공한다.
- **Array 생성자 함수**는 **정적 메서드**를 제공하며, 배열 객체의 프로토타입인 `Array.prototype`은 **프로토타입 메서드**를 제공한다.
- 배열 메서드는 결과물을 반환하는 패턴이 두 가지이다.
  - **배열에는 원본 배열(배열 메서드를 호출한 배열, 즉 배열 메서드의 구현체 내부에서 this가 가리키는 객체)을 직접 변경하는 메서드(Mutator method)**
  - **원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드(Accessor method)가 있다.**

```jsx
const arr = [1];

// push 메서드는 원본 배열(arr)을 직접 변경한다.
arr.push(2);
console.log(arr); // [1, 2]

// concate 메서드는 원본 배열(arr)을 직접 변경하지 않고 새로운 배열을 생성하여 반환한다.
const result = arr.concat(3);
console.log(arr); // [1, 2]
console.log(result); // [1, 2, 3]
```

- 원본 배열을 직접 변경하는 메서드는 외부 상태를 직접 변경하는 부수 효과가 있으므로 사용할 때 주의해야 한다. 따라서 가급적 원본 배열을 직접 변경하지 않는 메서드를 사용하는 편이 좋다.
- 배열이 제공하는 메서드 중에서 사용 빈도가 높은 메서드에 대해 살펴보도록 하자.

### 27.8.1 Array.isArray

- **`Array.isArray`**는 **Array 생성자 함수의 정적 메서드**다.
- **`Array.of`**와 **`Array.from`**도 Array 생성자 함수의 정적 메서드다.
- `Array.isArray` 메서드는 전달된 인수가 **배열이면** **true**, **배열이 아니면** **false**를 반환한다.

```jsx
// true
Array.isArray([]);
Array.isArray([1, 2]);
Array.isArray([new Array()]);

// false
Array.isArray();
Array.isArray({});
Array.isArray(null);
Array.isArray(undefined);
Array.isArray(1);
Array.isArray('Array');
Array.isArray(true);
Array.isArray(false);
Array.isArray({ 0: 1, length: 1 });
```

### 27.8.2 Array.prototype.indexOf

- `indexOf` **메서드**는 원본 배열에서 인수로 전달된 요소를 검색하여 **인덱스를 반환**한다.
  - 원본 배열에 인수로 전달한 요소와 중복되는 요소가 여러 개 있다면 **첫 번째로 검색된 요소의 인덱스를 반환**한다.
  - 원본 배열에 인수로 전달한 요소가 존재하지 않으면 **-1**을 **반환**한다.

```jsx
const arr = [1, 2, 2, 3];

// 배열 arr에서 요소 2를 검색하여 첫 번째로 검색된 요소의 인덱스를 반환한다.
arr.indexOf(2); // 1
// 배열 arr에 요소 4가 없으므로 -1을 반환한다.
arr.indexOf(4); // -1
// 두 번째 인수는 검색을 시작할 인덱스다. 두 번째 인수를 생략하면 처음부터 검색한다.
arr.indexOf(2, 2); // 2
```

- indexOf 메서드는 배열에 특정 요소가 존재하는지 확인할 때 유용하다.

```jsx
const foods = ['apple', 'banana', 'orange'];

// foods 배열에 'orange' 요소가 존재하는지 확인한다.
if (foods.indexOf('orange') === -1 {
    // foods 배열에 'orange' 요소가 존재하지 않으면 'orange' 요소를 추가한다.
    foods.push('orange');
}

console.log(foods); // ["apple", "banana", "orange"]
```

### 27.8.3 Array.prototype.push

- `push`메서드는 인수로 전달받은 모든 값을 원본 배열의 마지막 요소로 추가하고 변경된 **length 프로퍼티 값을 반환**한다.
- push 메서드는 **원본 배열을 직접 변경**한다.

```jsx
const arr = [1, 2];

// 인수로 전달받은 모든 값을 원본 배열 arr의 마지막 요소로 추가하고 변경된 length 값을 반환한다.

let result = arr.push(3, 4);
console.log(result); // 4

// push 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [1, 2, 3, 4]
```

- push 메서드는 성능 면에서 좋지 않다. ⇒ 마지막 요소로 추가할 요소가 하나뿐이라면 push 메서드를 사용하지 않고 length 프로퍼티를 사용하여 배열의 마지막에 요소를 직접 추가할 수도 있다.
- 또한 **push 메서드**는 원본 배열을 직접 변경하는 **부수 효과**가 있다.
- 따라서 **push 메서드**보다는 **ES6의 스프레드 문법을 사용하는 편이 좋다**.
- **스프레드 문법**을 사용하면 함수 호출 없이 표현식으로 마지막에 요소를 추가할 수 있으며 부수 효과도 없다.

```jsx
const arr = [1, 2];

// ES6 스프레드 문법
const newArr = [...arr, 3];
console.log(newArr); // [1, 2, 3]
```

### 27.8.4 Array.prototype.pop

- **pop** 메서드는 원본 배열에서 **마지막 요소를 제거하고 제거한 요소를 반환**한다.
- 원본 배열이 빈 배열이면 **undefined**를 반환한다.
- pop 메서드는 **원본 배열을 직접 변경**한다.

```jsx
const arr = [1, 2];

// 원본 배열에서 마지막 요소를 제거하고 제거한 요소를 반환한다.
let result = arr.pop();
console.log(result); // 2

// pop 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [1]
```

- pop 메서드와 push 메서드를 사용하면 스택을 쉽게 구현할 수 있다.
  - 스택은 데이터를 마지막에 밀어 넣고, 마지막에 밀어 넣은 데이터를 먼저 꺼내는 후입 선출(LIFO - Last In First Out) 방식의 자료구조다.

### 27.8.5 Array.prototype.unshift

- **unshift** 메서드는 인수로 전달받은 모든 값을 **원본 배열의 선두에 요소로 추가**하고 **변경된 length 프로퍼티 값을 반환**한다.
- **unshift** 메서드는 **원본 배열을 직접 변경**한다.

```jsx
const arr = [1, 2];

// 인수로 전달받은 모든 값을 원본 배열의 선두에 요소로 추가하고 변경된 length 값을 반환한다.
let result = arr.unshift(3, 4);
console.log(result); // 4

// unshift 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [3, 4, 1, 2]
```

- unshift 메서드는 원본 배열을 직접 변경하는 부수 효과가 있다.
- 따라서 unshift 메서드보다는 ES6의 스프레드 문법을 사용하는 편이 좋다.
- 스프레드 문법을 사용하면 함수 호출 없이 표현식으로 선두에 요소를 추가할 수 있으며 부수 효과도 없다.

```jsx
const arr = [1, 2];

// ES6 스프레드 문법
const newArr = [3, ...arr];
console.log(newArr); // [3, 1, 2]
```

### 27.8.6 Array.prototype.shift

- shift 메서드는 **원본 배열에서 첫 번째 요소를 제거**하고 **제거한 요소를 반환**한다.
- 원본 배열이 **빈 배열**이면 **undefined를 반환**한다.
- shift 메서드는 **원본 배열을 직접 변경**한다.

```jsx
const arr = [1, 2];

// 원본 배열에서 첫 번째 요소를 제거하고 제거한 요소를 반환한다.
let result = arr.shift();
console.log(result); // 1

// shift 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [2]
```

- **shift** 메서드와 **push** 메서드를 사용하면 큐를 쉽게 구현할 수 있다.
- 큐(Queue)는 데이터를 마지막에 밀어 넣고, 처음 데이터, 즉 가장 먼저 밀어 넣은 데이터를 먼저 꺼내는 선입 선출(FIFO = First In First Out) 방식의 자료구조다.

### 27.8.7 Array.prototype.concat

- **concat** 메서드는 인수로 전달된 값들(배열 또는 원시값)을 **원본 배열의 마지막 요소로 추가한 새로운 배열을 반환**한다.
- 인수로 전달한 값이 배열인 경우 배열을 해체하여 **새로운 배열의 요소로 추가**한다.
- **원본 배열은 변경되지 않는다.**

```jsx
const arr1 = [1, 2];
const arr2 = [3, 4];

// 배열 arr2를 원본 배열 arr1의 마지막 요소로 추가한 새로운 배열을 반환한다.
// 인수로 전달한 값이 배열인 경우 배열을 해체하여 새로운 배열의 요소로 추가한다.
let result = arr1.concat(arr2);
console.log(result); // [1, 2, 3, 4]

// 숫자를 원본 배열 arr1의 마지막 요소로 추가한 새로운 배열을 반환한다.
result = arr1.concat(3);
console.log(result); // [1, 2, 3]

// 배열 arr2와 숫자를 원본 배열 arr1의 마지막 요소로 추가한 새로운 배열을 반환한다.
result = arr1.concat(arr2, 5);
console.log(result); // [1, 2, 3, 4, 5]

// 원본 배열은 변경되지 않는다.
console.log(arr1); // [1, 2]
```

- **push**와 **unshift** 메서드는 **concat** 메서드로 대체할 수 있지만 다음과 같이 미묘한 차이가 있다.
  - `push`와 `unshift`메서드는 원본 배열을 **직접 변경**하지만 `concat`메서드는 원본 배열을 변경하지 않고 **새로운 배열을 반환**한다. ⇒ **push**와 **unshift** 메서드를 사용할 경우 **원본 배열을 반드시 변수에 저장**해 두어야 하며, **concat** 메서드를 사용할 경우 **반환값을 반드시 변수에 할당**받아야 한다.
  - 인수로 전달받은 값이 배열인 경우 **push**와 **unshift** 메서드는 배열을 그대로 **원본 배열의 마지막/첫 번째 요소로 추가**하지만 **concat** 메서드는 인수로 **전달받은 배열을 해체하여 새로운 배열의 마지막 요소로 추가**한다.
- concat 메서드는 ES6의 스프레드 문법으로 대체할 수 있다.

```jsx
let result = [1, 2].concat([3, 4]);
console.log(result); // [1, 2, 3, 4]

// concat 메서드는 ES6의 스프레드 문법으로 대체할 수 있다.
result = [...[1, 2], ...[3, 4]];
console.log(result); // [1, 2, 3, 4]
```

- 결론적으로 push/unshift 메서드와 concat 메서드를 사용하는 대신 ES6의 스프레드 문법을 일관성 있게 사용하는 것을 권장한다.

### 27.8.8 Array.prototype.splice

- **push, pop, unshift, shift** 메서드는 모두 **원본 배열을 직접 변경하는 메서드**이며 원본 배열의 **처음이나 마지막**에 요소를 추가하거나 제거한다.
- 원본 배열의 **중간에 요소를 추가**하거나 **중간에 있는 요소를 제거**하는 경우 **splice** 메서드를 사용한다.
- **splice** 메서드는 **3개의 매개변수**가 있으며 원본 배열을 직접 변경한다.
  - **start** : 원본 배열의 요소를 제거하기 시작할 인덱스다. start만 지정하면 원본 배열의 start부터 모든 요소를 제거한다. start가 음수인 경우 배열의 끝에서의 인덱스를 나타낸다. 만약 start가 -1이면 마지막 요소를 가리키고 -n이면 마지막에서 n번째 요소를 가리킨다.
  - **deleteCount**: 원본 배열의 요소를 제거하기 시작할 인덱스인 start부터 제거할 요소의 개수다. deleteCount가 0인 경우 아무런 요소도 제거되지 않는다.
  - **items**: 제거한 위치에 삽입할 요소들의 목록이다. 생략할 경우 원본 배열에서 요소들을 제거하기만 한다.

```jsx
const arr = [1, 2, 3, 4];

// 원본 배열의 인덱스 1부터 2개의 요소를 제거하고 그 자리에 새로운 요소 20, 30을 삽입한다.
const result = arr.splice(1, 2, 20, 30);

// 제거한 요소가 배열로 반환된다.
console.log(result); // [2, 3]
// splice 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [1, 20, 30, 4]
```

- splice 메서드에 3개의 인수를 빠짐없이 전달하면 **첫 번째 인수**, 즉 **시작 인덱스**부터 두 번째 인수, 즉 제거할 요소의 개수만큼 원본 배열에서 요소를 제거한다.
- 그리고 세 번째 인수, 즉 제거한 위치에 삽입할 요소들을 원본 배열에 삽입한다.
- **splice 메서드의 두 번째 인수**, 즉 제거할 요소의 개수를 0으로 지정하면 아무런 요소도 제거하지 않고 새로운 요소들을 삽입한다.

```jsx
const arr = [1, 2, 3, 4];

// 원본 배열의 인덱스 1부터 0개의 요소를 제거하고 그 자리에 새로운 요소 100을 삽입한다.
const result = arr.splice(1, 0, 100);

// 원본 배열이 변경된다.
console.log(arr); // [1, 100, 2, 3, 4]
// 제거한 요소가 배열로 반환된다.
console.log(result); // []
```

- **splice 메서드의 세 번째 인수**, 즉 제거한 위치에 추가할 요소들의 목록을 전달하지 않으면 원본 배열에서 지정된 요소를 제거하기만 한다.

```jsx
const arr = [1, 2, 3, 4];

// 원본 배열의 인덱스 1부터 2개의 요소를 제거한다.
const result = arr.splice(1, 2);

// 원본 배열이 변경된다.
console.log(arr); // [1, 4]
// 제거한 요소가 배열로 반환된다.
console.log(result); // [2, 3]
```

- **splice 메서드의 두 번째 인수**, 즉 제거할 요소의 개수를 생략하면 첫 번째 인수로 **전달된 시작 인덱스부터 모든 요소를 제거**한다.

```jsx
const arr = [1, 2, 3, 4];

// 원본 배열의 인덱스 1부터 모든 요소를 제거한다.
const result = arr.splice(1);

// 원본 배열이 변경된다.
console.log(arr); // [1]
// 제거한 요소가 배열로 반환된다.
console.log(result); // [2, 3, 4]
```

- 배열에서 특정 요소를 제거하려면 **indexOf** 메서드를 통해 특정 요소의 인덱스를 취득한 다음 splice 메서드를 사용한다.

```jsx
const arr = [1, 2, 3, 1, 2];

// 배열 array에서 item 요소를 제거한다. item 요소가 여러 개 존재하면 첫 번째 요소만 제거한다.
function remove(array, item) {
  // 제거할 item 요소의 인덱스를 취득한다.
  const index = array.indexOf(item);

  // 제거할 item 요소가 있다면 제거한다.
  if (index !== -1) array.splice(index, 1);

  return array;
}

console.log(remove(arr, 2)); // [1, 3, 1, 2]
console.log(remove(arr, 10)); // [1, 3, 1, 2]
```

### 27.8.9 Array.prototype.slice

- **slice** 메서드는 인수로 전달된 범위의 요소들을 복사하여 배열로 반환한다. 원본 배열은 변경되지 않는다. 이름이 유사한 splice 메서드는 원본 배열을 변경하므로 주의하기 바란다.
- **slice** 메서드는 두 개의 매개변수를 갖는다.
  - **start** : 복사를 시작할 인덱스다. 음수인 경우 배열의 끝에서의 인덱스를 나타낸다. 예를 들어, slice(-2)는 배열의 마지막 두 개의 요소를 복사하여 배열로 반환한다.
  - **end** : 복사를 종료할 인덱스다. 이 인덱스에 해당하는 요소는 복사되지 않는다. end는 생략 가능하며 생략 시 기본값은 length 프로퍼티 값이다.

```jsx
const arr = [1, 2, 3];

// arr[0]부터 arr[1] 이전(arr[1] 미포함)까지 복사하여 반환한다.
arr.slice(0, 1); // [1]

// arr[1]부터 arr[2] 이전(arr[2] 미포함)까지 복사하여 반환한다.
arr.slice(1, 2); // [2]

// 원본은 변경되지 않는다.
console.log(arr); // [1, 2, 3]
```

- **slice** 메서드는 첫 번째 인수(start)로 전달받은 인덱스부터 두 번째 인수(end)로 전달받은 인덱스 이전(end 미포함)까지 요소들을 복사하여 배열로 반환한다.
- slice 메서드의 두 번째 인수(end)를 생략하면 첫 번째 인수(start)로 전달받은 인덱스부터 모든 요소를 복사하여 배열로 반환한다.

```jsx
const arr = [1, 2, 3];

// arr[1]부터 이후의 모든 요소를 복사하여 반환한다.
arr.slice(1); // [2, 3]
```

- slice 메서드의 첫 번째 인수가 음수인 경우 배열의 끝에서부터 요소를 복사하여 배열로 반환한다.

```jsx
const arr = [1, 2, 3];

// 배열의 끝에서부터 요소를 한 개 복사하여 반환한다.
arr.slice(-1); // [3]

// 배열의 끝에서부터 요소를 두 개 복사하여 반환한다.
arr.slice(-2); // [2, 3]
```

- slice 메서드의 인수를 모두 생략하면 원본 배열의 복사본을 생성하여 반환한다.

```jsx
const arr = [1, 2, 3];

// 인수를 모두 생략하면 원본 배열의 복사본을 생성하여 반환한다.
const copy = arr.slice();
console.log(copy); // [1, 2, 3]
console.log(copy === arr); // false
```

- 이때 생성된 복사본은 얕은 복사를 통해 생성된다.

```jsx
const todos = [
  { id: 1, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 3, content: 'Javascript', completed: false },
];

// 얕은 복사(shallow copy)
const _todos = todos.slice();
// const _todos = [...todos];

// _todos와 todos는 참조값이 다른 별개의 객체다.
console.log(_todos === todos); // false

// 배열 요소의 참조값이 같다. 즉, 얕은 복사되었다.
console.log(_todos[0] === todos[0]); // true
```

> **얕은 복사**와 **깊은 복사객체**를 프로퍼티 값으로 갖는 객체의 경우
> **얕은 복사**는 한 단계까지만 복사하는 것을 말하고
> **깊은 복사**는 객체에 중첩되어 있는 객체까지 모두 복사하는 것을 말한다.

### 27.8.10 Array.prototype.join

- **join** 메서드는 **원본 배열**의 모든 요소를 **문자열로 변환**한 후, 인수로 전달받은 문자열, 즉 **구분자로 연결한 문자열을 반환**한다.
- 구분자는 **생략 가능**하며 **기본 구분자는 콤마(,)**다.

```jsx
const arr = [1, 2, 3, 4];

// 기본 구분자는 콤마다.
// 원본 배열 arr의 모든 요소를 문자열로 변환한 후 기본 구분자로 연결한 문자열을 반환한다.
arr.join(); // '1,2,3,4';

// 원본 배열 arr의 모든 요소를 문자열로 변환한 후, 빈 문자열로 연결한 문자열을 반환한다.
arr.join(''); // '1234'

// 원본 배열 arr의 모든 요소를 문자열로 변환한 후, 구분자 ':'로 연결한 문자열을 반환한다.
arr.join(':'); // '1:2:3:4'
```

### 27.8.11 Array.prototype.reverse

- reverse 메서드는 원본 배열의 순서를 반대로 뒤집는다.
- **원본 배열이 변경**된다.
- 반환값은 **변경된 배열**이다.

```jsx
const arr = [1, 2, 3];
const result = arr.reverse();

// reverse 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [3, 2, 1]
// 반환값은 변경된 배열이다.
console.log(result); // [3, 2, 1]
```

### 27.8.12 Array.prototype.fill

- ES6에서 도입된 fill 메서드는 인수로 전달받은 값을 배열의 처음부터 끝까지 요소로 채운다.
- **원본 배열이 변경**된다.

```jsx
const arr = [1, 2, 3];

// 인수로 전달받은 값 0을 배열의 처음부터 끝까지 요소로 채운다.
arr.fill(0);

// fill 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [0, 0, 0]
```

- 두 번째 인수로 요소 채우기를 시작할 인덱스를 전달할 수 있다.

```jsx
const arr = [1, 2, 3];

// 인수로 전달받은 값 0을 배열의 인덱스 1부터 끝까지 요소로 채운다.
arr.fill(0, 1);

// fill 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [1, 0, 0]
```

- 세 번째 인수로 요소 채우기를 멈출 인덱스를 전달할 수 있다.

```jsx
const arr = [1, 2, 3, 4, 5];

// 인수로 전달받은 값 0을 배열의 인덱스 1부터 3 이전(인덱스 3 미포함)까지 요소로 채운다.
arr.fill(0, 1, 3);

// fill 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [1, 0, 0, 4, 5]
```

- fill 메서드를 사용하면 배열을 생성하면서 특정 값으로 요소를 채울 수 있다.

```jsx
const arr = new Array(3);
console.log(arr); // [empty x 3]

// 인수로 전달받은 값 1을 배열의 처음부터 끝까지 요소로 채운다.
const result = arr.fill(1);

// fill 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [1, 1, 1]

// fill 메서드는 변경된 원본 배열을 반환한다.
console.log(result); // [1, 1, 1]
```

- fill 메서드로 요소를 채울 경우 모든 요소를 **하나의 값만으로 채울 수밖에 없다는 단점**이 있다.
- 하지만 Array.from 메서드를 사용하면 두 번째 인수로 전달한 콜백 함수를 통해 요소값을 만들면서 배열을 채울 수 있다.
- Array.from 메서드는 두 번째 인수로 전달한 콜백 함수에 첫 번째 인수에 의해 생성된 배열의 요소 값과 인덱스를 순차적으로 전달하면서 호출하고, 콜백 함수의 반환값으로 구성된 배열을 반환한다.

### 27.8.13 Array.prototype.includes

- ES7에서 도입된 includes 메서드는 배열 내에 **특정 요소가 포함되어 있는지 확인**하여 **true** 또는 **false**를 **반환**한다.
- 첫 번째 인수로 검색할 대상을 지정한다.

```jsx
const arr = [1, 2, 3];

// 배열에 요소 2가 포함되어 있는지 확인한다.
arr.includes(2); // true

// 배열에 요소 100이 포함되어 있는지 확인한다.
arr.includes(100); // false
```

- 두 번째 인수로 검색을 시작할 인덱스를 전달할 수 있고 두 번째 인수를 생략할 경우 기본값 0이 설정된다.
- 만약 두 번째 인수에 음수를 전달하면 length 프로퍼티 값과 음수 인덱스를 합산하여(length + index) 검색 시작 인덱스를 설정한다.

### 27.8.14 Array.prototype.flat

- ES10에서 도입된 flat 메서드는 인수로 전달한 깊이만큼 재귀적으로 배열을 평탄화한다.

```jsx
[1, [2, 3, 4, 5]].flat(); // [1, 2, 3, 4, 5]

[1, [2, [3, 4, 5]]].flat(); // [1, 2, [3, 4, 5]]
[1, [2, [3, 4, 5]]].flat(1); // [1, 2, [3, 4, 5]]

[1, [2, [3, 4, 5]]].flat().flat(); // [1, 2, 3, 4, 5]
[1, [2, [3, 4, 5]]].flat(Infinity); // [1, 2, 3, 4, 5]
```

## 27.9 배열 고차 함수

- 고차 함수는 **함수를 인수로 전달**받거나 **함수를 반환하는 함수**를 말한다.
- 자바스크립트의 함수는 일급 객체이므로 **함수를 값처럼 인수로 전달할 수 있으며 반환**할 수도 있다.
- **고차 함수**는 **외부 상태의 변경**이나 **가변 데이터를 피하고** **불변성을 지향**하는 **함수형 프로그래밍에 기반**을 두고 있다.
  - **함수형 프로그래밍**은 순수 함수와 보조 함수의 조합을 통해 로직 내에 존재하는 **조건문과 반복문을 제거**하여 복잡성을 해결하고 **변수의 사용을 억제**하여 상태 변경을 피하려는 프로그래밍 패러다임이다.
  - 조건문이나 반복문은 로직의 흐름을 이해하기 어렵게 하여 가독성을 해치고, 변수는 누군가에 의해 언제든지 변경될 수 있어 오류 발생의 근본적 원인이 될 수 있기 때문이다.
  - **함수형 프로그래밍**은 결국 **순수 함수를 통해 부수 효과를 최대한 억제**하여 오류를 피하고 프로그램의 안정성을 높이려는 노력의 일환이라고 할 수 있다.
- 자바스크립트는 고차 함수를 다수 지원한다. 특히 배열은 매우 유용한 고차 함수를 제공한다.

### 27.9.1 Array.prototype.sort

- sort 메서드는 **배열의 요소를 정렬**한다.
- **원본 배열을 직접 변경**하며 정렬된 배열을 반환한다.
- sort 메서드는 기본적으로 **오름차순으로 요소를 정렬한**다.
- **한글 문자열**인 요소도 **오름차순**으로 정렬된다.

```tsx
const fruits = ['Banana', 'Orange', 'Apple'];

// 오름차순(ascending) 정렬
fruits.sort();

// sort 메서드는 원본 배열을 직접 변경한다.
console.log(fruits); // ['Apple', 'Banana', 'Orange']

const fruits2 = ['바나나', '오렌지', '사과'];

fruits2.sort();
console.log(fruits2); // ['바나나', '사과', '오렌지]
```

- **sort** 메서드는 기본적으로 **오름차순으로 요소**를 **정렬**한다.
- 따라서 내림차순으로 요소를 정렬하려면 **sort** 메서드를 사용하여 오름차순으로 정렬한 후 **reverse** 메서드를 사용하여 요소의 순서를 뒤집는다.

```jsx
const fruits = ['Banana', 'Orange', 'Apple'];

// 오름차순(ascending) 정렬
fruits.sort();

// sort 메서드는 원본 배열을 직접 변경한다.
console.log(fruits); // ['Apple', 'Banana', 'Orange']

// 내림차순(descending) 정렬
fruits.reverse();

// reverse 메서드도 원본 배열을 직접 변경한다.
console.log(fruits); // ['Orange', 'Banana', 'Apple']
```

- 숫자 요소로 이루어진 배열을 정렬할 때는 주의가 필요하다.
- sort 메서드는 배열의 요소가 숫자 타입이라 할지라도 문자열로 변환한 후 유니코드 코드 포인트의 순서를 기준으로 정렬한다.
- 따라서 숫자 요소를 정렬할 때는 sort 메서드에 **정렬 순서를 정의하는 비교 함수를 인수로 전달**해야 한다. 비교 함수는 양수나 음수 또는 0을 반환해야 한다.
- 비교 함수의 반환 값이 0보다 작으면 비교 함수의 첫 번째 인수를 우선하여 정렬하고, 0이면 정렬하지 않으며, 0보다 크면 두 번째 인수를 우선하여 정렬한다.

```jsx
const points = [40, 100, 1, 5, 2, 25, 10];

// 숫자 배열의 오름차순 정렬. 비교 함수의 반환값이 0보다 작으면 a를 우선하여 정렬한다.
points.sort((a, b) => a - b);
console.log(points); // [1, 2, 5, 10, 25, 40, 100]

// 숫자 배열에서 최소/최대값 취득
console.log(points[0], points[points.length - 1]); // 1 100

// 숫자 배열의 내림차순 정렬. 비교 함수의 반환값이 0보다 작으면 b를 우선하여 정렬한다.
points.sort((a, b) => b - a);
console.log(points); // [100, 40, 25, 10, 5, 2, 1]

// 숫자 배열에서 최소/최대값 취득
console.log(points[points.length - 1], points[0]); // 1 100
```

- 객체를 요소로 갖는 배열을 정렬하는 예제는 다음과 같다.

```jsx
const todos = [
  { id: 4, content: 'JavaScript' },
  { id: 1, content: 'HTML' },
  { id: 2, content: 'CSS' },
];

// 비교 함수. 매개변수 key는 프로퍼티 키다.
function compare(key) {
  // 프로퍼티 값이 문자열인 경우 - 산술 연산으로 비교하면 NaN이 나오므로 비교 연산을 사용한다.
  // 비교 함수는 양수/음수/0을 반환하면 되므로 - 산술 연산 대신 비교 연산을 사용할 수 있다.
  return (a, b) => (a[key] > b[key] ? 1 : a[key] < b[key] ? -1 : 0);
}

// id를 기준으로 오름차순 정렬
todos.sort(compare('id'));
console.log(todos);
/*
[
  { id: 1, content: 'HTML' },
  { id: 2, content: 'CSS' },
  { id: 4, content: 'JavaScript' }
]
*/

// content를 기준으로 오름차순 정렬
todos.sort(compare('content'));
console.log(todos);
/*
[
  { id: 2, content: 'CSS' },
  { id: 1, content: 'HTML' },
  { id: 4, content: 'JavaScript' }
]
*/
```

### 27.9.2 Array.prototype.forEach

- 함수형 프로그래밍은 순수 함수와 보조 함수의 조합을 통해 로직 내에 존재하는 **조건문과 반복문을 제거**하여 복잡성을 해결하고 **변수의 사용을 억제**하여 상태 변경을 피하려는 프로그래밍 패러다임이다. 조건문이나 반복문은 로직의 흐름을 이해하기 어렵게 한다. 특히나 `**for**`문은 반복을 위한 변수를 선언해야 하며, 조건식과 증감식으로 이루어져 있어서 **함수형 프로그래밍이 추구하는 바와 맞지 않는다**.
- **forEach** 메서드는 **for 문을 대체할 수 있는 고차 함수**다.
- **forEach** 메서드는 자신의 내부에서 반복문을 실행한다.
- 즉, **forEach** 메서드는 반복문을 추상화한 고차 함수로서 내부에서 반복문을 통해 자신을 호출한 배열을 순회하면서 수행해야 할 처리를 콜백 함수로 전달받아 반복 호출한다.

```jsx
const numbers = [1, 2, 3];
const pows = [];

// forEach 메서드는 numbers 배열의 **모든 요소를 순회하면서 콜백 함수를 반복 호출**한다.
numbers.forEach((item) => pows.push(item ** 2));
console.log(pows);
[1, 4, 9];
```

- 위 예제의 경우 **forEach** 메서드는 **numbers** 배열의 모든 요소를 순회하며 콜백 함수를 반복 호출한다.
- forEach 메서드의 콜백 함수는 forEach 메서드를 호출한 배열의 요소값과 인덱스, forEach 메서드를 호출한 배열 자체, 즉 this를 순차적으로 전달받을 수 있다.
- 다시 말해, forEach 메서드는 콜백 함수를 호출할 때 3개의 인수, 즉 forEach 메서드를 호출한 배열의 요소값과 인덱스, forEach 메서드를 호출한 배열(this)을 순차적으로 전달한다.

```jsx
// forEach 메서드는 콜백 함수를 호출하면서 3개(1 **요소값 2 인덱스 3 this**)의 인수를 전달한다.
[1, 2, 3].forEach((item, index, arr) => {
  console.log(`요소값: ${item}, 인덱스: ${index}, this: ${arr}`);
});

/*
요소값: 1, 인덱스: 0, this: [1,2,3]
요소값: 2, 인덱스: 1, this: [1,2,3]
요소값: 3, 인덱스: 2, this: [1,2,3]
*/
```

- **forEach** 메서드는 **원본 배열을 변경하지 않는다.**
- 하지만 **콜백 함수**를 통해 **원본 배열을 변경**할 수는 있다.

```jsx
const numbers = [1, 2, 3];

// forEach 메서드는 원본 배열을 변경하지 않지만 콜백 함수를 통해 원본 배열을 변경할 수는 있다.
// 콜백 함수의 세 번째 매개변수 arr은 원본 배열 numbers를 가리킨다.
// 따라서 콜백 함수의 세 번째 매개변수 arr을 직접 변경하면 원본 배열 numbers가 변경된다.
numbers.forEach((item, index, arr) => {
  arr[index] = item ** 2;
});
console.log(numbers); // [1, 4, 9]
```

- forEach 메서드의 반환값은 언제나 **undefined**다.

```jsx
const result = [1, 2, 3].forEach(console.log);
console.log(result); // undefined
```

- **forEach** 메서드의 두 번째 인수로 **forEach** 메서드의 콜백 함수 내부에서 **this**로 사용할 객체를 전달할 수 있다.

```jsx
class Numbers {
  numberArray = [];

  multiply(arr) {
    arr.forEach(function (item) {
      // TypeError: Cannot read property 'numberArray' of undefined
      this.numberArray.push(item * item);
    });
  }
}

const numbers = new Numbers();
numbers.multiply([1, 2, 3]);
```

- **forEach** 메서드의 콜백 함수는 일반 함수로 호출되므로 콜백 함수 내부의 **this**는 **undefined**를 가리킨다. **this**가 전역 객체가 아닌 **undefined**를 가리키는 이유는 클래스 내부의 모든 코드에는 암묵적으로 **strict mode**가 적용되기 때문이다.
- forEach 메서드의 콜백 함수 내부의 **this**와 **multiply** 메서드 내부의 this를 일치시키려면 forEach 메서드의 두 번째 인수로 forEach 메서드의 콜백 함수 내부에서 this로 사용할 객체를 전달한다. 아래 예제의 경우 forEach 메서드의 두 번째 인수로 multiply 메서드 내부의 this를 전달하고 있다.

```jsx
class Numbers {
  numberArray = [];

  multiply(arr) {
    arr.forEach(function (item) {
      this.numberArray.push(item * item);
    }, this); // forEach 메서드의 콜백 함수 내부에서 this로 사용할 객체를 전달
  }
}

const numbers = new Numbers();
numbers.multiply([1, 2, 3]);
console.log(numbers.numberArray); // [1, 4, 9]
```

- 더 나은 방법은 ES6의 화살표 함수를 사용하는 것이다.
- 화살표 함수는 **함수 자체의 this 바인딩을 갖지 않는다**. 따라서 화살표 함수 내부에서 this를 참조하면 상위 스코프, 즉 **multiply 메서드 내부의 this를 그대로 참조**한다.

```jsx
class Numbers {
  numberArray = [];

  multiply(arr) {
    // 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조한다.
    arr.forEach((item) => this.numberArray.push(item * item));
  }
}

const numbers = new Numbers();
numbers.multiply([1, 2, 3]);
console.log(numbers.numberArray);
[1, 4, 9];
```

- forEach 메서드의 동작을 이해하기 위해 forEach 메서드의 폴리필을 살펴보자.

> **폴리필**
> 최신 사양의 기능을 지원하지 않는 브라우저를 위해 **누락된 최신 사양의 기능을 구현**하여 **추가**하는 것을 **폴리필**이라 한다.

```jsx
// 만약 Array.prototype에 forEach 메서드가 존재하지 않으면 **폴리필을 추가**한다.
if (!Array.prototype.forEach) {
  Array.prototype.forEach = function (callback, thisArg) {
    // 첫 번째 인수가 함수가 아니면 TypeError를 발생시킨다.
    if (typeof callback !== 'function') {
      throw new TypeError(callback + ' is not a function');
    }

    // this로 사용할 두 번째 인수를 전달받지 못하면 전역 객체를 this로 사용한다.
    thisArg = thisArg || window;

    // for 문으로 배열을 순회하면서 콜백 함수를 호출한다.
    for (var i = 0; i < this.length; i++) {
      // call 메서드를 통해 thisArg를 전달하면서 콜백 함수를 호출한다.
      // 이때 콜백 함수의 인수로 배열 요소, 인덱스, 배열 자신을 전달한다.
      callback.call(thisArg, this[i], i, this);
    }
  };
}
```

- **forEach** 메서드는 **for** 문과는 달리 **break, continue 문을 사용할 수 없다.**
- **forEach** 메서드는 **for** 문에 비해 **성능이 좋지는 않지만 가독성은 더 좋다**.
- 따라서 요소가 대단히 많은 배열을 순회하거나 시간이 많이 걸리는 복잡한 코드 또는 **높은 성능이 필요한 경우가 아니라면** **for 문 대신 forEach 메서드를 사용할 것을 권장**한다.

### 27.9.3 Array.prototype.map

- **map** 메서드는 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다.
- 그리고 **콜백 함수의 반환값들로 구성된 새로운 배열을 반환한다.**
- 이때 **원본 배열은 변경되지 않는다.**

```jsx
const numbers = [1, 4, 9];

// **map 메서드**는 numbers 배열의 모든 요소를 순회하면서 콜백 함수를 반복 호출한다.
// 그리고 콜백 함수의 반환값들로 구성된 새로운 배열을 반환한다.
const roots = numbers.map((item) => Math.sqrt(item));

// 위 코드는 다음과 같다.
// const roots = numbers.map(Math.sqrt);

// map 메서드는 새로운 배열을 반환한다.
console.log(roots); // [ 1, 2, 3 ]
// map 메서드는 원본 배열을 변경하지 않는다.
console.log(numbers); // [ 1, 4, 9 ]
```

- **forEach** 메서드는 **언제나 undefined를 반환**하고, **map** 메서드는 **콜백 함수의 반환값들로 구성된 새로운 배열을 반환**하는 차이가 있다.
- 즉, **forEach** 메서드는 단순히 **반복문을 대체하기 위한 고차 함수이**고, **map** 메서드는 **요소값을 다른 값으로 매핑한 새로운 배열을 생성**하기 위한 고차 함수다.
- **map 메서드가 생성하여 반환하는 새로운 배열의 length 프로퍼티 값은 map 메서드를 호출한 배열의 length 프로퍼티 값과 반드시 일치한다.**
- **즉, map 메서드를 호출한 배열과 map 메서드가 생성하여 반환한 배열은 1:1 매핑한다.**
- forEach 메서드와 마찬가지로 map 메서드의 콜백 함수는 map 메서드를 호출한 배열의 요소값과 인덱스, map 메서드를 호출한 배열 자체, 즉 this를 순차적으로 받을 수 있다.
- 다시 말해, map 메서드는 콜백 함수를 호출할 때 3개의 인수, 즉 map 메서드를 호출한 배열의 요소값과 인덱스 그리고 map 메서드를 호출한 배열(this)을 순차적으로 전달한다.

```jsx
// map 메서드는 콜백 함수를 호출하면서 **3개(요소값, 인덱스, this)**의 인수를 전달한다.
[1, 2, 3].map((item, index, arr) => {
  console.log(
    `요소값: ${item}, 인덱스: ${index}, this: ${JSON.stringify(arr)}`
  );
  return item;
});
/*
요소값: 1, 인덱스: 0, this: [1,2,3]
요소값: 2, 인덱스: 1, this: [1,2,3]
요소값: 3, 인덱스: 2, this: [1,2,3]
*/
```

### 27.9.4 Array.prototype.filter

- **filter** 메서드는 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다.
- 그리고 **콜백 함수의 반환값이 true인 요소로만 구성된 새로운 배열을 반환한다.**
- 이때 **원본 배열은 변경되지 않는다**.

```jsx
const numbers = [1, 2, 3, 4, 5];

// filter 메서드는 numbers 배열의 모든 요소를 순회하면서 콜백 함수를 반복 호출한다.
// 그리고 콜백 함수의 반환값이 true인 요소로만 구성된 새로운 배열을 반환한다.
// 다음의 경우 numbers 배열에서 홀수인 요소만 필터링한다(1은 true로 평가된다).

const odds = numbers.filter((item) => item % 2);
console.log(odds); // [1, 3, 5]
```

- forEach, map 메서드와 마찬가지로 filter 메서드는 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다.
- forEach 메서드는 언제나 **undefined**를 **반환하고**, map 메서드는 콜백 함수의 반환값들로 구성된 새로운 배열을 반환하지만 **filter** 메서드는 콜백 함수의 반환값이 **true인 요소만 추출**한 새로운 배열을 반환한다.
- filter 메서드는 자신을 호출한 배열에서 필터링 조건을 만족하는 특정 요소만 추출하여 새로운 배열을 만들고 싶을 때 사용한다.
- **filter 메서드가 생성하여 반환한 새로운 배열의 length 프로퍼티 값은 filter 메서드를 호출한 배열의 length 프로퍼티 값과 같거나 작다.**
- forEach, map 메서드와 마찬가지로 filter 메서드의 콜백 함수는 filter 메서드를 호출한 배열의 요소값과 인덱스, filter 메서드를 호출한 배열 자체, 즉 this를 순차적으로 전달받을 수 있다.
- filter 메서드는 자신이 호출한 배열에서 특정 요소를 제거하기 위해 사용할 수도 있다.

```jsx
class Users {
  constructor() {
    this.users = [
      { id: 1, name: 'Kyeom' },
      { id: 2, name: 'Lee' },
    ];
  }

  // 요소 추출
  findById(id) {
    // id가 일치하는 사용자만 반환한다.
    return this.users.filter((user) => user.id === id);
  }

  // 요소 제거
  remove(id_) {
    // id가 일치하지 않는 사용자를 제거한다.
    this.users = this.users.filter((user) => user.id !== id);
  }
}

const users = new Users();

let user = users.findById(1);
console.log(user); // [{ id: 1, name: 'Kyeom' }]

// id가 1인 사용자를 제거한다.
users.remove(1);

user = users.findById(1);
console.log(user); // []
```

- **filter** 메서드를 사용해 특정 요소를 제거할 경우 특정 요소가 중복되어 있다면 중복된 요소가 모두 제거된다. 특정 요소를 하나만 제거하려면 indexOf 메서드를 통해 특정 요소의 인덱스를 취득한 다음 spilce 메서드를 사용한다.

### 27.9.5 Array.prototype.reduce

- **reduce** 메서드는 자신을 호출한 배열을 모든 요소를 순회하며 인수로 전달받은 콜백 함수를 반복 호출한다.
- 그리고 콜백 함수의 반환값을 다음 순회 시에 콜백 함수의 첫 번째 인수로 전달하면서 콜백 함수를 호출하여 **하나의 결과값을 만들어 반환한다.**
- 이때 원본 배열은 변경되지 않는다.
- reduce 메서드는 첫 번째 인수로 콜백 함수, 두 번째 인수로 초기값을 전달받는다.
- reduce 메서드의 콜백 함수에는 4개의 인수, 초기값 또는 콜백 함수의 이전 반환값, reduce 메서드를 호출한 배열의 요소값과 인덱스, reduce 메서드를 호출한 배열 자체, 즉 this가 전달된다.

```jsx
// 1부터 4까지 누적을 구한다.
const sum = [1, 2, 3, 4].reduce(
  (accumulator, currentValue, index, array) => accumulator + currentValue,
  0
);

console.log(sum); // 10
```

- reduce 메서드의 콜백 함수는 4개의 인수를 전달받아 배열의 length만큼 총 4회 호출된다. 이때 콜백 함수로 전달되는 인수와 콜백 함수의 반환값은 다음과 같다.

| 구분         | accumulator | currentValue | index | array     | 콜백 함수의 반환값              |
| ------------ | ----------- | ------------ | ----- | --------- | ------------------------------- |
| 첫 번째 순회 | 0 (초기값)  | 1            | 0     | [1,2,3,4] | 1 (accumulator + currentValue)  |
| 두 번째 순회 | 1           | 2            | 1     | [1,2,3,4] | 3 (accumulator + currentValue)  |
| 세 번째 순회 | 3           | 3            | 2     | [1,2,3,4] | 6 (accumulator + currentValue)  |
| 4 번째 순회  | 6           | 4            | 3     | [1,2,3,4] | 10 (accumulator + currentValue) |

- 이처럼 reduce 메서드는 초기값과 배열의 첫 번째 요소값을 콜백 함수에게 인수로 전달하면서 호출하고 다음 순회에는 콜백 함수의 반환값과 두 번째 요소값을 콜백 함수의 인수로 전달하면서 호출한다.
- 이러한 과정을 반복하여 **reduce 메서드는 하나의 결과값을 반환한다.**
- **reduce 메서드를 호출할 때는 언제나 초기값을 전달하는 것이 안전하다.**

```jsx
const sum = [].reduce((acc, cur) => acc + cur);
// TypeError: Reduce of empty array with no initial value
```

- 이처럼 빈 배열로 reduce 메서드를 호출하면 에러가 발생한다. 이때 reduce 메서드에 초기값을 전달하면 에러가 발생하지 않는다.

```jsx
const sum = [].reduce((acc, cur) => acc + cur, 0);
console.log(sum); // 0
```

- 객체의 특정 프로퍼티 값을 합산하는 경우에는 반드시 초기값을 전달해야 한다.

```jsx
const products = [
  { id: 1, price: 100 },
  { id: 2, price: 200 },
  { id: 3, price: 300 },
];

/*
1번째 순회 : acc => 0,   cur => { id: 1, price: 100 }
2번째 순회 : acc => 100, cur => { id: 2, price: 200 }
3번째 순회 : acc => 300, cur => { id: 3, price: 300 }
*/
const priceSum = products.reduce((acc, cur) => acc + cur.price, 0);

console.log(priceSum); // 600
```

### 27.9.6 Array.prototype.some

- **some** 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출한다.
- 이때 **some** 메서드는 콜백 함수의 반환값이 **단 한번이라도 참이면 true,** **모두 거짓이면 false**를 반환한다.
- 단, **some** 메서드를 호출한 배열이 **빈 배열인 경우 언제나 false를 반환**하므로 주의하기 바란다.

```jsx
// 배열의 요소 중 10보다 큰 요소가 1개 이상 있는지 존재하는지 확인
[5, 10, 15].some((item) => item > 10); // true

// 배열의 요소 중 0 보다 작은 요소가 1개 이상 존재하는지 확인
[5, 10, 15].some((item) => item < 0); // false

// 배열의 요소 중 'banana'가 1개 이상 존재하는지 확인
['apple', 'banana', 'mango'].some((item) => item == 'banana'); // true

// some 메서드를 호출한 배열이 빈 배열인 경우 언제나 false를 반환한다.
[].some((item) => item > 3);
```

### 27.9.7 Array.prototype.every

- **every** 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출한다.
- 이때 **every** 메서드는 콜백 함수의 반환값이 **모두 참이면 true**, **단 한 번이라도 거짓이면 false**를 반환한다.
- 단, every 메서드를 호출한 배열이 **빈 배열**인 경우 **언제나 true를 반환**하므로 주의하기 바란다.

```jsx
// 배열의 모든 요소가 3보다 큰지 확인
[5,10,15].every(item => item > 3); // true

// 배열의 모든 요소가 10보다 큰지 확인
[5,10,15].every(item => item > 10) // false

// every 메서드를 호출한 배열이 빈 배열인 경우 언제나 true를 반환한다
[].every(item => item > 3); // true
```

### 27.9.8 Array.prototype.find

- ES6에서 도입된 find 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 **true인** **첫 번째 요소를 반환**한다.
- 콜백 함수의 반환값이 true인 요소가 **존재하지 않는다면** **undefined를 반환**한다.

```jsx
const users = [
  { id: 1, name: 'Lee' },
  { id: 2, name: 'Kim' },
  { id: 2, name: 'Choi' },
  { id: 3, name: 'Park' },
];

// id가 2인 첫 번째 요소를 반환한다. find 메서드는 배열이 아니라 요소를 반환한다.
users.find((user) => user.id === 2); // -> {id: 2, name: 'Kim'}
```

- find 메서드는 콜백 함수의 반환값이 true인 첫 번째 요소를 반환하므로 find의 결과값은 배열이 아닌 해당 **요소(element)값**이다.

### 27.9.9 Array.prototype.findInext

### 27.9.10 Array.prototype.flatMap
