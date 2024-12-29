---
layout: single
title: "코어 자바 스크립트 - 07.Class"
typora-root-url: ../
---

## 자바스크립트의 클래스

```javascript
// 생성자
var Rectangle = function (width, height) {
  this.width = width;
  this.height = height;
};
//

// (프로토타입) 메서드
Rectangle.prototype.getArea = function() {
  return this.width * this.heigh;
};
//

//스태틱 메서드
Rectangle.isRectangle = function(instance) {
  return instance instanceof Rectangle &&
    instance.width > 0 && instance.height >0;
};
//

var rect1 = new Rectangle(3, 4);

console.log(rect1.getArea()); // 12 (O)
console.log(rect1.isRectangle(rect1));  // Error (X)
console.log(Rectangle.isRectangle(rect1));  // true
```
- 프로토타입 메서드 : 인스턴스에서 직접 호출할 수 있는 메서드
- 스태틱 메서드 : 인스턴스에서 직접 접근할 수 없는 메서드(생성자 함수를 this로 해야만 호출 가능)


## 클래스 상속
ES5까지의 자바스크립트에는 클래스가 없음. ES6에서 클래스가 도입됐지만 prototype을 기반으로 한것

=> 자바스크립트에서 클래스 상속을 구현했다는 것은 프로토타입 체이닝을 잘 연결한 것으로 이해하면 됨, but 클래스의 추상성을 해치는 경우가 있음

### 클래스가 구체적인 데이터를 지니지 않게 하는 방법
  - 인스턴스 생성 후 프로퍼티 제거
  - 빈 함수를 활용
  - Object.create 활용

위 세가지 방법 과 constructor 복구하기(SubClass.prototype.constructor가 원래의 SubClass를 바라보도록 하기)

## ES6의 클래스 및 클래스 상속
```javascript
var ES5 = function (name) {
  this.name = name;
};

ES5.staticMethod = function () {
  return this.name + ' staticMethod';
};

ES5.prototype.method = function () {
  return this.name + ' method';
}

var es5Instance = new ES5('es5');
console.log(ES5.staticMethod());  // es5 staticMethod
console.log(es5Instance.method());  // es5 method

var ES6 = class {
  constructor(name) {
    this.name = name;
  }
  static staticMethod() {
    return this.name + ' staticMethod';
  }
  method () {
    return this.name + ' method';
  }
};

var es6Instance = new ES6('es6');
console.log(ES6.staticMethod());  // es6 staticMethod
console.log(es6Instance.method());  // es6 method
```

ES5에서는 클래스 상속 문법 자체가 없었지만, ES6에서는 class extends, super 활용

### 정리
- 클래스는 어떤 사물의 공통 속성을 모아 정의한 추상적인 개념이고, 인스턴스는 클래스의 속성을 지니는 구체적인 사례
- 클래스의 prototype 내부에 정의된 메서드를 프로토타입 메서드라고 하며, 이들은 인스턴스가 마치 자신의 것처럼 호출 할 수 있음. 한편, 클래스(생성자 함수)에 직접 정의한 메서드를 스태틱 메서드라고 하며, 이들은 인스턴스가 직접 호출할 수 없고 클래스(생성자 함수)에 의해서만 호출할 수 있음.
- 클래스 상송 흉내내는 방법
  - SubClass.prototype에 SuperClass의 인스턴스 할당한 다음 프로퍼티 모두 삭제
  - 빈 함수(Bridge)를 활용하는 방법
  - Object.create를 이용하는 방법
  - 위 세가지 방법 모두 constructor 프로퍼티가 원래 생성자 함수 바라보도록 조정 필요
- ES5까지는 상속 및 추상화 구현이 복잡한 방법을 사용했지만 ES6 관련 문법 등장으로 손쉽게 사용