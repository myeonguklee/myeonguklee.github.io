---
layout: single
title: "코어 자바 스크립트 - 05.Closure"
typora-root-url: ../
---

## Clusure(클로저)

실행컨텍스트 A 내부에서 함수 B를 선언 -> A의 lexical environment와 내부함수 B의 조합 => A의 L.E와 함수 B의 조합에서 나타나는 특별한 현상

즉, **컨텍스트 A에서 선언한 변수 a를 참조하는 내부함수 B를 A의 외부로 전달할 경우, A가 종료된 이후에도 a가 사라지지 않는 현상** (지역 변수가 함수 종료 후에도 사라지지 않게 할 수 있다)


```javascript
function  user (_name) {
  var _logged = true;
  return {
    get name() { return _name; },
    set name(v) { _name = v },
    login() { _logged = true; },
    logout() { _logged = false; },
    get status() { return _logged ? 'login':'logout'; },
  }
}

var roy = user('재남');

console.log(roy.name);  // '재남'

roy.name = '제이';
console.log(roy.name);  // '제이'

roy._name = '로이';
console.log(roy.name);  // '제이'

console.log(roy.status);  // 'login'

roy.logout();
console.log(roy.status);  // 'logout'

// => _name, _logged 두 변수 함수 종료 후에도 사라지지 않음!
// status 프로퍼티는 getter로서만 역할, 실제 _logged의 값과는 별개의 문자열 만 반환,
// 직접적으로 _logged의 변화에 영향을 주는건 login/logout 메서드에 의해서만 가능 <= 캡술화
```

1. 함수 종료 후에도 사라지지 않고 값을 유지하는 변수
2. 외부로부터 내부 변수 보호(캡슐화)

=> **함수 종료 후에도 사라지지 않는 지역변수를 만들 수 있다!**


## 클로저의 메모리 관리
클로저 사용에 있어 개발자가 의도적으로 참조 카운트를 0이 되지 않게 설계한 경우이기 때문에 '메모리 누수'라고 보기 어렵다.

  - 참조 카운트를 0으로 만드는 방법? : 식별자에 참조형이 아닌 기본형 데이터(보통 null, undefined)를 할당

*메모리 누수 : 개발자의 의도와 달리 어떤 값의 참조 카운트가 0이 되지 않안 GC(Garbage Collector)의 수거 대상이 되지 않는 경우

## 클로저 활용 사례
- 콜백 함수 내부에서 외부 데이터를 사용하고자 할 때
- 접근 권한 제어(정보 은닉)
- 부분 적용 함수(디바운스(debounce) 등)
- 커링 함수(여러 개의 인자를 받는 함수를 하나의 인자만 받는 함수로 나눠서 순차적으로 호출 될 수 있게 체인 형태로 구성, 지연 실행(lazy execution))


### 정리
- 클로저 : 어떤 함수에서 선언한 변수를 참조하는 내부함수를 외부로 전달할 경우, 함수의 실행 컨텍스트가 종료된 후에도 해당 변수가 사라지지 않는 현상

- 내부함수를 외부로 전달하는 방법에는 함수를 return 하는 경우, 콜백으로 전달하는 경우도 포함

- 클로저는 그 본질이 메모리를 계속 차지하는 개념이므로 더는 사용하지 않게 된 클로저에 대해서 메모리를 차지하지 않도록 관리해줄 필요가 없음

- 클로저는 다양한 곳에서 활용할 수 있는 중요한 개념.