## 타입이 값들의 집합이라고 생각하기

- 런타임에 모든 변수는 자바스크립트 세상의 값으로부터 정해지는 각자의 고유한 값을 가짐
- 코드가 실행되기 전, 타입스크립트가 오류를 체크하는 순간에는 할당 가능한 값들의 집합인 타입을 가지고 있음
- 가장 작은 집합은 아무 값도 포함하지 않는 공집합으로 타입스크립트에서는 `never`
- 리터럴 타입(유닛 타입): 한 가지 값만 포함하는 타입(ex.type A = 'A';)
- 유니온 타입: 두 개 혹은 세 개로 리터럴을 묶음(ex.type AB = 'A' | 'B';)
- 타입스크립트의 주요 역할은 하나의 집합이 다른 집합의 부분 집합인지 검사하는 것
- 타입 연산자는 인터페이스의 속성이 아닌 값의 집합에 적용됨

  ```TypeScript
  interface Person {
    name: string;
  }
  interface Lifespan {
    birthdate: Date;
    death?: Date;
  }
  type PersonSpan1 = Person & Lifespan;
  // Person 타입이 가진 속성과 Lifespan 이 가진 속성의 교집합이 아닌, Person 타입과 Lifespan의 속성을 모두 가진(두 타입의 속성+a 도 포함) 타입을 의미함
  ```

  ```
  keyof (A&B) = (keyof A) | (keyof B)
  keyof (A|B) = (keyof A) & (keyof B)
  ```

- extends 키워드를 사용하는 것은 ~의 부분 집합이라는 의미로 받아들일 수 있음
- `A는 B를 상속`, `A는 B에 할당 가능`, `A는 B의 서브타입`은 `A는 B의 부분 집합과 같은말`
