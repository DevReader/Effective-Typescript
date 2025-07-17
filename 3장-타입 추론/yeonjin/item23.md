## [ITEM 23] 한꺼번에 객체 생성하기

객체를 생성할 때 속성을 하나씩 추가하기보다는 여러 속성을 포함해서 한번에 생성해야 타입 추론에 유리하다

```tsx
const pt: Point {
    x: 3,
    y: 4,
}
```

- 객체 전개 연산자 … 사용하기
  - 업데이트마다 새 변수를 사용해서 새로운 타입을 얻어야함
  ```tsx
  const namePoint = { ...pt, ...id };
  ```
- 조건부 속성 추가하기
  - {}
  ```tsx
  const president = { ...firstLast, ...(hasMiddel ? { middle: "S" } : {}) };
  ```
  - 헬퍼 함수 사용하기
  ```tsx
  function addOptional<T extends object, U extends object>(
    a: T,
    b: U | null
  ): T & Partial<U> {
    return { ...a, ...b };
  }
  ```
  - 내장된 함수형 기법 또는 로대시 유틸 라이브러리 사용하기
