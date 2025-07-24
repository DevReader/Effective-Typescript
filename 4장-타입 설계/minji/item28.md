## 유효한 상태만 표현하는 타입을 지향하기

- 타입은 유효한 상태를 표현하는 값만 허용해야 함
- 유효한 상태와 무효한 상태 예시

  ```typescript
  // 무효한 상태
  interface State { // isLoading 이 true 고 error 텍스트가 있으면 로딩중이면서 에러가 발생했다는 무효한 상태가 될 수 있음
    pageText: string;
    isLoading: boolean;
    error? : string;
  }

  function renderPage(state: State) { ... }

  // 유효한 상태
  interface RequestPending {
    state: 'pending';
  }

  interface RequestSuccess {
    state: 'ok';
    pageText: string;
  }

  interface RequestError {
    state: 'error';
    error: string;
  }

  type RequestState = RequestPending | RequestSuccess | RequestError;

  interface State {
    currentPage: string;
    requests: {[page:string]: RequestState}
  }
  ```
