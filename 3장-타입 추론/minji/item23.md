## 한꺼번에 객체 생성하기

- 객체를 생성할 때는 속성을 하나씩 추가하기보다는 여러 속성을 포함해서 한꺼번에 생성해야 타입 추론에 유리함

  ```typescript
  // 속성 하나씩 추가 -> 타입스크립트에서 오류
  const pt = {};
  pt.x = 3; // ~ '{}' 형식에 'x' 속성이 없습니다.
  pt.y = 4; // ~ '{}' 형식에 'y' 속성이 없습니다.

  // 해결1: Point 인터페이스 정의
  interface Point { x: number; y: number; }
  const pt = {
    x: 3;
    y: 4;
  }

  // 해결2: 타입 단언문 사용 -> 한 번에 생성하는 게 더 나음(단언보다 선언이 낫기 때문)
  const pt = {} as Point;
  pt.x = 3;
  pt.y = 4;
  ```

- 작은 객체들을 조합해서 큰 객체를 만들어야 하는 경우에도 여러 단계를 거치는 것은 좋지 않음 -> 객체 전개 연산자 `...` 을 사용

  ```typescript
  // 큰 객체 한 번에 만들기
  const pt = { x: 3, y: 4 };
  const id = { name: "Pythagoras" };
  const namePoint = { ...pt, ...id };

  // 타입 걱정 없이 필드 단위로 객체 생성하기
  const p0 = {};
  const p1 = { ...p0, x: 3 };
  const p2 = { ...p1, y: 4 };
  ```

- 타입에 안전한 방식으로 조건부 속성을 추가하려면, 속성을 추가하지 않는 `null` 또는 `{}`으로 객체 전개 사용
  ```typescript
  declare let hasMiddle: boolean;
  const firstLast = { first: 'Harry', last: 'Truman' };
  const president = { ...firstLast, ...{hasMiddle ? { middle: 'S' } : {}}};
  ```
- 전개 연산자로 한꺼번에 여러 속성 추가 가능
- 선택적 필드 방식으로 표현하려면 헬퍼 함수를 사용
  ```typescript
  function addOptional<T extends obj, U extends obj>(
    a: T,
    b: U | null
  ): T & Partial<U> {
    return { ...a, ...b };
  }
  ```
- 객체나 배열을 변환해서 새로운 객체나 배열을 생성하고 싶을 땐 루프 대신 내장된 함수형 기법 또는 로대시(Lodash)같은 유틸리티 라이브러리를 사용하는 것이 옳음
