## 추론 가능한 타입을 사용해 장황한 코드 방지하기

- 모든 변수에 타입을 선언하는 것은 불필요함. 타입 추론이 된다면 명시적 타입 구문은 필요하지 않음
- 비구조화 할당문을 사용해 구현하는 것이 더 좋음

  ```typescript
  // before
  interface Product {
    id: string;
    name: string;
    price: number;
  }

  function logProduct(product: Product) {
    const id: number = product.id; // 오류 발생
    ...
  }

  // after
  function logProduct(product : Product) {
    const { id, name, price } = product;

  }
  ```

  비구조화 할당문은 모든 지역변수의 타입이 추론되도록 함

- 기본 값이 존재하는 함수의 매개변수에 대해서는 타입 구문 생략 가능
- 타입이 추론될 때도 매개변수를 명시하고 싶은 상황
  - 객체 리터럴 정의(변수를 사용하는 시점이 아닌 할당하는 시점에 오류를 발생시킴)
  - 함수의 반환(분기에 따라 반환값이 바뀔 수 있음을 선언 시점에 확인 가능, 함수를 조금 더 직관적으로 이해할 수 있음)
