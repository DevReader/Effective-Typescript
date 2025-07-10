## [ITEM 17] 변경 관련된 오류 방지를 위해 readonly 사용하기

- readonly: 변경되지 않음을 선언.

### readonly number[] 와 number[]의 구분

- 배열의 요소를 읽을 수 있지만, 쓸 수 없음
- length를 읽을 수 있지만 바꿀 수 없음
- 배열을 변경하는 pop을 비롯한 다른 메서드를 호출할 수 없음.
- number[]는 readonly number[]의 서브타입
- readonly number[]에 number[] 할당 가능
- 다른 라이브러리에 함수를 호출하는 경우라면 타입 단언문(as number[]) 사용

### **readonly로 선언시 발생하는 문제 해결 예시**

```tsx
const currPara: readonly string[] = [];
const paragraphs: string[][] = [];

paragraphs.push(currPara); // 오류: readonly string[]을 string[] 형식의 매개변수에 할당 X
```

- 해결 1: currPara의 복사본 만들기
  ```tsx
  paragraphs.push([...currPara]);
  ```
- 해결 2: paragraphs를 readonly string[]의 배열로 변경하기
  ```tsx
  const paragraphs: (readonly string[])[] = [];
  ```
- 해결 3: readonly 속성을 제거하기 위해 단언문 쓰기
  ```tsx
  paragraphs.push(currPara as string[]);
  ```

### readonly는 얕게 동작한다

```tsx
interface Outer {
  inner: {
    x: number;
  };
}
const o: Readonly<Outer> = { inner: { x: 0 } };
o.inner = { x: 1 }; // ~~ 읽기 전용 속성이기 때문에 'inner'에 할당할 수 없습니다.
o.inner.x = 1; // 정상
```

- DeepReadonly 제너릭: 깊은 readonly 타입 사용하기

### const와 readonly의 차이

> const는 변수를 보호하고, readonly는 객체 속성을 보호한다.

- const
  ```tsx
  const obj = { name: "Alice" };
  obj = { name: "Bob" }; // error
  obj.name = "Bob"; // ok
  ```
- readonly
  ```tsx
  type User = {
    readonly name: string;
  };

  const x: User = { name: "Bob" };
  x.name = "Alice"; // X
  ```
- 둘다 사용할 때
  ```tsx
  const user: { readonly name: string } = { name: "Alice" };
  user.name = "Bob"; // ❌ readonly 때문에 불가
  user = { name: "Bob" }; // ❌ const 때문에 불가
  ```
