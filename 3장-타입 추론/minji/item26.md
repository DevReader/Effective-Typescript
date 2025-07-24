## 타입 추론에 문맥이 어떻게 사용되는지 이해하기

- 타입스크립트는 타입 추론을 위해 문맥 또한 고려함
- 인라인 형태로 작성하는 것과 변수를 통해 참조 형태로 작성하는 것에는 문맥 소실로 인해 타입 추론이 다르게 적용됨

  ```typescript
  type Language = 'JS' | 'TS' | 'Python';
  function setLanguage(language: Language) { ... }

   // 인라인 형태
  setLanguage('JS'); // 정상

  // 참조 형태
  let lang = 'JS';
  setLanguage(lang); // 'string' 으로 추론해서 Language 에 할당할 수 없다는 오류 발생
  ```

  오류 해결 방법

  ```typescript
  // 타입 선언
  let lang: Language = "JS";

  // 상수로 선언
  const lang: "JS";
  ```

- 튜플 사용 시 주의점

  ```typescript
  function panTo(where: [number, number]) { ... }

  // 인라인 형태
  panTo([10, 20]); // 정상

  // 상수 변수
  const loc = [10, 20];
  panTo(loc); // loc을 number[] 형식으로 추론해서 [number, number] 에 할당할 수 없다는 오류 발생

  // 해결 - 상수 변수
  const loc: [number, number] = [10, 20];

  // 싱수 문맥 제공
  const loc = [10, 20] as const;
  panTo(loc); // loc을 readonly [10, 20] 으로 추론해서 [number, number] 에 할당할 수 없다는 오류 발생

  // 해결 - 상수 문맥 제공
  function panTo(where: readonly [number, number]) { ... }
  ```

  - const는 값이 가리키는 참조가 변하지 않는다는 얕은 상수라면, as const 는 내부까지(deeply) 상수라는 의미
  - as const 는 문맥 손실 문제를 해결할 수 있지만 타입 정의에서 실수가 있다면 오류가 타입을 정의한 곳이 아닌 호출되는 곳에서 발생함

- 객체나 콜백 사용 시에도 튜플과 같은 문제가 발생할 수 있으므로 타입 선언을 추가하거나 상수 단언을 사용해 해결
