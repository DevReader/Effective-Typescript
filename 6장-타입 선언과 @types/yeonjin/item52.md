## [ITEM 52] 테스팅 타입의 함정에 주의하기

### 타입 테스트하기

- dtslint 또는 타입 시스템 외부에서 타입을 검사하는 유사한 도구를 사용하는 것이 더 안전하고 간단함
- 함수 실행 테스트 & 함수 반환 타입 체크

```tsx
const lengths: number[] = map(["john", "paul"], (name) => name.length);
```

### 반환 타입 체크를 위해 할당을 사용하는 방법의 문제점

1. 불필요한 변수를 만들어야 함

   1. 해결책: 변수를 도입하는 대신 헬퍼 함수를 정의한다

   ```tsx
   function assertType<T>(x: T) {}
   assertType<number[]>(map(["john", "paul"], (name) => name.length));
   ```

2. 위 해결책은 두 타입이 동일한지 확인하지 않고, 할당 가능성을 체크하고 있다

   1. 선언된 것보다 적은 매개변수를 가진 함수를 할당하는 것도 가능하다

   ```tsx
   const double = (x: number) => 2 * x;
   assertType<(a: number, b: number) => number>(double); //정상
   ```

### 매개변수 타입과 반환 타입만 분리하여 테스트하기

```tsx
const double = (x: number) => 2 * x;
let p: Parameters<typeof double> = null;
assertType<[number, number]>(p); // [number] 형식은 [number, number] 형식의 매개변수에 할당될 수 없습니다
let r: ReturnType<typeof double> = null;
assertType<number>(r); // 정상
```

### this가 포함된 콜백 함수 테스트하기

```tsx
function assertType<T>(x: T): void {}

declare function map<U, V>(
  array: U[],
  fn: (this: U[], u: U, i: number, array: U[]) => V // this 추가
): V[];

const beatles = ["john", "paul", "george", "ringo"];

assertType<number[]>(
  map(beatles, function (name, i, array) {
    assertType<string>(name);
    assertType<number>(i);
    assertType<string[]>(array);

    // this도 명시적으로 타입 체크
    assertType<string[]>(this);

    return name.length;
  })
);

// 참고: 화살표 함수는 this를 상위 스코프에서 캡처하므로 this 단언 불가
map(beatles, (name, i, array) => {
  assertType<string>(name);
  assertType<number>(i);
  assertType<string[]>(array);

  // assertType<string[]>(this); 컴파일 에러 발생 (화살표 함수는 this 명시 불가)
  return name.length;
});
```

### 타입 체커와 독립적으로 동작하는 도구로 타입 선언 테스트

- bad

```tsx
declare module "overbar"; // 문제: 전체 모듈에 any 타입 할당
```

- dtslint 사용해서 타입 선언 테스트
  - 글자 자체를 비교하기 때문에 string|number와 number|string이 다르게 인식됨

```tsx
declare function map<U, V>(
  array: U[],
  fn: (this: U[], u: U, i: number, array: U[]) => V
): V[];

const beatles = ["john", "paul", "george", "ringo"];
map(
  beatles,
  function (
    name, // $ExpectType string
    i, // $ExpectType number
    array // $ExpectType string[]
  ) {
    this; // $ExpectType string[]
    return name.length;
  }
); // $ExpectType number[]
```
