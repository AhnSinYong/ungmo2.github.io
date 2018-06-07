---
layout: post
title: Javascript <strong>Data type & Variable</strong>
subtitle: 자료형과 변수
categories: javascript
section: javascript
description: 자료형(Data Type)은 프로그래밍 언어에서 객체, 정수, 불린 등 여러 종류의 데이터를 식별하는 분류를 말한다. 모든 프로그래밍 언어의 학습은 자료형을 파악하는 것으로부터 시작된다. 프로그래밍은 변수를 통해 값을 저장하고 참조하며 연산자로 값을 연산, 평가하고 조건문과 반복문에 의한 흐름제어로 데이터의 흐름을 제어하고 함수로 구문의 집합을 만들며 객체, 배열 등으로 자료를 구조화하는 것이다. 변수는 위치(주소)를 기억하는 저장소이다. 위치란 메모리 상의 주소(address)를 의미한다. 즉, 변수란 메모리 주소(Memory address)에 접근하기 위해 사람이 이해할 수 있는 언어로 지정한 식별자(identifier)이다.
---

* TOC
{:toc}

프로그래밍은 변수를 통해 값을 저장하고 참조하며 연산자로 값을 연산, 평가하고 조건문과 반복문에 의한 흐름제어로 데이터의 흐름을 제어하고 함수로 재사용이 가능한 구문의 집합을 만들며 객체, 배열 등으로 자료를 구조화하는 것이다.

변수는 위치(주소)를 기억하는 저장소이다. 위치란 메모리 상의 주소(address)를 의미한다. 즉, 변수란 메모리 주소(Memory address)에 접근하기 위해 사람이 이해할 수 있는 언어로 지정한 식별자(identifier)이다.

![memory_address](./img/memory_address.png)

변수 선언과 할당의 구조
{: .desc-img}

