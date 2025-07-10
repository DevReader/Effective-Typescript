## 함수 표현식에 타입 적용하기

- 자바스크립트와 타입스크립트에서는 함수 **문장**과 함수 **표현식**을 다르게 인식함
  - 함수 문장: `function rollDice1(sides: number): number { }`
  - 함수 표현식: `const rollDice2 = function(sides: number): number { }`, `const rollDice3 = (sides: number): number => { }`
- 타입스크립트에서는 함수의 매개변수부터 반환값까지 전체를 함수 타입으로 선언하여 함수 표현식에 재사용할 수 있다는 장점이 있음
- 같은 타입 시그니철르 반복적으로 작성한 코드가 있다면 함수 타입을 분리해내거나 이미 존재하는 타입을 찾아봐야 함
- 다른 함수의 시그니처를 참조하려면 `typeof fn` 을 사용하면 됨
