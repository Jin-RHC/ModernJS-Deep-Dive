# 9장: 타입 변환과 단축평가

### 몰랐던 것들

- 옵셔널 체이닝 연산자
- `null` 병합 연산자





## 9.1 타입 변환이란?

자바스크립트의 모든 값은 타입이 있다.

- 개발자가 의도적으로 값의 타입을 변환하는 것: 

  `명시적 타입변환(explicit coercion)` 혹은 `타입 캐스팅(type casting)`이라 한다.

- 개발자의 의도와 상관없이 표현식을 평가하는 도중 자바스크립트 엔진에 의해 변환되는 것:

   `암묵적 타입변환(implicit coercion)` 혹은 `타입 강제 변환(type coercion)`이라 한다.



```js
var x = 10;

// 명시적 타입 변환
// 숫자를 문자열로 타입 캐스팅한다.
var str = x.toString();
console.log(typeof str, str); // string 10

// x 변수의 값이 변경된 것은 아니다.
console.log(typeof x, x); // number 10
```



암묵적 타입변환(implicit coercion)은 `타입 강제 변환(type coercion)`이라고도 한다.

```js
var x = 10;

// 암묵적 타입 변환
// 문자열 연결 연산자는 숫자 타입 x의 값을 바탕으로 새로운 문자열을 생성한다.
var str = x + '';
console.log(typeof str, str); // string 10

// x 변수의 값이 변경된 것은 아니다.
console.log(typeof x, x); // number 10
```

명시적 타입변환이나 암묵적 타입변환은 기존 원시 값을 직접 변경하는 것은 아니다. 

*원시 값은 변경 불가능한 값(immutable value)이므로 변경할 수 없다. 타입 변환이란 기존 원시 값을 사용해 다른 타입의 새로운 원시 값을 생성하는 것이다.*



명시적 타입변환은 타입을 변경하겠다는 개발자의 의지가 코드에 드러나지만, 암묵적 타입 강제변환은 자바스크립트 엔진에 의해 암묵적으로, 즉 드러나지 않게 변환되기 때문에 코드에 명백히 나타나지 않는다. 다만 `(10).toString()`보다는 `10 + ''`이 더욱 간결하고 이해하기 쉽듯이, 꼭 암묵적 타입 변환을 억제해야 하는 건 아니다.

중요한 건 코드를 예측할 수 있어야 하고, 동시에 코드가 쉽게 이해돼야 한다는 것이다. 자신이 작성한 코드에서 암묵적 타입 변환이 발생하는지, 발생다면 어떤 타입의 어떤 값으로 변환되는지, 그리고 타입 변환된 값으로 표현식이 어떻게 평가될 것인지 예측 가능해야 한다. 예측하지 못하거나 예측이 결과와 일치하지 않는다면 오류를 생산할 가능성이 높아지기 때문이다.



## 9.2 암묵적 타입 변환

자바스크립트 엔진은 표현식을 평가할 때 개발자의 의도와는 상관없이 코드의 문맥을 고려해 암묵적으로 데이터 타입을 강제 변환할 때가 있다. 암묵적 타입 변환이 발생하면 문자열, 숫자, 불리언과 같은 원시 타입 중 하나로 타입을 자동 변환한다.

```js
// 피연산자가 모두 문자열 타입이어야 하는 문맥
'10' + 2 // -> '102'

// 피연산자가 모두 숫자 타입이어야 하는 문맥
5 * '10' // -> 50

// 피연산자 또는 표현식이 불리언 타입이어야 하는 문맥
!0 // -> true
if (1) { }
```

자바스크립트는 가급적 에러를 발생시키지 않도록 암묵적 타입 변환을 통해 표현식을 평가한다.



### 문자열 타입으로 변환

`+` 연산자를 사용하면 문자열 타입으로 변환된다. `+` 연산자는 피연산자 중 하나 이상이 문자열이면 문자열 연결 연산자로 동작한다.

