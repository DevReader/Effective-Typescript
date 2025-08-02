## 아이템 7

### 공집합을 의미하는 타입인 `never`가 사용되는 순간은 언제?

## 아이템 12

### 자바스크립트에서는 함수 표현식 vs 함수 문장 중 어떤 것이 유리할까?

## 아이템 14

### 함수 시그니처란?

함수와 메서드의 입출력을 정의하는 방식으로 다음을 포함함

- 매개변수와 그들의 타입
- 반환값과 타입
- 던져지거나 콜백으로 반환되는 예외
- 객체 지향 프로그램에서 메서드의 접근 권한에 대한 정보(public, static 혹은 prototype 과 같은 키워드)

```typescript
type Operation = (a: number, b: number) => number;
```

함수 시그니처를 타입으로 정의하고 사용하면 매번 타입을 적지 않아도 되어서 간결해짐

```typescript
// before
const add = (a: number, b: number) => a + b;
const sub = (a: number, b: number) => a - b;

// after
const add: Operation = (a, b) => a + b;
const sub: Operation = (a, b) => a - b;
```

### ReturnType 제너릭 타입이란?

주어진 제너릭 타입 T의 return type 을 할당함

```typescript
type T0 = ReturnType<() => string>; // type T0 은 string
type T1 = ReturnType<(s: string) => number>; // type T1 은 number
```

## 아이템 15

### 자바스크립트 객체에서 키는 항상 문자열로 변환됨

```
const obj = {
  123: "숫자 키",
  true: "불리언 키",
  [Symbol()]: "심볼 키"
};

console.log(obj["123"]); // "숫자 키"
console.log(obj[123]); // "숫자 키"
```
