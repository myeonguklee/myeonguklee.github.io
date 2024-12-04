---
layout: single
title:  "코어 자바 스크립트 - 02.실행 컨텍스트"
typora-root-url: ../


---

## 실행 컨텍스트 (execution context) : 함수를 실행할 때 필요한 환경정보를 담은 객체

- 전역공간
- **함수**
- module
- (eval)

\* if, for, switch, while .. '문'?
 => 블록 스코프, 별개의 실행컨텍스트 X

call stack : 현재 어떤 함수가 동작중인지, 다음에 어떤 함수가 호출될 예정인지 등을 제어하는 자료 구조

- 실행 컨텍스트
  - VariableEnvironment (현재 환경과 관련된 식별자 정보들, **식별자 정보 수집**, 변화 반영 X)
  - LexicalEnvironment (현재 환경과 관련된 식별자 정보들, **각 식별자의 '데이터' 추적**, 변화 반영 O)
  - ThisBinding


- LexicalEnvironment(어휘적/사전적 환경): 실행 컨텍스트를 구성하는 환경 정보들을 모아 사전처럼 구성한 객체
  - environmentRecord : 현재 문맥의 식별자 정보
  
    => HOISTING : 실행 컨텍스트의 맨 위로 식별자 정보를 끌어올림
  - outerEnvironmentReference : 외부 식별자
  
    => SCOPE CHAIN : '식별자의 유효범위(스코프)'를 안에서부터 바깥으로 차례로 검색해 나가는 것


outerEnvironmentReference 는 해당 함수가 선언된 위치의 LexicalEnvironment 참조

코드 상에서 어떤 변수에 접근하려고 하면
1. 현재 컨텍스트의 LexicalEnvironment를 탐색해서 발견되면 그 값을 반환하고,
2. 발견하지 못할 경우 다시 outerEnvironmentReference에 담긴 LexicalEnvironment를 탐색하는 과정 거침,
3. 전역 컨텍스트의 LexicalEnvironment까지 탐색해도 해당 변수를 찾지 못하면 undefinedf를 반환

```javascript
var a = 1;

var outer = function () {
  var inner = function () {
    console.log(a)  // (1) undefined 
    var a = 3;
  };
  inner();
  console.log(a); // (2) 1
};

outer();
console.log(a); // (3) 1
```


```javascript
var a = 1;
var b = 2;

var outer = function () {
  var inner = function () {
    console.log(b)  // (1) 2 
    var a = 3;
  };
  inner();
  console.log(a); // (2) 1
};

outer();
console.log(a); // (3) 1
```

함수 선언문(function declaration) 과 함수 표현식(function expression)

```javascript
// 함수 선언문. 함수명 a가 변수명
function a () {} 
a(); // 실행 OK.

// (익명) 함수 표현식. 변수명 b가 곧 함수명.
var b = function () {} 
b(); // 실행 OK.

// 기명 함수 표현식. 변수명은 c, 함수명은 d.
var c = function d () {} 
c(); // 실행 OK.
d(); // 에러 !
```


**함수 선언문은 전체를 호이스팅!, 함수 표현식은 변수 선언부만 호이스팅!**
```javascript
console.log(sum(1, 2))
console.log(multiply(3, 4))

// 함수 선언문
function sum (a, b) {
  return a + b;
}

// 함수 표현식
var multiply = function (a, b) {
  return a * b;
}
```
- 호이스팅 마친 상태
```javascript
// 함수 선언문은 전체를 호이스팅
function sum (a, b) {
  return a + b;
}

/// 변수는 선언부만 끌어올림
var multiply;

console.log(sum(1, 2))  // 3
console.log(multiply(3, 4)) // multiply is not a function

// 변수의 할당부는 원래 자리에
multiply = function (a, b) {
  return a * b;
}
```


=> 함수 표현식이 더 안전하다!

왜?

1.함수 선언식은 전체가 호이스팅 되기 때문에 함수를 선언하기 전에 사용할 수도 있고

2.만약 전역 공간에 이름이 같은 함수가 여러 개일 경우 먼저 선언된 함수를 덮어써버릴 수 있다.