```js
1 + '2' // -> "12"
```



피연산자만 암묵적 타입 변환의 대상이 되는 건 아니다. 예컨대 ES6에 도입된 템플릿 리터럴의 표현식 삽입도 평가 결과를 문자열 타입으로 암묵적 변환한다.

```js
`1 + 1 = ${1 + 1}` // -> "1 + 1 = 2"
```


문자열 타입으로 암묵적 타입변환 되는 사례

```js
// 숫자 타입
0 + ''         // -> "0"
-0 + ''        // -> "0"
1 + ''         // -> "1"
-1 + ''        // -> "-1"
NaN + ''       // -> "NaN"
Infinity + ''  // -> "Infinity"
-Infinity + '' // -> "-Infinity"

// 불리언 타입
true + ''  // -> "true"
false + '' // -> "false"

// null 타입
null + '' // -> "null"

// undefined 타입
undefined + '' // -> "undefined"

// 심벌 타입
(Symbol()) + '' // -> TypeError: Cannot convert a Symbol value to a string. 에러 발생

// 객체 타입. 모두 결과가 스트링임에 유의!!
({}) + ''           // -> "[object Object]"
Math + ''           // -> "[object Math]"
[] + ''             // -> ""
[10, 20] + ''       // -> "10,20"
(function(){}) + '' // -> "function(){}"
Array + ''          // -> "function Array() { [native code] }"
```



### 숫자 타입으로 변환

산술 연산자중 `+`제외한 연산자 `-`, `*`, `/`, `%` 를 사용하면 숫자 타입으로 변환된다. 문자열과 같이 피연산자를 숫자로 타입 변환할 수 없는 경우엔 표현식의 평가 결과가 `NaN`이 된다.

```js
1 - '1'   // -> 0
1 * '10'  // -> 10
1 / 'one' // -> NaN
13 % '5' // -> 3
```



비교 연산자의 피연산자 중에서 숫자 타입이 아닌 피연산자를 숫자 타입으로 변환한다.

```js
'1' > 0  // -> true
```



`+`단항 연산자를 문자열 앞에 사용하면 숫자 타입의 값으로 변환한다.

```js
// 문자열 타입
+''       // -> 0
+'0'      // -> 0
+'1'      // -> 1
+'string' // -> NaN

// 불리언 타입
+true     // -> 1
+false    // -> 0

// null 타입
+null     // -> 0

// undefined 타입
+undefined // -> NaN

// 심벌 타입
+Symbol() // -> ypeError: Cannot convert a Symbol value to a number

// 객체 타입
+{}             // -> NaN
+[]             // -> 0
+[10, 20]       // -> NaN
+(function(){}) // -> NaN
```



### 불리언 타입으로 변환

`if`문과 같은 제어문 또는 삼항 조건 연산자의 조건식은 불리언 타입으로 암묵적 타입 변환된다.

```js
if ('')    console.log('1');
if (' ')    console.log('2'); // 스페이스 바 -> 빈 문자열이 아니게 됨.
if (true)  console.log('3');
if (0)     console.log('4');
if ('str') console.log('5');
if (null)  console.log('6');

// 2 3 5
```



자바스크립트 엔진은 불리언 타입이 아닌 값을 `Truthy`값(참으로 평가되는 값) 또는 `Falsy`값(거짓으로 평가되는 값)으로 구분한다. 즉, 제어문의 조건식과 같이 불리언 값으로 평가되어야 할 문맥에서 Truthy 값은 `true`로, Falsy 값은 `false`로 암묵적 타입 변환된다.



`false`로 평가되는 Falsy 값

- `false`
- `undefined`
- `null`
- `0`
- `-0`
- `NaN`
- `''`(빈 문자열)
- 

