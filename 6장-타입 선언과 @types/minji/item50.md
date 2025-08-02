## 오버로딩 타입보다는 조건부 타입을 사용하기

- 오버로딩 타입보다 조건부 타입을 사용하는 것이 좋음

  ```ts
  // 오버로딩 타입 사용
  function double(x: number): number;
  function double(x: string): string;
  function double(x: any) {
    return x + x;
  }

  // 제네릭과 조건부 타입 사용 -
  function double<T extends number | string>(
    x: T
  ): T extends string ? string : number;
  function double(x: any) {
    return x + x;
  }

  // 기대하는대로 정상 동작
  const num = double(12); // 타입이 number
  const str = double("x"); // 타입이 string

  // 유니온 타입 관련해서는 문제 발생
  function f(x: number | string) {
    return double(x);
    // ~ 'string | number' 형식의 인수는 'string' 형식의 매개변수에 할당될 수 없습니다.
  }
  ```
