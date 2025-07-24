## 타입 넓히기

- 타입스크립트가 작성된 코드를 체크하는 정적 분석 시점엔 변수는 가능한 값들의 집합인 타입을 가짐
- 타입을 명시하지 않으면 타입 체커는 단일 값으로 할당 가능한 값들의 집합을 유추함(타입 넓히기: 단일 값 -> 할당 가능한 값들의 집합)
  ```typescript
  let x = "x"; // 단일 값 'x'가 아닌 집합인 string 으로 추론
  ```
- `const` 사용해서 타입 넓히기의 과정을 제어하는 법

  ```typescript
  const x = "x"; // "x" 타입
  ```

  단 객체와 배열의 경우 const 로는 여전히 문제가 될 수 있음 (ex. `const mixed = ['x', 1];`)

  객체의 경우 타입스크립트 넓히기 알고리즘 상에서는 각 요소를 let으로 할당된 것처럼 다룸

  ```typesscript
  const v = {
    x: 1,
  };
  ```

  위 경우에는 v의 타입은 `{x: number}` 가 되어, v.x 는 숫자로 재할당 가능하고 다른 속성 추가는 불가능

- 타입 추론 강도를 직접 제어하기 위해 타입스크립트 기본 동작 재정의하기
  - 명시적 타입 구문 제공하기
    ```typescript
    const v: { x: 1 | 2 | 3 } = {
      x: 1,
    }; // v의 타입: { x: 1|2|3 }
    ```
  - 타입 체커에 추가적인 문맥 제공하기
  - const 단언문 사용하기()

    ```typescript
    const v2 = {
      x: 1 as const,
      y: 1,
    }; // v2 타입: { x: 1; y: number }
    const v3 = {
      x: 1,
      y: 2,
    } as const; // v3 타입: { readonly x:1; readonly y:2; }

    const a1 = [1, 2, 3]; // a1 타입: number[]
    const a2 = [1, 2, 3] as const; // a2 타입: readonly [1,2,3]
    ```