```js
 // 아래의 조건문은 모두 코드 블록을 실행한다.
if (!false)     console.log(false + ' is falsy value');
if (!undefined) console.log(undefined + ' is falsy value');
if (!null)      console.log(null + ' is falsy value');
if (!0)         console.log(0 + ' is falsy value');
if (!NaN)       console.log(NaN + ' is falsy value');
if (!'')        console.log('' + ' is falsy value');
```



**`Falsy`값 이외의 모든 값은 `true`로 평가되는 `Truthy`값이다.**

```js
// 전달받은 인수가 Falsy 값이면 true, Truthy 값이면 false를 반환한다.
function isFalsy(v) {
  return !v;
}

// 전달받은 인수가 Truthy 값이면 true, Falsy 값이면 false를 반환한다.
function isTruthy(v) {
  return !!v;
}

// 모두 true를 반환한다.
isFalsy(false);
isFalsy(undefined);
isFalsy(null);
isFalsy(0);
isFalsy(NaN);
isFalsy('');

// 모두 true를 반환한다.
isTruthy(true);
isTruthy('0'); // 빈 문자열이 아닌 문자열은 Truthy 값이다.
isTruthy({});
isTruthy([]);
```



## 9.3 명시적 타입 변환

개발자의 의도에 따라 명시적으로 타입을 변경하며, 그 방법은 다양하다.
표준 빌트인 생성자 함수(`String`, `Number`, `Boolean`)를 `new`연산자 없이 호출하는 방법, 빌트인 메서드를 사용하는 방법, 그리고 앞에서 살펴본 암묵적 타입 변환을 이용하는 방법이 있다.

>#### 표준 빌트인 생성자 함수와 빌트인 메서드
>
>표준 빌트인(built-in) 생성자 함수와 표준 빌트인 메서드는 자바스크립트에서 기본 제공하는 함수다. 표준 빌트인 생성자 함수는 객체를 생성하기 위한 함수이며 `new` 연산자와 함께 호출한다. 표준 빌트인 메서드는 자바스크립트에서 기본 제공하는 빌트인 객체의 메서드다 21장 "빌트인 객체"에서 자세히 배운다. 
>
>~~`new`가 연산자라는 게 신기하군~~



### 문자열 타입으로 변환

문자열 타입이 아닌 값을 문자열 타입으로 변환하는 방법

- `String` 생성자 함수를 `new` 연산자 없이 호출하는 방법
- `Object.prototype.toString` 메서드를 사용하는 방법
- 문자열 연결 연산자를 이용하는 방법

```js
// 1. String 생성자 함수를 new 연산자 없이 호출하는 방법
// 숫자 타입 => 문자열 타입
String(1);        // -> "1"
String(NaN);      // -> "NaN"
String(Infinity); // -> "Infinity"
// 불리언 타입 => 문자열 타입
String(true);     // -> "true"
String(false);    // -> "false"

// 2. Object.prototype.toString 메서드를 사용하는 방법
// 숫자 타입 => 문자열 타입
(1).toString();        // -> "1"
(NaN).toString();      // -> "NaN"
(Infinity).toString(); // -> "Infinity"
// 불리언 타입 => 문자열 타입
(true).toString();     // -> "true"
(false).toString();    // -> "false"

// 3. 문자열 연결 연산자를 이용하는 방법
// 숫자 타입 => 문자열 타입
1 + '';        // -> "1"
NaN + '';      // -> "NaN"
Infinity + ''; // -> "Infinity"
// 불리언 타입 => 문자열 타입
true + '';     // -> "true"
false + '';    // -> "false"
```



※ 참고(new 연산자 사용 여부에 따라 생성 타입이 달라진다.)

```js
// new 연산자를 사용하지 않으면 원시값이 생성된다.
var primitive = String('a');
typeof primitive // 'string'

// new 연산자와 함께 사용하면 object 타입이 생성된다.
var obj = new String('a');
typeof obj // 'object'
```



### 숫자 타입으로 변환

숫자 타입이 아닌 값을 숫자 타입으로 변환 하는 방법

