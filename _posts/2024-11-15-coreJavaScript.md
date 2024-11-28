---
layout: single
title:  "코어 자바 스크립트 - 01.데이터 타입"
typora-root-url: ../


---

## DATA TYPE

### Primitive Type(기본형)  
    * Number
    * String
    * Boolean
    * nulll
    * undefined
    * Symbol (ES6)

### Reference Type(참조형)
    * Array
    * Function
    * RegExp
    * Set/WeakSet (ES6)
    * Map/ WeakMap (ES6)


\* 변수(variable): 변할 수 있는 수<br>
\* 식별자(identifier): 어떤 데이터를 식별하는 데 사용하는 이름, 즉 변수명

\* stack memory -> 변수, 기본형 데이터, 정적 할당<br>
\* heap memory -> 참조형 데이터, 동적할당

\* 값을 직접 저장: 데이터 할당시에는 빠름, 비교에 비용이 많이 듦, 메모리 낭비가 심함<br>
\* 값의 주소를 저장: 데이터 할당시에는 느림, 비교에 비용이 들지 않음, 메모리 낭비 최소화

### 불변 객체

불변값 : 같은 값이 오직 하나만 존재(값의 주소를 저장, 비교에 비용이 들지 않음) => **기본형 데이터가 불변값**

기본형의 경우에는 값을 바꿨을 때 바로 바뀌는 반면에 **객체를 복사하고 복사한 객체의 값을 바꿨는데 원본 객체의 값이 같이 바뀌는 경우** -> 참조하고 있는 객체(값)이 바뀌지 않기 때문, 뎁스가 하나 더 들어가기 때문에

\* 불변 객체가 필요한 경우 : 값으로 전달받은 객체에 변경을 가하더라도 원복 객체가 변하지 않아야 하는 경우 (참조형 데이터의 '가변'은 데이터 자체가 아닌 내부 프로퍼티를 변경할 때만 성립)

**불변 객체를 만들기 위해**

1) 라이브러리 활용(immutable,js, baoba.js)
2) 깊은 복사 활용
    - 참조형 데이터의 경우 그 내부의 프로퍼티들을 복사(재귀적으로 복사를 해야 함)

    ~~~
    function copyObjectDeep (target) {
        let result = {};
        if (typeof target === 'object' && target !== null) {
            for (let prop in target) {
                result[prop] = copyObjectDeep(target[prop]);
            }
        } else {
            result = target;
        }
        return result;
    }
    ~~~
    - 객체를 JSON 문법으로 표현된 문자열로 전환했다가 다시 JSON 객체로 바꾸기

    ~~~
    function copyObjectViaJSON (target) {
        return JSON.parse(JSON.stringify(target))
    }
    ~~~

### undefined 와 null

* undefined
    * 값을 대입하지 않은 변수, 즉 데이터 영역의 메모리 주소를 지정하지 않은 식별자에 접근할 때
    * 객체 내부의 존재하지 않는 프로퍼티에 접근하려고 할 때
    * return 문이 없거나 호출되지 않는 함수의 실행 결과

=> undefined를 할당하지 말자!!, '비어있음'을 명시적으로 나타내고 싶을 때는 undefined가 아닌 null을 사용하기!!
\* 주의) typeof null은 object 임으로 일치 연산자(===)를 써서 판별 해야함!