## [ITEM 16] number 인덱스 시그니처보다는 Array, 튜플, ArrayLike 사용하기

- 자바스크립트에서 객체란 키/값 쌍의 모음이다. 키는 보통 문자열이고 값은 어떤 것이든 될 수 있다.
- 타입스크립트에서는 `number` 인덱스 시그니처를 별도로 지원해서, "배열처럼" 사용하라는 의미로 쓰는 것일 뿐 실제 내부적으로는 string key로 저장된다.
- 인덱스 시그니처에 number를 사용하기보다 Array나 튜플, ArrayLike 타입을 사용하는 것이 좋다.

```tsx
interface NumberMap {
  [index: number]: string;
}

interface Array<T> {
  length: number;
  [index: number]: T;
  // plus: map, filter, push, pop...
}
```