- `Number` 생성자 함수를 `new`연산자 없이 호출하는 방법
- `parseInt, parseFloat` 함수를 사용하는 방법(문자열만 숫자 타입으로 변환 가능)
- `+` 단항 산술 연산자를 이용하는 방법
- `*` 산술 연산자를 이용하는 방법

```js
 // 1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
// 문자열 타입 => 숫자 타입
Number('0');     // -> 0
Number('-1');    // -> -1
Number('10.53'); // -> 10.53
// 불리언 타입 => 숫자 타입
Number(true);    // -> 1
Number(false);   // -> 0

// 2. parseInt, parseFloat 함수를 사용하는 방법(문자열만 변환 가능)
// 문자열 타입 => 숫자 타입
parseInt('0');       // -> 0
parseInt('-1');      // -> -1
parseFloat('10.53'); // -> 10.53

// 3. + 단항 산술 연산자를 이용하는 방법
// 문자열 타입 => 숫자 타입
+'0';     // -> 0
+'-1';    // -> -1
+'10.53'; // -> 10.53
// 불리언 타입 => 숫자 타입
+true;    // -> 1
+false;   // -> 0

// 4. * 산술 연산자를 이용하는 방법
// 문자열 타입 => 숫자 타입
'0' * 1;     // -> 0
'-1' * 1;    // -> -1
'10.53' * 1; // -> 10.53
// 불리언 타입 => 숫자 타입
true * 1;    // -> 1
false * 1;   // -> 0
```



### 불리언 타입으로 변환

불리언 타입이 아닌 값을 불리언 타입으로 변환하는 방법

- `Boolean`생성자 함수를 `new`연산자 없이 호출하는 방법
- `!`부정 논리 연산자를 두 번 사용하는 방법

```
// 1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
// 문자열 타입 => 불리언 타입
Boolean('x');       // -> true
Boolean('');        // -> false
Boolean('false');   // -> true
// 숫자 타입 => 불리언 타입
Boolean(0);         // -> false
Boolean(1);         // -> true
Boolean(NaN);       // -> false
Boolean(Infinity);  // -> true
// null 타입 => 불리언 타입
Boolean(null);      // -> false
// undefined 타입 => 불리언 타입
Boolean(undefined); // -> false
// 객체 타입 => 불리언 타입
Boolean({});        // -> true
Boolean([]);        // -> true

// 2. ! 부정 논리 연산자를 두번 사용하는 방법
// 문자열 타입 => 불리언 타입
!!'x';       // -> true
!!'';        // -> false
!!'false';   // -> true
// 숫자 타입 => 불리언 타입
!!0;         // -> false
!!1;         // -> true
!!NaN;       // -> false
!!Infinity;  // -> true
// null 타입 => 불리언 타입
!!null;      // -> false
// undefined 타입 => 불리언 타입
!!undefined; // -> false
// 객체 타입 => 불리언 타입
!!{};        // -> true
!![];        // -> true
```



## 9.4 단축 평가

### 논리 연산자를 사용한 단축 평가

논리합 `||` 또는 논리곱 `&&`연산자 표현식의 평가 결과는 불리언 값이 아닐 수도 있다. 언제나 2개의 피연산자 중 어느 한 쪽으로 평가된다.

논리곱 `&&`연산자는 두 개의 피연산자가 모두 `true`로 평가될 때 `true`를 반환한다. 좌항에서 우항으로 평가가 진행된다. 피 연산자가 불리언 타입이 아닐경우 값이 평가된다.

```js
'Cat' && 'Dog' // -> "Dog"
'a' === 'a' && 1 == '1' // -> true
```

위 예제에서 첫 번재 피연산자 `'Cat'`은 `Truthy`값이므로 `true`로 평가된다. 하지만 이 시점까지는 위 표현식을 평가할 수 없다. 두 번째 피연산자 까지 평가해야 표현식을 평가할 수 있다. 따라서 두 번째 피연산자, 즉 `'Dog'`까지 평가하고 평가 결과인 `'Dog'`문자열을 그대로 반환한다.



