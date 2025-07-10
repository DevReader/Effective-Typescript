## 동적 데이터에 인덱스 시그니처 사용하기

- 타입스크립트에서는 타입에 인덱스 시그니처를 명시하여 유연하게 매핑을 표현할 수 있음
- 인덱스 시그니처란?
  ```typescript
  const Rocket = {[property: string]: string};
  const rocket = {
    name: 'Falcon 9',
    variant: 'Block 5',
    thrust: '7,607 kN'
  }
  ```
  `[property: string]: string` 를 인덱스 시그니처라고 하고 세 가지 의미를 담고 있음
  - 키의 이름: 키의 위치만 표현. 타입 체커에서는 사용하지 않아 무시할 수 있는 참고 정보
  - 키의 타입: string / number / symbol의 조합이어야 함. 주로 string 을 사용
  - 값의 타입: 어떤 것이든 될 수 있음
- 인덱스 시그니처를 사용해서 타입 체크를 수행했을 때 단점
  - 잘못된 키를 포함해 모든 키 허용. name 대신 Name 을 작성해도 유효한 Rocket 타입이 됨
  - 특정 키가 필요하지 않음. `{}` 도 유효한 Rocket 타입
  - 키마다 다른 타입을 가질 수 없음(name, variant, thrust 모두 string 타입)
  - 타입스크립트 언어 서비스가 도움이 되지 못할 수 있음(키에 뭐든 가능하므로 자동 완성 기능 동작x)
- 인덱스 시그니처는 부정확하므로 원하는 필드가 명확하다면 interface 를 사용하는 것이 나음
- 인덱스 시그니처는 동적 데이터를 표현할 때 사용
- 안전한 접근을 위해 인덱스 시그니처의 값 타입에 undefined 를 추가하는 것을 고려해야 함
- string 타입이 너무 광범위해서 인덱스 시그니처를 사용하는 데 문제가 있을 때
  - 키 타입에 유연성을 제공하는 Record 제너릭 타입 사용
    ```typescript
    type Vec3D = Record<"x" | "y" | "z", number>;
    ```
  - 매핑된 타입 사용
  ```typescript
  type ABC = { [k in "a" | "b" | "c"]: k extends "b" ? string : number };
  ```
