## [ITEM 15] 동적 데이터에 인덱스 시그니처 사용하기

- 인덱스 시그니처
  - 단점: 모든 키 허용, {}도 가능, 키마다 다른 타입 가질 수 없음, 자동 완성 기능 동작하지 않음

```tsx
type Rocket = {[property: string]: string};

- 키의 이름: 키위 위치 표시
- 키의 타입: 보통은 string
- 값의 타입: 어떤 것이든 될 수 있음
```

- 동적 데이터 표현
  - 미리 알 수 없는 데이터
  - 열을 미리 알 수 있다면 미리 선언해둔 타입으로 단언문 사용
    ```tsx
    const products = parseCSV(csvData) as unknown as ProductRow[];
    ```
- 연관 배열의 경우, 인덱스 시그니처 대신 Map 타입을 사용하는 것을 고려할 수 있음.
- 어떤 타입에 가능한 필드가 제한되어 있는 경우 → 인덱스 시그니처로 모델링 X
  - 선택적 필드 또는 유니온 타입으로 모델링.
  - Record 사용 (부분 집합 사용 가능)
    ```tsx
    type Vec3D = Record<"x" | "y" | "z", number>;
    ```
  - 매핑된 타입 사용: 키마다 별도의 타입을 사용할 수 있음
    ```tsx
    type ABC = { [k in "a" | "b" | "c"]: k extends "b" ? string : number };
    ```
