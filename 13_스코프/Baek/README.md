# 13. 스코프

Created: 2022년 6월 22일 오후 10:38
Tags: javascript

## 13.0. 새로 알게 된 것

- `javascript`에서 함수는 렉시컬 스코프를 따른다.

## 13.1. 스코프란?

> 모든 식별자(변수 이름, 함수 이름, 클래스 이름 등)는 자신이 선언된 위치에 의해 다른 코드가 식별자 자신을 참조할 수 있는 유효 범위가 결정된다. 이를 스코프라한다. 즉, **스코프는 식별자가 유효한 범위를 말한다.**

- `var` 키워드로 선언된 변수와 `let`, `const`키워드로 선언된 변수의 스코프가 다르게 동작함에 유의

```tsx
var var1 = 1;

if (true) {
  var2 = 2;
  if (true) {
    var var3 = 3;
  }
}

function foo() {
  let var4 = 4;

  function bar() {
    var var5 = 5;
  }
}

console.log(var1);
console.log(var2);
console.log(var3);
console.log(var4);
console.log(var5);
```

---

`var`은 같은 스코프 내에서 중복 선언 가능. `let`, `const`는 이미 선언된 스코프에서는 재선언 불가능

## 13.2. 스코프의 종류

코드의 구분

| 구분 | 설명                     | 스코프      | 변수      |
| ---- | ------------------------ | ----------- | --------- |
| 전역 | 코드의 가장 바깥 영역    | 전역 스코프 | 전역 변수 |
| 지역 | 블록 혹은 함수 몸체 내부 | 지역 스코프 | 지역 변수 |

### 13.2.1. 전역과 전역 스코프

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fe2e8b54d-2bef-4831-9110-f4156662b8d0%2FUntitled.png?table=block&id=b0609717-e5d2-4f37-ac18-299995b79fb0&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

- 전역: 가장 바깥 영역. 전역 변수는 어디서든지 참조할 수 있다.

### 13.2.2. 지역과 지역 스코프

- 지역이란 **함수 몸체 내부**를 말한다.
- 지역에 변수를 선언하면 지역 스코프를 갖는 지역 변수가 된다.
- 지역 변수는 자신이 선언된 지역과 그 하위 지역에서 유효하다.
- `inner`함수 내부에 선언된 `x` 또한 지역 변수. 스코프 체인을 통해 전역변수 `x`보다 먼저 바라보고 참조하게 된다.

## 13.3. 스코프 체인

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F532737e5-08f3-4dc9-b09f-e5f0ec6394cf%2FUntitled.png?table=block&id=10a46d4b-b5c4-4cb2-b4fc-46e5ad45a92e&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

- 함수의 중첩이 가능하다. `===` 함수의 지역 스코프도 중첩될 수 있다.
  - **이는 스코프가 함수의 중첩에 의해 함수처럼 계층 구조를 갖는다는 것을 의미**
- 모든 지역 스코프의 최상위 스코프는 전역 스코프
- 이렇게 스코프가 계층적으로 연결된 것을 `스코프 체인`이라고 함
- 참조하려는 식별자가 실행된 코드의 스코프에서 시작해서 상위 스코프 방향으로 거슬러 올라 가면서 선언된 식별자를 검색
- 스코프 체인은 자바스크립트 엔진에서 `렉시컬 환경`이라는 물리적인 실체로 구현돼 있음.
- 식별자가 선언되면 렉시컬 환경에 키로 등록되고, 할당이 이뤄지면 이 키에 해당하는 값을 변경

> **렉시컬 환경**
> 스코프 체인은 실행 컨텍스트의 렉시컬 환경을 단방향으로 연결한 것이다. 전역 렉시컬 환경은 코드가 로드 되면 곧바로 생성되고, 함수의 렉시컬 환경은 함수가 호출되면 곧바로 생성된다.

### 13.3.1. 스코프 체인에 의한 변수 검색

```tsx
var x = "global x";
var y = "global y";

function outer() {
  var z = "outer's local z";

  console.log(x); // 1
  console.log(y); // 2
  console.log(z); // 3

  function inner() {
    var x = "inner's local x";

    console.log(x); // 4
    console.log(y); // 5
    console.log(z); // 6
  }

  inner();
}

outer();

console.log(x);
console.log(z);
```