논리합`||`연산자는 두 개의 피연산자 중 하나만 `true`로 평가되어도 `true`를 반환한다.
좌항에서 우항으로 평가를 진행한다.

```js
'Cat' || 'Dog' // -> "Cat"
```

첫 번째 피연산자 `'Cat'`은 `Truthy`값이므로 `true`로 평가되고, 이 시점에서 두 번째 피연산자까지 평가하지 않아도 표현식이 평가되기 때문에 첫 번째 피연산자 `'Cat'`문자열을 그대로 반환한다.



**논리곱 연산자와 논리합 연산자는 이처럼 논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 반환한다.** 이를 단축 평가(short-circuit evaluation)라 한다. **단축 평가는 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것을 말한다.** 대부분의 프로그래밍 언어는 단축 평가를 통해 논리 연산을 수행한다. ~~설명이 이게 맞는지...?~~



단축 평가는 다음 규칙에 따른다.

|  단축 평가 표현식   | 평가 결과 |
| :-----------------: | :-------: |
| `true`||`anything`  |   true    |
| `false`||`anything` | anything  |
| `true && anything`  | anything  |
| `false && anything` |   false   |

```js
// 논리합(||) 연산자
'Cat' || 'Dog'  // -> "Cat"
false || 'Dog'  // -> "Dog"
'Cat' || false  // -> "Cat"

// 논리곱(&&) 연산자
'Cat' && 'Dog'  // -> "Dog"
false && 'Dog'  // -> false
'Cat' && false  // -> false
```



어떤 조건이 `Truthy`값일 경우 무언가를 해야 한다면 논리곱`&&`연산자 표현식으로 `if`문을 대체할 수 있다.

```js
var done = true;
var message = '';

// 주어진 조건이 true일 때
if (done) message = '완료';

// if 문은 단축 평가로 대체 가능하다.
// done이 true라면 message에 '완료'를 할당
message = done && '완료';
console.log(message); // 완료
```



조건이 `Falsy`값일 경우 논리합`||`연산자 표현식으로 `if`문을 대체할 수 있다.

```js
var done = false;
var message = '';

// 주어진 조건이 false일 때
if (!done) message = '미완료';

// if 문은 단축 평가로 대체 가능하다.
// done이 false라면 message에 '미완료'를 할당
message = done || '미완료';
console.log(message); // 미완료
```



삼항 조건 연산자는 `if...else`문을 대체 할 수 있다.

```js
var done = true;
var message = '';

// if...else 문
if (done) message = '완료';
else      message = '미완료';
console.log(message); // 완료

// if...else 문은 삼항 조건 연산자로 대체 가능하다.
message = done ? '완료' : '미완료';
console.log(message); // 완료
```



단축 평가는 다음과 같은 상황에서 유용하게 사용된다.

#### 객체를 가리키기를 기대하는 변수가 `null`또는 `undefined`가 아닌지 확인하고 프로퍼티를 참조할 때
단축 평가를 사용하면 에러 발생 없이 사용할 수 있다.

```js
// 그냥 참조
var elem = null;
var value = elem.value; // TypeError: Cannot read property 'value' of null


// 단축 평가 사용
var elem = null;
// elem이 null이나 undefined와 같은 Falsy 값이면 elem으로 평가되고
// elem이 Truthy 값이면 elem.value로 평가된다.
var value = elem && elem.value; // -> null
```



#### 함수 매개변수에 기본값을 설정할 때

함수를 호출할 때 인수를 전달하지 않으면 매개변수에는 `undefined`가 할당된다. 이때 단축 평가를 사용해 매개변수의 기본값을 설정하면 `undefined`로 인해 발생할 수 있는 에러를 방지할 수 있다.

