## [ITEM 22] 타입 좁히기

타입스크립트가 넓은 타입으로부터 좁은 타입으로 진행하는 과정

- null 체크, 분기문에서 예외를 던지거나 함수 반환, 속성 체크, instanceof, 일부 내장함수 (Array.isArray)
- 명시적 태그 붙이기 (태그된 유니온)
- 사용자 정의 타입 가드
  - el is HTMLInputElement
  ```tsx
  function isInputElement(el: HTMLElement): el is HTMLInputElement {
    return "value" in el;
  }
  ```
