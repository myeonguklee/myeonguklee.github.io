---
layout: single
title: "코어 자바 스크립트 - 04.Callback Function"
typora-root-url: ../
---

### Callback function(콜백 함수) : 다른 코드의 인자로 제어권을 넘겨주는 함수
  - 실행 시점 -> setInterval
  
  ```javascript
  var cb = function () {
    console.log('1초마다 실행될 겁니다.');
  };

  setInterval(cb, 1000);
  // setInterval( callback, milliseconds )
  ```


  - 매개변수 -> forEach

  ```javascript
  var arr = [1, 2, 3, 4, 5];
  var entries = [];

  arr.forEach(function(v, i) {
    entries.push([i, v, this[i]]);
  }, [10, 20, 30, 40, 50]);
  
  console.log(entries);

  // [[0, 1, 10], [1, 2, 20], [2, 3, 30], [3, 4, 40], [4, 5, 50]]
  // 콜백 함수의 첫 번째 인자 = 배열의 요소 중 현재값, 두 번째 인자 = 현재값의 인덱스, 세 번째 인자 = 메서드의 대상이 되는 배열 자체
  ```


  - this -> addEventListener
  
  ```javascript
  document.body.innerHTML = '<div id="a">abc</div>';
  function cbFunc(x) {
    console.log(this, x);
  }

  document.getElementById('a').addEventListener('click', cbFunc);

  /// <div id="a">abc</div> PointerEvent {isTrusted: true, pointerId: 1, width: 1, height: 1, ... }
  ```


콜백 함수의 특징
  - 다른 함수(A)의 인자로 콜백함수(B)를 전달하면, A가 B의 제어권을 갖게 된다.
  - **특별한 요청(bind)이 없는 한** A에 미리 정해놓은 방식(어떤 시점에 콜백을 호출할지, 인자에는 어떤 값들을 지정할지, this에 무엇ㅇ르 바인딩할지 등)에 따라 B를 호출한다.


주의
  - 콜백 함수는은 함수이다!
  ```javascript
  var arr = [1, 2, 3, 4, 5];
  var obj = {
    vals: [1, 2, 3],
    logValues: function(v, i) {
      if(this.vals) {
        console.log(this.vals, v, i);
      } else {
        console.log(this, v, i);
      }
    }
  };

  // 메소드로 호출
  obj.logValues(1, 2);  // [1, 2, 3] 1 2

  // 콜백함수로 전달
  arr.forEach(obj.logValues); // window { ... } 1 0 ... window { ... } 5 4

  // obj 지정하고 싶을 때
  // arr.forEach(obj.logValues.bind(obj));
  // arr.forEach(obj.logValues, obj);

  ```

콜백지옥 : 콜백 함수를 익명 함수로 전달하는 과정이 반복되어 코드의 들여쓰기 수준이 감당하기 힘들 정도로 깊어지는 현상

```javascript
setTimeout(function (name){
  var coffeeList = name;
  console.log(coffeeList);

  setTimeout(function (name) {
    coffeeList += ', ' + name;
    console.log(coffeeList);

    setTimeout(function (name) {
      coffeeList += ', ' + name;
      console.log(coffeeList);

      setTimeout(function (name) {
        coffeeList += ', ' + name;
        console.log(coffeeList);
      }, 500, '카페라떼');
    }, 500, '카페모카');
  }, 500, '아메리카노');
}, 500, '에스프레소')
```

  - **해결 방안**

```javascript
// 기명함수로 변환
var coffeeList = '';

var addEspresso = function(name) {
  coffeeList = name;
  console.log(coffeeList);
  setTimeout(addAmericno, 500, '아메리카노');
}

var addAmericano = function(name) {
  coffeeList += ', ' + name;
  console.log(coffeeList);
  setTimeout(addMocha, 500, '카페모카');
} // ...


// 비동기 작업의 동기적 표현 - Promise
// new Promis, resolve, then 활용

// 비동기 작업의 동기적 표현 - Generator
// *, yield, next 활용

// 비동기 작업의 동기적 표현 - Promise + Async/await
// Promise, async, await 활용
```

---
### 정리
  - 콜백 함수는 다른 코드에 인자로 넘겨줌으로써 그 제어권도 함께 위임한 함수

  - 제어권을 넘겨받은 코드는 다음과 같은 제어권을 가짐
    1. 콜백 함수를 호출하는 시점을 스스로 판단해서 실행
    2. 콜백 함수를 호출할 때 인자로 넘겨줄 값들 및 그 순서가 정해져 있음. 이 순서를 따르지 않고 코드를 작성하면 엉뚱한 결과
    3. 콜백 함수의 this가 무엇을 바라보도록 할지가 정해져 있는 경우도 있음. 정하지 않은 경우에는 전역객체를 바라봄. 사용자 임의로 this를 바꾸고 싶을 경우 bind 메서드를 활용

  - 어떤 함수에 인자로 메서드를 전달하더라도 이는 결국 함수로서 실행

  - 비동기 제어를 위해 콜백 함수를 사용하다 보면 콜백 지옥에 빠지기 쉬움. 최근 ECMAScript에는 Promise, Generator, async/await 등 콜백 지옥에서 벗어날 수 있는 좋은 방법들이 등장
