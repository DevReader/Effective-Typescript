## [ITEM 33] string 타입보다 더 구체적인 타입 사용하기

### string을 사용하기보다 문자열 리터럴 타입의 유니온 사용하기

- 장점1: 타입을 명시적으로 정의함으로써 다른 곳으로 값이 전달되어도 타입 정보가 유지됨
- 장점2: 타입을 명시적으로 정의하고 해당 타입의 의미를 설명하는 주석을 붙여 넣을 수 있음
- 장점3: keyof 연산자로 더욱 세밀하게 객체의 속성 체크가 가능해짐

### 객체의 속성 이름을 함수 매개변수로 받을 때는 string보다 keyof T 사용하기

```tsx
function pluch<T, K extends keyof T>(records: T[], key: K): T[K][] {
  return records.map((r) => r[key]);
}
```
