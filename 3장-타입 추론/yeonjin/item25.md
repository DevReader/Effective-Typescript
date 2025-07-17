## [ITEM 25] 비동기 코드에는 콜백 대신 async 함수 사용하기

### 콜백

```tsx
fetchURL(url1, function (response1) {
  fetchURL(url2, function (response2) {
    fetchURL(url3, function (response3) {
      //...
      console.log(1);
    });
    console.log(2);
  });
  console.log(3);
});
console.log(4);
```

### ES2015 (프로미스)

- 미래에 가능해질 어떤 것
- 중첩 적어짐. 실행 순서 == 코드 순서
- 오류 처리하기 쉬워짐

```tsx
const page1Promise = fetch(url1);
page1Promise
  .then((response1) => {
    return fetch(url2);
  })
  .then((response2) => {
    return fetch(url3);
  })
  .then((response3) => {
    // ...
  })
  .catch((error) => {
    // ...
  });
```

### ES2017 (async, await)

```tsx
async function fetchPages() {
  const response1 = await fetch(url1);
  const response2 = await fetch(url2);
  const response3 = await fetch(url3);
}
```

- 콜백보다 async/await을 사용해야함
  - 콜백보다 프로미스가 코드를 작성하기 쉬움
  - 콜백보다 프로미스가 타입을 추론하기 쉬움
- 프로미스를 직접 사용하기보다는 `async` / `await` 를 활용해야함
  - 일반적으로 더 간결하고 직관적인 코드를 작성할 수 있다.
  - `async` 함수는 항상 프로미스를 반환하도록 강제된다.
