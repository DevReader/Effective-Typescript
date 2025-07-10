## [ITEM 8] 타입 공간과 값 공간의 심벌 구분하기

### 타입

- type이나 interface 다음에 나오는 심벌
- 타입 선언 또는 단언문 다음에 나오는 심벌
- type T1 = typeof p; → 타입스크립트 타입을 반환

### 값

- const나 let 선언에 쓰이는 것
- = 다음에 나오는 모든 것
- const v1 = typeof p; → object (런타임 타입을 가리키는 문자열을 반환)
