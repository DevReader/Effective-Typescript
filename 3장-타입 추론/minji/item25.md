## 비동기 코드에는 콜백 대신 async 사용하기

- 콜백보다는 프로미스가 코드를 작성하기 쉽고, 타입을 추론하기 쉬우므로 콜백보다는 프로미스나 async/awiat을 사용해야 함
- Promise 예제

  ```typescript
  // Promise.all - 병렬적으로 프로미스 처리. 중간에 에러 발생 시, 즉시 rejec
  async function fetchPages() {
    const [res1, res2, res3] = await Promise.all([
      fetch(url1),
      fetch(url2),
      fetch(url3),
    ]);
  }

  // Promise.race - 프로미스들 중 첫 번째가 처리될 때 완료
  function timeout(millis: number): Promise<never> {
    return new Promise((resolve, reject) => {
      setTimeout(() => reject("timeout"), millis);
    });
  }

  async function fetchWithTimeout(url: string, ms: number) {
    return Promise.race([fetch(url), timeout(ms)]);
  }
  ```

- 프로미스를 직접 생성해야 할 때, 일반적으로는 프로미스를 생성하기 보다 async/await을 사용해야 함
  - 일반적으로 더 간결하고 직관적
  - async 함수는 항상 프로미스를 반환하도록 강제
- 함수는 항상 비동기 또는 항상 동기로 실행되어야 하고 절대 혼용해선 안됨
