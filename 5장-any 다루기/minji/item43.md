## 몽키패치보다는 안전한 타입 쓰기

- 몽키패치: 런타임 동안에 동적으로 객체에 메서드나 프로퍼티를 추가하는 패턴
- 몽키패치는 지양해야하지만 어쩔 수 없이 사용해야 하는 경우가 있고, 타입스크립트에서는 타입체커가 임의로 추가한 속성에 대한 정보를 갖고 있지 않아 문제가 발생함
- 해결법

  - interface의 보강 기능 사용 -> 단, 전역적으로 적용되기 때문에 코드의 다른 부분이나 라이브러리로부터 분리할 수 없음
    ```typescript
    interface Document {
      monkey: string; // 기존의 Document 인터페이스에 monkey 속성이 추가됨.
    }
    ```
  - 더 구체적인 타입 단언문 사용

  ```typescript
  interface MonkeyDocument extends Document {
    monkey: string;
  }

  (document as MonkeyDocument).monkey = "hello";
  ```
