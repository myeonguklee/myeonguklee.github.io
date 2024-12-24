---
layout: single
title: "코어 자바 스크립트 - 03.This"
typora-root-url: ../
---

### 상황에 따라 달라지는 this

- 전역 공간에서 => window(브라우저) / global(node.js)

  - "전역변수를 선언하면 자바스크립트 엔진은 이를 전역객체의 프로퍼티로 할당한다" + a를 직접 호출 = 변수 a에 접근하고자 하면 스코프 체인에서 a를 검색하다가 가장 마지막에 도달하는 전역 스코프의 Lexical Environment, 즉 전역 객체에서 해당 프로퍼티 a를 발견해서 그 값을 반환

  - Delete(삭제)를 할 때는 전역변수 선언과 전역객체의 프로퍼티 할당이 다르게 동작

    - 전역변수 선언하면 자바스크립트 엔진이 이를 자동으로 전역객체의 프로퍼티로 할당하면서 추가적으로 해당 프로퍼티의 configurable 속성(변경 및 삭제 가능성)을 false로 정의하기 때문

  - var로 선언한 전역변수와 전역객체의 프로퍼티는 호이스팅 여부 및 configurable 여부에서 차이를 보여줌

- 함수 호출시 => window / global

- 메서드 호출시 => 메서드 호출 주체 (메서드명의 . or [] 앞)

```javascript
// 함수로서 호출, 메서드로서 호출

var func = function (x) {
  console.log(this, x);
};

func(1); // Window { ... } 1

var obj = {
  method: func,
};

obj.method(1); // { method: f } 1
obj["method"](2); // { method: f } 2
```

```javascript
// 내부 함수에서 this 우회하는 방법
var obj = {
  outer: function () {
    console.log(this); // (1) { outer: f }
    var innerFunc1 = function () {
      console.log(this); // (2) window { ... }
    };
    innerFunc1();

    var self = this;
    var innerFunc2 = function () {
      console.log(self); // (3) { outer: f }
    };
    innerFunc2();
  },
};

obj.outer();

// ES6에서 this를 바인딩하지 않는 함수
// => 화살표 함수는 실행 컨텍스트 생성시 this 바인딩 과정 자체가 빠져, 상위 스코프의 this를 그대로 활용

var obj = {
  outer: function () {
    console.log(this); // (1) { outer: f }
    var innerFunc = () => {
      console.log(this); // (2) { outer: f }
    };
    innerFunc1();
  },
};

obj.outer();
```

- callback 호출시 => 기본적으로는 함수내부에서와 동일

  - 기본적으로는 함수의 this와 같다.
  - 제어권을 가진 함수가 콜백의 this를 지정해둔 경우도 있다.
  - 이 경우에도 개발자가 this를 바인딩해서 콜백을 넘기면 그에 따른다.

- 생성자함수(new) 호출시 => 인스턴스

\*함수의 제어권을 다른 함수(또는 메서드) B에게 넘겨주는 경우 함수 A를 **콜백 함수**라고 함 ex. setTimeout

---

### **명시적으로 this를 바인딩 하는 법** 메서드(call, apply, bind 메서드 등)
  - call : 메서드의 호출 주체인 함수를 즉시 실행하도록 하는 명령, call 메서드의 첫 번째 인자를 this로 바인딩, 이후 인자들을 호출할 함수의 매개 변수로! + 함수를 그냥 실행하면 this는 전역 객체를 참조하지만 **call 메서드를 이용하면 임의의 객체를 this로 지정 가능!**

  - apply : call 메서드와 기능적으로 완전 동일, but call 메서드는 첫 번째 인자를 제외한 나머지 모든 인자들을 호출할 함수의 매개변수로 지정, apply 메서드는 두 번째 인자를 배열로 받아 그 배열의 요소들을 호출할 함수의 매개변수로 지정

  - call/apply 메서드 활용 : 유사배열객체에 배열 메서드 적용, 생성자 내부에서 다른 생성자를 호출, 여러 인수를 묶어 하나의 배열로 전달하고 싶을 때(apply)

  - bind : ES5에서 추가, call과 비슷하지만 즉시 호출하지는 않고 넘겨 받은 this 및 인수들을 바탕으로 새로운 함수를 반환하기만 하는 메서드, 다시 새로운 함수를 호출할 때 인수를 넘기면 그 인수들은 기존 bind 메서드를 호출할 때 전달했던 인수들의 뒤에 이어서 등록. 즉 bind 메서드는 함수에 this를 미리 적용하는 것과 부분 적용 함수를 구현하는 두 가지 목적을 지님.

  - ES6의 화살표 함수

  - 별도의 인자로 this를 받는 경우(콜백 함수 내에서의 this)

```javascript
function a(x, y, z) {
  console.log(this, x, y, z);
}

var b = {
  bb: "bbb",
};

a.call(b, 1, 2, 3); // {bb: 'bbb'} 1 2 3
a.apply(b, [1, 2, 3]); // {bb: 'bbb'} 1 2 3

var c = a.bind(b);
c(1, 2, 3); // {bb: 'bbb'} 1 2 3

var d = a.bind(b, 1, 2);
d(2); // {bb: 'bbb'} 1 2 3
```

---

### 정리

- 규칙 (명시적 this 바인딩이 없는 한 늘 성립)
  - 전역공간에서의 this는 전역객체(브라우저에서는 window, Node.js에서는 global)를 참조
  - 어떤 함수를 메서드로서 호출한 경우 this는 메서드 호출 주체(메서드명 앞의 객체)를 참조
  - 어떤 함수를 함수로서 호출한 경우 this는 전역객체를 참조, 메서드의 내부함수도 도일
  - 콜백 함수 내부에서의 this는 해당 콜백 함수의 제어권을 넘겨받은 함수가 정의한 바에 따르며, 정의하지 않은 경우에는 전역객체 참조
  - 생성자 함수에서의 this는 생성될 인스턴스를 참조

- 명시적 this 바인딩
  - call, apply 메서드는 this를 명시적으로 지정하면서 함수 또는 메서드를 호출
  - bind 메서드는 this 및 함수에 넘길 인수를 일부 지정해서 새로운 함수
  - 요소를 순회하면서 콜백 함수를 반복 호출하는 내용의 일부 메서드는 별도의 인자로 this를 받기도 함