## [ITEM 9] 타입 단언보다는 타입 선언을 사용하기

### 타입 단언

```tsx
const bob = { name: "Bob" } as Person; // !도 포함
```

- 타입스크립트보다 타입 정보를 잘 알고 있는 상황에서는 타입 단언문과 null이 아님 단언문을 사용하면 된다.

### 타입 선언

```tsx
const alice: Person = { name: "Alice" };
```

- 할당되는 값이 해당 인터페이스를 만족하는지 검사함. (단언과 다름)
