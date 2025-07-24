## [ITEM 26] 타입 추론에 문맥이 어떻게 사용되는지 이해하기

### **'문맥'으로부터 '값'을 분리하는 작업으로 인해 발생한 불일치를 해결하는 방법**

타입스크립트는 일반적으로 값이 처음 등장할 때 타입을 결정한다

- 타입 선언에서 가능한 값을 제한한다.
  ```tsx
  let language: Language = "JS";
  setLanguage(language);
  ```
- 상수로 만든다
  ```tsx
  const language = "JS";
  setLanguage(language);
  ```

### 튜플/객체에서 문맥과 값을 분리할 때 발생하는 문제 해결하기

- 타입 선언 제공하기
  ```tsx
  const loc: [number, number] = [10, 20];
  ```
- 상수 문맥 제공하기
  - 상수 단언을 사용하면 정의한 곳이 아닌, 사용한 곳에서 오류가 발생하므로 주의
  ```tsx
  const loc = [10, 20] as const;
  ```

### 콜백 사용 시 주의점

```tsx
function callWithRandomNumbers(fn: (n1: number, n2: number) => void) {
  fn(Math.random(), Math.random());
}

callWithRandomNumbers((a, b) => {
  console.log(a + b); // a, b type은 number로 추론된다.
});

const func = (a, b) => {
  console.log(a + b); // a, b type은 any로 추론된다.
};

callWithRandomNumbers(func);
```

- 타입 구문 추가하기
  ```tsx
  const func = (a: number, b: number) => {
    console.log(a + b);
  };

  type callback = (a: number, b: number) => void;
  const func2: callback = (a, b) => {
    console.log(a + b);
  };
  ```
