## [ITEM 54] 객체를 순회하는 노하우

### 타입 한정

- 키가 어떤 타입인지 정확히 파악하고 있다면 let k: keyof T와 for-in 루프를 사용한다.
- 함수의 매개변수로 쓰이는 객체에는 추가적인 키가 존재할 수 있다.

```tsx
function foo(abc, ABC) {
  let k: keyof ABC; // 이 코드를 추가하지 않으면 다르게 추론될 수 있음
  for (k in abc) {
    // let k : "a" | "b" | "c"
    const v = abc[k]; // string | number 타입
  }
}
```

→ 타입이 너무 좁아 문제가 발생할 수 있음

### Object.entries 사용하기 (일반적인 방법)

```tsx
function foo(abc: ABC) {
  for (const [k, v] of Object.entries(abc)) {
    k; // string 타입
    v; // any 타입
  }
}
```
