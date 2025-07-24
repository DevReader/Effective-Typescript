## [ITEM 20] 다른 타입에는 다른 변수 사용하기

### 변수의 값은 바뀔 수 있지만 타입은 보통 바뀌지 않는다

```tsx
const id = "12-34-56";
fetchProduct(id);

const serial = 123456; // 정상
fetchProductBySerialNumber(serial); // 정상
```

### 다른 타입에는 별도의 변수를 사용한다

- 서로 관련없는 두 개의 값을 분리
- 변수명 구체적으로 적을 수 있음
- 타입 추론을 향상시키며, 타입 구문이 불필요해짐
- 타입이 더 간결해짐
- let 대신 const로 변수를 선언하게 됨

```tsx
"no-shadow": "error" // eslint
```
