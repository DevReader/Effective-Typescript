## any 타입은 가능한 한 좁은 범위에서만 사용하기

- any 를 반환하면 any 타입의 영향력이 프로젝트 전반에 미칠 수 있음

  ```typescript
  // any 반환
  function f1() {
    const x: any = expressionReturningFoo();

    processBar(x);
    return x;
  }

  // 더 나은 방법
  function f2() {
    const x = expressionReturningFoo();

    processBar(x as any);
    return x; // Foo 타입으로 반환
  }
  ```

- `@ts-ignore` 를 통해 타입 오류를 강제로 제거할 수 있지만 근본적인 문제를 해결하는 것이 바람직함
- 큰 객체 안의 한 개 속성이 타입 오류를 가지는 상황

  ```typescript
  const config: Config {
    a: 1,
    b: 2,
    c: {
      key: value // 에러, Foo 타입에 foo 프로퍼티가 필요하지만 Bar 타입에는 없습니다.
    }
  }

  // 지양
  const config: Config {
    a: 1,
    b: 2,
    c: {
      key: value
    }
  } as any

  // 지향
  const config3: Config {
    a: 1,
    b: 2,
    c: {
      key: value as any
    }
  }
  ```
