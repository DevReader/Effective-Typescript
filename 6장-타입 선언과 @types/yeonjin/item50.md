## [ITEM 50] 오버로딩 타입보다는 조건부 타입을 사용하기

- double 함수 예제
  - double 함수에는 string or number type의 매개변수 가능

```tsx
function double(x) {
  return x + x;
}
```

- 잘못된 매칭의 가능성
  - 문제: number 타입의 매개변수 & string 타입 반환

```tsx
function double(x: number | string): number | string;
function double(x: any) {
  return x + x;
}

const num = double(12); // string | number
const str = double("x"); // string | number
```

- 제너릭 사용해서 개선
  - 문제: 타입이 너무 구체적

```tsx
function double<T extends number | string>(x: T): T;
function double(x: any) {
  return x + x;
}

const num = double(12); // 타입이 12
const str = double("x"); // 타입이 "x"
```

- 여러 타입으로 분리해보기
  - 문제: 유니온 타입 관련해서 문제가 발생

```tsx
function double(x: number): number;
function double(x: string): string;
function double(x: any) {
  return x + x;
}

const num = doluble(12); //타입이 number
const str = double("x"); // 타입이 string

function f(x: number | string) {
  return double(x); // 'string | number' 형식은 'string'형식에 할당 x
}
```

- **조건부 타입 사용하기** (=타입 공간의 if 구문)
  - 추천: 개별 타입의 유니온으로 일반화되기 때문에 타입이 더 정확해짐

```tsx
function double<T extends number | string>(
  x: T
): T extends string ? string : number;
function double(x: any) {
  return x + x;
}
```
