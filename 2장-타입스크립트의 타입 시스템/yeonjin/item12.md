## [ITEM 12] 함수 표현식에 타입 적용하기

> 타입 스크립트에서는 함수 표현식을 사용하는 것이 좋다. 함수 문장과 함수 표현식을 다르게 인식한다.

### 함수 표현식 사용하기

함수 매개변수부터 반환값까지 전체를 함수 타입으로 선언하여 함수 표현식에 재사용할 수 있음.

```tsx
function roll(sides: number): number {} // 문장
const roll = function (sidies: number): number {}; // 표현식
const roll = (sides: number): number => {}; // 표현식
```

동일한 타입 시그니처를 가지는 여러 개의 함수를 작성할 때는 매개변수의 타입과 반환 타입을 반복해서 작성하지 말고 함수 전체의 타입 선언을 적용해야 함
