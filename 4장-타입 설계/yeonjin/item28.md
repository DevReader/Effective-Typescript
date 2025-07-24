## [ITEM 28] 유효한 상태만 표현하는 타입을 지향하기

> 유효한 상태와 무효한 상태를 둘 다 표현하는 타입은 혼란을 초래하기 쉽고 오류를 유발하게 됨

### 유효한 상태만 표현할 수 있는 타입 만들기

- before
  - `isLoading: true`인데도 `error`나 `pageText`가 함께 존재할 수 있는 모순된 상태가 표현 가능

```tsx
interface State {
  pageText: string;
  isLoading: boolean;
  error?: string;
}
```

- after
  - 무효한 상태를 허용하지 않도록 개선
  - 각 상태에 필요한 필드만 접근 가능 → **타입 추론이 정확**

```tsx
interface RequestPending {
  state: "pending";
}

interface RequestError {
  state: "error";
  error: string;
}

interface RequestSuccess {
  state: "ok";
  pageText: string;
}

type RequestState = RequestPending | RequestError | RequestSuccess;

interface State {
  currentPage: string;
  requests: { [page: string]: RequestState };
}
```