```js
// 단축 평가를 사용한 매개변수의 기본값 설정
function getStringLength(str) {
  str = str || '';  // 단축 평가
  return str.length;
}

getStringLength();     // -> 0
getStringLength('hi'); // -> 2


// 참고) ES6의 매개변수 기본값 설정
function getStringLength(str = '') {
  return str.length;
}

getStringLength();     // -> 0
getStringLength('hi'); // -> 2
```



### 옵셔널 체이닝 연산자

ES11(ECMAScript2020)에 도입된 `옵셔널 체이닝(optional chaining)` 연산자 `?.`는 좌항의 피연산자가 `null`또는 `undefined`인 경우 `undefined`를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다. 참고로 프로퍼티 참조란 변수를 통해 변수값을 참조하듯이 객체의 프로퍼티에 접근해 프로퍼티 값을 참조하는 것을 뜻한다. 10장에서 자세히 살펴볼 예정이다.

```js
var elem = null;

// elem이 null 또는 undefined이면 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.
var value = elem?.value;
console.log(value); // undefined
```



옵셔널 체이닝 연산자가 도입되기 전에는 논리곱`&&`을 통한 단축평가를 사용했었다. 하지만 논리곱`&&`연산자는 옵셔널 체이닝 연산자와는 다르게 `Falsy`(`false`, `undefined`, `0`, `NaN`, `''`)값이면 좌항의 피연산자를 그대로 반환하기 때문에 문제가 있었다.

```js
var elem = null;

// elem이 Falsy 값이면 elem으로 평가되고 elem이 Truthy 값이면 elem.value로 평가된다.
var value = elem && elem.value;
console.log(value); // null



var str = '';

// 문자열의 길이(length)를 참조한다.
var length = str && str.length;

// 문자열의 길이(length)를 참조하지 못한다.
console.log(length); // ''
```



하지만 옵셔널 체이닝 연산자`?.`는 좌항의 피연산자가 `false`로 평가되는 `Falsy`(`false`, `undefined`, `0`, `NaN`, `' '`)값이라도 `null` 또는 `undefined`가 아니면 우항의 프로퍼티 참조를 이어간다.

```js
var str = '';

// 문자열의 길이(length)를 참조한다. 이때 좌항 피연산자가 false로 평가되는 Falsy 값이라도
// null 또는 undefined가 아니면 우항의 프로퍼티 참조를 이어간다.
var length = str?.length;
console.log(length); // 0
```



### null 병합 연산자

ES11(ECMAScript2020)에 도입된 `null 병합(nullish coalescing)`연산자 `??`는 좌항의 피연산자가 `null` 또는 `undefined`인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.

```js
// 좌항의 피연산자가 null 또는 undefined이면 우항의 피연산자를 반환하고, 
// 그렇지 않으면 좌항의 피연산자를 반환한다.
var foo = null ?? 'default string';
console.log(foo); // "default string"
```



`null` 병합 연산자는 변수에 기본값을 설정할 때 유용하다. `null` 병합 연산자가 도입되기 이전에는 논리합`||`연산자를 사용했는데, 논리곱`&&`연산자와 동일하게 모든 좌항의 피연산자가 `Falsy`(`false`, `undefined`, `0`, `NaN`, `' '`)값인 경우 우항의 피연산자를 반환하여 예기치 않은 동작이 발생할 수 있다.

```js
// Falsy 값인 0이나 ''도 기본값으로서 유효하다면 예기치 않은 동작이 발생할 수 있다.
var foo = '' || 'default string';
console.log(foo); // "default string"


// 좌항의 피연산자가 Falsy 값이라도 null 또는 undefined이 아니면 좌항의 피연산자를 반환한다.
var foo = '' ?? 'default string';
console.log(foo); // ""
```

옵셔널 체이닝과 마찬가지로 null 병합 연산자 `??`는 좌항의 피연산자가 `false`로 평가되는 `Falsy`(`false`, `undefined`, `0`, `NaN`, `' '`)값이라도 `null` 또는 `undefined`가 아니면 우항의 프로퍼티 참조를 이어간다.