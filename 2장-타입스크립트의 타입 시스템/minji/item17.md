## 변경 관련된 오류 방지를 위해 readonly 사용하기

- 자바스크립트는 배열의 내용을 변경할 수 있음
  ```typescript
  function arraySum(arr: number[]) {
    let sum = 0,
      num;
    while ((num = arr.pop()) !== undefined) {
      sum += num;
    }
    return sum;
  }
  ```
  위 함수의 실행이 끝나면 원래 배열이 전부 비게 됨
- readonly 접근 제어자를 이용하면 배열을 변경하지 않는다는 선언을 할 수 있음
  ```typescript
  function arraySum(arr: readonly number[]) {
    let sum = 0,
      num;
    while ((num = arr.pop()) !== undefined) {
      // 오류 발생.
      sum += num;
    }
    return sum;
  }
  ```
- readonly number[] vs number[]
  - 배열의 요소는 읽을 수 있지만 쓸 수 없음
  - length를 읽을 순 있지만 바꿀 수 없음
  - 배열을 변경하는 pop을 비롯한 다른 메서드를 호출할 수 없음
- 매개변수를 readonly 로 선언하면 생기는 일
  - 타입스크립트는 매개변수가 함수 내에서 변경이 일어나는지 체크
  - 호출하는 쪽에서는 함수가 매개변수를 변경하지 않는다는 보장을 받게됨
  - 호출하는 쪽에서 함수에 readonly 배열을 매개변수로 넣을 수 있음
- 자바스크립트와 타입스크립트는 명시적으로 언급하지 않는 이상 함수가 매개변수를 변경하지 않는다고 가정함
- readonly 로 선언함으로써 더 넓은 타입으로 호출할 수 있고 의도치 않은 변경이 방지됨
- readonly 로 선언했을 때의 단점
  - readonly 로 선언되지 않는 함수를 호출해야 할 수 있음
  - 어떤 함수를 readonly로 만들면 그 함수를 호출하는 다른 함수들도 모두 readonly로 만들어야 함(다른 라이브러리에 있는 함수를 호출하는 경우 타입 선언을 바꿀 수 없기에 타입 단언문을 이용해야 함)
- readonly는 얕게 동작함 -> DeepReadonly 제너릭을 사용하여 깊은 readonly 타입을 이용할 수 있음

  ```typescript
  interface Outer {
    inner: {
      x: number;
    };
  }

  const o: Readonly<Outer> = { inner: { x: 0 } };
  ```

  `o.inner = { x:1 }` 는 오류지만 `o.inner.x = 1` 은 정상임(얕게 동작하기 때문)
