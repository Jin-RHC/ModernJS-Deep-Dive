# 08. 제어문

- **제어문** : 조건에 따라 코드 블록을 **실행(조건문)**하거나 **반복실행(반복문)**하게 함.
- 제어문을 통해 실행 흐름을 인위적으로 제어 가능 ⇒ 흐름을 혼란, 가독성을 해침
- 후에 **forEach, map, filter, reduce** 등 함수형 프로그래밍을 통해 복잡성 해결

## 8.1 블록문

- 0개 이상의 문을 **중괄호**로 묶은 것 ⇒ **코드 블록** or **블록**

```jsx
// 블록문
{
  var foo = 10;
}

// 제어문
var a = 1;
if (x < 10) {
  x++;
}

// 함수 선언문
function sum(a, b) {
  return a + b;
}
```

## 8.2 조건문

- 주어진 조건식의 평가 결과에 따라 코드블록의 실행이 결정.
- 불리언 값으로 평가 (`if … else,` `switch`)

### if … else 문

```jsx
if (조건식1) {
  // 조건식1 가 참이면 실행
} else if (조건식2) {
  // 조건식2 가 참이면 실행
} else {
  //조건식1, 조건식2 모두 거짓이면 실행
}
```

- **삼항 조건 연산자**로도 바꿔 쓸 수 있다.

### switch 문

```jsx
switch (표현식) {
	case 표현식1:
		switch 문의 표현식과 표현식1이 일치하면 실행될 문;
		break;
	case 표현식2:
		switch 문의 표현식과 표현식2이 일치하면 실행될 문;
		break;
	default:
		switch 문의 표현식과 일치하는 case문이 없으면 실행될 문;
}
```

- if … else 보다 다양한 상황일 경우 사용한다.
- if … else 문으로 해결할 수 있으면 switch을 쓰는 대신 사용하는 것이 좋다.
- 하지만 가독성이 switch문이 나을 경우 사용하는 것이 좋다.

## 8.3 반복문

- 조건식의 평가 결과가 참일 경우 실행하며 조건식이 거짓일 때 까지 반복한다.
- `for`문, `while`문, `do … while` 문
  ### for문
  - 조건식이 거짓으로 평가될 때까지 반복 실행한다.
  ```jsx
  for (변수 선언문 또는 할당문 ; 조건식 ; 증감식){
  	// 조건식이 참 일 경우 반복 실행 될 문;
  }
  ```
  ### while문
  - 조건식의 평가 결과가 참일 경우 반복 실행된다.
  - 반복횟수가 **불명확**할 때 주로 사용한다.
  ```jsx
  var count = 0;

  while (count < 3) {
    console.log(count);
    count++;
  }
  ```
  ### do … while문
  - 코드블록 먼저 실행하고 조건식을 평가한다. 무조건 1번은 실행된다.
  ```jsx
  var count = 0;

  do {
    console.log(count);
    count++;
  } while (count < 3);
  ```

## 8.4 break 문

- break문은 레이블 문, 반복문(for, for… in, for… of, while, do … while), switch문의 코드 블록을 탈출한다.
- 그 외의 문에서 사용할 경우 **SyntaxError**가 발생한다.
  - **레이블문 :** 식별자가 붙은 문

```jsx
// foo라는 레이블 식별자가 붙은 레이블 문1
foo: console.log('foo');

// foo라는 레이블 식별자가 붙은 레이블 문2
foo: {
  console.log(1);
  break foo;
  console.log(2);
}

console.log('Done!');
```

## 8.5 continue 문

- continue 문은 현 지점에서 코드 블록 실행을 중단하고 반복문의 증감식을 실행시킨다.

```jsx
// 문자열은 유사 배열이므로 for 문으로 순회할 수 있다.
for (var i = 0; i < string.length; i++) {
  // 일치 하지 않으면 현 지점에서 실행을 중단하고 반복문의 증감식으로 이동한다.
  if (string[i] !== search) continue;
  count++; // continue 문이 실행되면 이 문은 실행되지 않는다.
}
```

## 새로 알게된 부분

- 레이블문