변수(memory address에 접근하기 위한 식별자)를 통해 메모리에 값을 저장하기 위해서는 먼저 확보해야 할 메모리의 크기([byte](https://ko.wikipedia.org/wiki/바이트))를 알아야한다. 이는 값의 종류에 따라 확보해야 할 메모리의 크기가 다르기 때문이다. 이때 값의 종류, 즉 데이터의 종류를 자료형(Data Type)이라 한다.

예를 들어 1byte(8bit)로 표현할 수 있는 경우의 수, 즉 값의 총 개수는 256개(2<sup>8</sup>)로 [아스키코드(ASCII)](https://ko.wikipedia.org/wiki/ASCII)를 표현할 수 있으며, 4byte(32bit)로 표현할 수 있는 값의 총수는 4,294,967,296개(2<sup>32</sup>)로 -2,147,483,648 ~ 2,147,483,647의 정수를 표현할 수 있다.

C나 Java같은 C-family 언어는 Static Typing(정적 타이핑) 언어로 변수 선언 시 변수에 저장할 값의 종류에 따라 사전에 자료형을 지정(Type annotation)하여야 한다. 다음은 C에서 정수형 변수를 선언하는 예이다.

```c
// 1byte 정수형: -128 ~ 127
char c;

// 4byte 정수형: -2,124,483,648 ~ 2,124,483,647
int num;
```

![int num](./img/int_num.png)

변수 선언과 메모리의 확보
{: .desc-img}

C 언어의 경우, 4byte 정수형인 int형 변수 선언을 만나면 시스템은 이후 할당될 값과는 상관없이 4byte의 메모리 영역을 확보한다. 이후 int형 변수에 할당할 때에는 int형 값을 할당해야한다. 다음은 C에서 정수형 변수에 문자열을 잘못 할당한 예이다.

```c
int main(void) {
  int num = 46;
  char * str = "String";

  num = "String"; // warning: incompatible pointer to integer conversion assigning to 'int' from 'char [7]'

  return 0;
}
```

자바스크립트는 동적 타이핑(Dynamic Typing) 언어로 변수의 Type annotation이 필요없이 값이 할당되는 과정에서 자동으로 변수의 자료형이 결정(타입 추론, Type Inference)된다. 따라서 같은 변수에 여러 자료형의 값을 할당할 수 있다.

```javascript
var str  = 'Hello';
var num  = 1;
var bool = true;

var foo = 'string';
console.log(typeof foo); // string
foo = 1;
console.log(typeof foo); // number
```

자바스크립트에는 어떠한 자료형이 있는지 그리고 변수는 어떻게 사용하는지 알아보도록 하자.

# 1. Data Type (자료형)

자료형(Data Type)은 프로그래밍 언어에서 문자열, 숫자, 불리언, 객체 등 여러 종류의 데이터를 식별하는 분류를 말한다. 모든 프로그래밍 언어의 학습은 자료형을 파악하는 것으로부터 시작된다.

ECMAScript 표준(ECMAScript 2015 (6th Edition, ECMA-262) / 2015.06)은 7개의 Data type을 정의한다

* 기본 자료형 (primitive data type)
  * `Boolean`
  * `null`
  * `undefined`
  * `Number`
  * `String`
  * `Symbol` (ECMAScript 6에서 추가)
* 객체형 (Object type, Reference type)
  * `Object`

Javascript의 자료형은 크게 기본 자료형(primitive data type)과 객체형(reference type)으로 구분할 수 있다.

## 1.1 기본자료형 (Primitive Data Type)

기본 자료형의 값은 [변경 불가능한 값(immutable value)](./js-immutability)이며 **[pass-by-value(값에 의한 전달)](./js-object#5-pass-by-value)** 이다. 또한 이들 값은 메모리의 스택 영역(Stack Segment)에 고정된 메모리 영역을 점유하고 저장된다.

### 1.1.1 Boolean

논리적인 요소를 나타내며 `true`와 `false` 두가지 값을 가질 수 있다. 비어있는 문자열과 `null`, `undefined`, 숫자 0은 `false`로 간주된다.

```javascript
var foo = true;
var bar = false;
```

### 1.1.2 null

null 타입은 딱 한 가지 값, `null`을 가질 수 있다. 자바스크립트는 case-sensitive하므로 `null`은 Null, NULL등과 다르다.

Computer science에서 `null`은 의도적으로 기본자료형 또는 객체형 변수에 값이 없다는 것을 명시한 것이다. 이는 변수와 메모리 어드레스의 참조 정보를 제거하는 것을 의미하며 자바스크립트 엔진은 참조가 없어진 메모리 영역에 대해 [가비지 콜렉션](https://developer.mozilla.org/ko/docs/Web/JavaScript/Memory_Management)을 수행할 것이다.

```javascript
var foo = 'Lee';
foo = null;  // 참조 정보가 제거됨
```

주의할 것은 자료형을 나타내는 문자열을 반환하는 typeof 연산자로 null값은 가진 변수를 연산해 보면 null이 아닌 object가 나온다. 이는 설계상의 오류이다.

```javascript
var foo = null;
console.log(typeof foo); // object
```

따라서 null 타입 변수인지 확인할 때 typeof 연산자를 사용하면 안되고 일치 연산자(===)를 사용하여야 한다.

```javascript
var foo = null;
console.log(typeof foo === null); // false
console.log(foo === null);        // true
```

### 1.1.3 undefined

선언 이후 값을 할당하지 않은 변수는 `undefined` 값을 가진다. 즉, 선언은 되었지만 할당된 적이 없는 변수에 접근하거나 존재하지 않는 객체 프로퍼티에 접근할 경우 반환된다.

```javascript
var foo;
console.log(foo); // undefined

var bar = {
  name: 'Lee',
  gender: 'male'
};
console.log(bar);     // { name: 'Lee', gender: 'male' }
console.log(bar.baz); // undefined
```

### 1.1.4 Number

C 언어의 경우, 정수형과 실수형을 구분하여 int, long, float, double 등과 같은 다양한 숫자 자료형이 존재한다. 하지만 자바스크립트는 하나의 숫자 자료형만 존재한다.

ECMAScript 표준에 따르면, 숫자 자료형은 배정밀도 64비트 부동소수점 형([double-precision 64-bit floating-point format](https://en.wikipedia.org/wiki/Double-precision_floating-point_format) : -(2<sup>53</sup> -1) 와 2<sup>53</sup> -1 사이의 숫자값) 단 하나만 존재한다. 정수만을 표현하기 위한 특별한 자료형(integer type)은 없다.

추가적으로 세가지 의미있는 기호적인 값들도 표현할 수 있다.

* `+/- Infinity`
* `NaN` (not-a-number)

```javascript
var x = 10;    // 정수
var y = 10.12; // 실수
var z = -20;   // 음의 정수

var foo = 42 / -0;
console.log(foo);        // -Infinity
console.log(typeof foo); // number

var bar = 1 * 'string';
console.log(bar);        // NaN
console.log(typeof bar); // number
```

수학적 의미의 실수(實數, real number)는 허수(虛數, imaginary number)가 아닌 유리수와 무리수를 통틀은 말이지만 프로그래밍 언어에서 실수는 일반적으로 소수를 가리킨다.

### 1.1.5 String

String(문자열) 타입은 텍스트 데이터를 나타내는데 사용한다. 이는 0개 이상의 유니코드(16비트 부호없는 정수 값) 문자들의 집합이다. 문자열은 작은 따옴표('') 또는 큰 따옴표("") 안에 텍스트를 넣어 생성한다.

```javascript
var str = "string";      // double quotes
    str = 'string';      // single quotes
console.log(typeof str); // string

var answer = "It's alright";        // Single quote inside double quotes
    answer = "He is called 'John'"; // Single quotes inside double quotes
    answer = 'He is called "John"'; // Double quotes inside single quotes
```

C와 같은 언어와는 다르게, 자바스크립트의 문자열은 기본자료형으로 변경 불가능(immutable)하다. 이것은 한 번 문자열이 생성되면, 그 문자열을 변경할 수 없다는 것을 의미한다. 아래의 코드를 살펴보자.

```javascript
var str = 'Hello';
str = 'world';
```

첫번째 구문이 실행되면 메모리에 문자열 'Hello'가 생성되고 식별자 str은 메모리에 생성된 문자열 'Hello'의 메모리 주소를 가리킨다. 그리고 두번째 구문이 실행되면 이전에 생성된 문자열 'Hello'을 수정하는 것이 아니라 새로운 문자열 'world'를 메모리에 생성하고 식별자 str은 이것을 가리킨다. 이때 문자열 'Hello'와 'world'는 모두 메모리에 존재하고 있다. 변수 str은 문자열 'Hello'를 가리키고 있다가 문자열 'world'를 가리키도록 변경되었을 뿐이다.

```javascript
var str = 'string';
// 문자열은 유사배열이다
console.log(str[0], str[1], str[2], str[3], str[4], str[5]);

str[0] = 'S';
console.log(str); // string
```

문자열은 배열처럼 인덱스를 통해 접근할 수 있다. 이와 같은 특성을 갖는 데이터를 **유사 배열**이라 한다.

`str[0] = 'S'`처럼 이미 생성된 문자열의 일부 문자를 변경해도 반영되지 않는다(이때 에러가 발생하지 않는다). 한번 생성된 문자열은 read only로서 변경할 수 없다. 이것을 변경 불가능(immutable)이라 한다.

그러나 새로운 문자열을 재할당하는 것은 물론 가능하다. 이는 기존 문자열을 변경하는 것이 아니라 새로운 문자열을 새롭게 할당하는 것이기 때문이다.

```javascript
var str = 'string';
console.log(str); // string

str = 'String';
console.log(str); // String

str += ' test';
console.log(str); // String test

str = str.substring(0, 3);
console.log(str); // Str

str = str.toUpperCase();
console.log(str); // STR
```

### 1.1.6 Symbol

[Symbol](./es6-symbol)은 ES6에서 새롭게 추가된 7번째 타입이다. Symbol은 애플리케이션 전체에서 유일하며 변경 불가능한(immutable) 기본 자료형(primitive)이다. 주로 객체의 프로퍼티 키(property key)를 생성할 때 사용한다. Symbol 값은 애플리케이션 전역에서 유일하기 때문에 Symbol 값을 키로 갖는 프로퍼티는 다른 어떠한 프로퍼티와도 충돌하지 않는다.

```javascript
var key = Symbol('key');
console.log(typeof key); // symbol

var obj = {};
obj[key] = 'value';
console.log(obj[key]); // value
```

## 1.2 객체형 (Object type, Reference type)

[객체](./js-object)는 데이터와 그 데이터에 관련한 동작(절차, 방법, 기능)을 모두 포함할 수 있는 개념적 존재이다. 달리 말해, 이름과 값을 가지는 데이터를 의미하는 프로퍼티(property)와 동작을 의미하는 메소드(method)를 포함할 수 있는 독립적 주체이다.

자바스크립트는 객체(object) 기반의 스크립트 언어로서 자바스크립트를 이루고 있는 거의 "모든 것"이 객체이다. 기본자료형(Primitives)을 제외한 나머지 값들(배열, 함수, 정규표현식 등)은 모두 객체이다. 또한 객체는 <strong>[pass-by-reference(참조에 의한 전달)](./js-object#4-pass-by-reference)</strong>이며 메모리의 힙 영역(Heap Segment)에 저장된다.

# 2. 변수 (Variable)

애플리케이션에서 값(value)을 유지할 필요가 있을 때 변수를 사용한다. 변수는 위치(주소)를 기억하는 저장소이다. 위치란 메모리 상의 주소(address)를 의미한다. 즉, 변수란 메모리 주소(Memory address)에 접근하기 위해 사람이 이해할 수 있는 언어로 지정한 식별자(identifier)이다.

변수는 값(value)을 저장(할당), 참조하기 위해 사용된다. 한번 쓰고 버리는 값이 아닌 유지할 필요가 있는 값의 경우, 변수를 사용한다.

변수는 다른 사용자가 변수의 존재 목적을 쉽게 이해할 수 있도록 의미있는 이름을 지정하여야한다.

```javascript
var x = 3;        // NG
var score = 100;  // OK
```

변수명은 식별자(identifier)로 불리기도 하며 명명 규칙이 존재한다.

* 반드시 영문자(특수문자 제외), underscore ( _ ), 또는 달러 기호($)로 시작하여야 한다. 이어지는 문자에는 숫자(0~9)도 사용할 수 있다.
* 자바스크립트는 대/소문자를 구별하므로 사용할 수 있는 문자는 "A" ~ "Z" (대문자)와 "a" ~ "z" (소문자)이다.

변수를 선언할 때 `var` keyword가 사용된다. 등호(=, equal sign)는 변수에 값을 할당하기 위해 사용된다.

```javascript
var name;     // 선언
name = 'Lee'; // 할당

var age = 30; // 선언과 할당

var person = 'Lee',
    address = 'Seoul',
    price = 200;

var price = 10;
var tax   = 1;
var total = price + tax;
```

값을 할당하지 않은 변수 즉 선언만 되어 있는 변수는 `undefined`로 초기값을 갖게 된다. 미선언 변수에 접근하면 `ReferenceError`가 발생한다.

```javascript
var x;
console.log(x); // undefined
console.log(y); // ReferenceError
```

## 2.1 변수의 중복 선언

변수는 중복 선언이 가능하다.

```javascript
var x = 1;
console.log(x); // 1

// 변수의 중복 선언
var x = 100;
console.log(x); // 100
```

변수의 중복 선언은 문법적으로 허용되지만 의도하지 않게 변수의 값을 변경할 수 있으므로 사용하지 않는 것이 좋다.

## 2.2 변수 선언 시 var 키워드 생략 허용

변수 선언 시 var 키워드를 생략할 수 있다. 이때 변수는 전역 변수가 된다.

```javascript
x = 1;
console.log(x); // 1
```

var 키워드의 생략은 문법적으로 허용되지만 의도하지 않게 변수를 전역화할 수 있으므로 사용하지 않는 것이 좋다.

## 2.3 동적 타이핑 (Dynamic Typing)

자바스크립트는 동적 타입(dynamic typed) 언어 혹은 느슨한 타입(loosely typed) 언어이다. 이것은 변수의 Type annotation이 필요없이 값이 할당되는 과정에서 자동으로 자료형이 결정(Type Inference)될 것이라는 뜻이다. 따라서 같은 변수에 여러 자료형의 값을 할당할 수 있다. 이를 동적 타이핑(Dynamic Typing)이라 한다.

```javascript
var foo;

console.log(typeof foo);  // undefined

foo = null;
console.log(typeof foo);  // object

foo = {};
console.log(typeof foo);  // object

foo = 3;
console.log(typeof foo);  // number

foo = 3.14;
console.log(typeof foo);  // number

foo = 'Hi';
console.log(typeof foo);  // string

foo = true;
console.log(typeof foo);  // boolean
```

## 2.4 변수 호이스팅(Variable Hoisting)

아래의 예제를 살펴보자.

```javascript
console.log(foo); // ① undefined
var foo = 123;
console.log(foo); // ② 123
{
  var foo = 456;
}
console.log(foo); // ③ 456
```

var 키워드를 사용하여 선언한 변수는 중복 선언이 가능하기 때문에 위의 코드는 문법적으로 문제가 없다.

①에서 변수 foo는 아직 선언되지 않았으므로 ReferenceError: foo is not defined가 발생할 것을 기대했겠지만 콘솔에는 undefined가 출력된다.

이것은 다른 C-family 언어와는 차별되는 자바스크립트의 특징으로 <strong>모든 선언문은 호이스팅(Hoisting)되기 때문</strong>이다.

호이스팅이란 var 선언문이나 function 선언문 등 모든 선언문이 해당 [Scope](./js-scope)의 선두로 옮겨진 것처럼 동작하는 특성을 말한다. 즉, 자바스크립트는 모든 선언문(var, let, const, function, [function*](./es6-generateor), class)이 선언되기 이전에 참조 가능하다.

변수가 어떻게 생성되며 호이스팅은 어떻게 이루어지는지 좀더 자세히 살펴보자. 변수는 3단계에 걸쳐 생성된다. 자세한 내용은 [Execution Context](./js-execution-context)을 참조하기 바란다.

선언 단계(Declaration phase)
: 변수 객체(Variable Object)에 변수를 등록한다. 이 변수 객체는 스코프가 참조하는 대상이 된다.

초기화 단계(Initialization phase)
: 변수 객체(Variable Object)에 등록된 변수를 메모리에 할당한다. 이 단계에서 변수는 undefined로 초기화된다.

할당 단계(Assignment phase)
: undefined로 초기화된 변수에 실제값을 할당한다.

var 키워드로 선언된 변수는 선언 단계와 초기화 단계가 한번에 이루어진다. 즉, 스코프에 변수가 등록되고 변수는 메모리에 공간을 확보한 후 undefined로 초기화된다. 따라서 변수 선언문 이전에 변수에 접근하여도 Variable Object에 변수가 존재하기 때문에 에러가 발생하지 않는다. 다만 undefined를 반환한다. 이러한 현상을 변수 호이스팅(Variable Hoisting)이라한다.

이후 변수 할당문에 도달하면 비로서 값의 할당이 이루어진다.

![var lifecycle](./img/var-lifecycle.png)
{: .w-450}

var 키워드로 선언된 변수의 생명 주기
{: .desc-img}

앞에서 살펴본 예제를 호이스팅 관점에서 다시 한번 알아보도록 하자.

①이 실행되기 이전에 `var foo = 123;`이 호이스팅되어 ①구문 앞에 `var foo;`가 옮겨진다.(실제로 변수 선언이 코드 레벨로 옮겨진 것은 아니고 변수 객체(Variable object)에 등록되고 undefined로 초기화된 것이다.) 하지만 변수 선언 단계와 초기화 단계가 할당 단계와 분리되어 진행되기 때문에 이 단계에서는 foo에는 undefined가 할당되어 있다. 변수 foo에 값이 할당되는 것은 2행에서 실시된다.

②에서는 변수에 값이 할당되었기 때문에 123이 출력된다.

자바스크립트의 변수는 다른 C-family와는 달리 <strong>블록 레벨 스코프(block-level scope)</strong>를 가지지 않고 <strong>함수 레벨 스코프(function-level scope)</strong>를 갖는다. 단, ECMAScript 6에서 도입된 [let, const](./es6-block-scope) 키워드를 사용하면 블록 레벨 스코프를 사용할 수 있다. 자세한 내용은 [Scope](./js-scope)를 참조하기 바란다.

함수 레벨 스코프(Function-level scope)
: 함수 내에서 선언된 변수는 함수 내에서만 유효하며 함수 외부에서는 참조할 수 없다. 즉, 함수 내부에서 선언한 변수는 지역 변수이며 함수 외부에서 선언한 변수는 모두 전역 변수이다.

블록 레벨 스코프(Block-level scope)
: 코드 블록 내에서 선언된 변수는 코드 블록 내에서만 유효하며 코드 블록 외부에서는 참조할 수 없다.

따라서 코드 블록 내의 변수 foo는 전역변수이므로 전역에 선언된 변수 foo에 할당된 값을 재할당하기 때문에 ③의 결과는 456이 된다.

## 2.5 var 키워드로 선언된 변수의 문제점

ES5에서 변수를 선언할 수 있는 유일한 방법은 var 키워드를 사용하는 것이다. var 키워드로 선언된 변수는 아래와 같은 특징을 갖는다. 이는 다른 C-family 언어와는 차별되는 특징(설계상 오류)으로 주의를 기울이지 않으면 심각한 문제를 발생시킨다.

1. [함수 레벨 스코프(Function-level scope)](./js-scope#3-function-scope)
  - 전역 변수의 남발
  - for loop 초기화식에서 사용한 변수를 for loop 외부 또는 전역에서 참조할 수 있다.
2. var 키워드 생략 허용
  - 의도하지 않은 변수의 전역화
3. 중복 선언 허용
  - 의도하지 않은 변수값 변경
4. 변수 호이스팅
  - 변수를 선언하기 전에 참조가 가능하다.

대부분의 문제는 전역 변수로 인해 발생한다. 전역 변수는 간단한 애플리케이션의 경우, 사용이 편리한 면이 있지만 불가피한 상황을 제외하고 사용을 억제해야 한다. 전역 변수는 유효 범위(scope)가 넓어서 어디에서 어떻게 사용될 지 파악하기 힘들다. 이는 의도치 않은 변수의 변경이 발생할 수 있는 가능성이 증가한다. 또한 여러 함수와 상호 의존하는 등 부수 효과(side effect)가 있을 수 있어서 복잡성이 증가한다.

변수의 유효 범위(scope)는 좁을수록 좋다.

ES6는 이러한 var의 단점을 보완하기 위해 [let과 const 키워드](./es6-block-scope)를 도입하였다.

# Reference

* [자바스크립트의 메모리관리](https://developer.mozilla.org/ko/docs/Web/JavaScript/Memory_Management)

* [자바스크립트는 어떻게 작동하는가](https://engineering.huiseoul.com/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%9E%91%EB%8F%99%ED%95%98%EB%8A%94%EA%B0%80-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EA%B4%80%EB%A6%AC-4%EA%B0%80%EC%A7%80-%ED%9D%94%ED%95%9C-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EB%88%84%EC%88%98-%EB%8C%80%EC%B2%98%EB%B2%95-5b0d217d788d)