1. `x` 변수를 참조하는 코드의 스코프인 `inner` 함수의 지역 스코프에서 `x`변수가 선언되었는지 검색. 존재하므로 검색된 변수를 참조하고 검색 종료
2. `y` 변수는 `inner` 함수에 선언이 존재하지 않으므로 상위 스코프인 `outer`에서 검색. 마찬가지로 `outer`에도 없으므로 그 다음 상위 스코프인 전역 변수에서 검색. `y` 변수의 선언이 존재하므로 해당 변수를 참조하고 검색 종료
3. `outer` 에 있으므로 해당 변수를 참조하고 종료

**상위 변수에서 유효한 변수는 하위 스코프에서 자유롭게 참조할 수 있지만 하위 스코프에서 유효한 변수를 상위 스코프에서 참조할 수는 없다. 상속과 유사한 개념.**

### 13.3.2. 스코프 체인에 의한 함수 검색

```tsx
function foo() {
  console.log("global function foo");
}

function bar() {
  function foo() {
    console.log("local function foo");
  }
  foo();
}

bar();
```

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F6572e399-84ba-469b-94cd-2bb492fab3d2%2FUntitled.png?table=block&id=1907af23-8044-4146-aac2-69496265e63c&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2)

- 함수도 변수와 동일하게 식별자에 할당이 된다.
- 따라서 스코프는 변수를 검색할 때 사용하는 규칙이라기보다는 **식별자를 검색하는 규칙**이라는 표현이 더 명확

## 13.4. 함수 레벨 스코프

- **코드 블록이 아닌 함수에 의해서만 지역 스코프가 생성된다.**
- `C`나 `Java`와 같은 대부분의 프로그래밍 언어는 **블록 레벨 스코프**를 따름.
  - 블록레벨 스코프: 함수, 조건문, 반복문, `try/catch` 등의 모든 코드 블록이 스코프가 됨
- `javascript`의 `var` 만큼은 이런 블록 레벨 스코프를 따르지 않고 **함수 레벨 스코프를 따름.**

> **함수 레벨 스코프**
> 오로지 함수의 코드 블록 만을 지역 스코프로 인정

```tsx
var i = 10;

for (var i = 0; i < 5; i++) {
  console.log(i);
}

console.log(i);
```

출력 결과

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F91b9a582-1790-4ae4-92ac-b443ecef6183%2FUntitled.png?table=block&id=3481e9ce-b672-470c-9d36-147c628223df&spaceId=02fea814-1974-4421-ad79-1c751b190824&width=2000&userId=81ed4946-7ab1-42ae-99e6-7b19f7f96590&cache=v2g)

## 13.5. 렉시컬 스코프

```tsx
let x = 1;

function foo() {
  let x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo();
bar();
```

### 13.5.1. 동적 스코프와 렉시컬 스코프

> **동적 스코프**
> 함수를 어디서 `호출`했는지에 따라서 함수의 상위 스코프를 결정

- 함수를 정의하는 시점에는 함수가 어디서 호출될지 알 수 없다. 따라서 함수가 호출되는 시점에 동적으로 상위 스코프를 결정하기 때문에 동적 스코프라고 부른다.

> **정적 스코프**
> 함수를 어디서 `정의`했는지에 따라서 함수의 상위 스코프를 결정

- 스코프가 호출에 따라 동적으로 변하지 않고 함수 정의가 평가되는 시점에 상위 스코프가 정적으로 결정되기 때문에 정적 스코프라고 부른다.

### 13.5.2. `javascript`의 스코프

- `javascript`는 렉시컬 스코프를 따르므로 함수를 어디서 호출했는지가 아니라 함수를 어디서 정의했는지에 따라 상위 스코프를 결정한다.
- 함수가 호출된 위치는 상위 스코프 결정에 `어떠한 영향도 주지 않는다.`

따라서 위 예제에서도 `bar`함수의 상위 스코프는 정적으로 결정.
`10`을 먼저 출력하고 `1`을 출력할 것 같지만, `bar`의 상위스코프는 전역 스코프이므로 `1`이 두번 출력된다.

> 렉시컬 스코프는 클로저와 깊은 관계가 있다. 24장 `클로저`에서 알아봄

## 질문 리스트

`스코프 🔥`

- 스코프가 뭔가요? 🔥🔥🔥
- 스코프에는 어떤 종류가 있죠? 🔥🔥
- 렉시컬 스코프를 아나요? 안다면 렉시컬 스코프는 무엇을 의미하나요? 🔥
- 전역 변수로 변수를 선언하면 생기는 문제점은 무엇이 있을까요?
