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
