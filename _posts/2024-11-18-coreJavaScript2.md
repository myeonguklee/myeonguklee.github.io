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
  => SCOPE CHAIN