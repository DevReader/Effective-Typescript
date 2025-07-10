## [ITEM 7] 타입이 값들의 집합이라고 생각하기

- 타입: 할당 가능한 값들의 집합

  1. 공집합: never 타입. 아무런 값도 할당할 수 없음
  2. 유닛 타입(리터럴 타입): 한 가지 값만 포함하는 타입.
  3. 유니온 타입: 값 집합들의 합집합. `type AB = 'A' | 'B';`

- &연산자는 두 타입의 인터섹션(교집합)을 계산함. 타입 연산자는 인터페이스의 속성을 아닌 값의 집합에 적용됨.

```tsx
keyof (A&B) = (keyof A) | (keyof B)
keyof (A|B) = (keyof A) & (keyof B)
```

- extends
  - ~에 할당 가능한, ~의 부분 집합

```tsx
interface Person {
  name: string;
}

interface PersonSpan extends Person {
  birth: Date;
  death?: Date;
}
```

- 타입이 엄격한 상속 관계가 아닐 때는 집합 스타일로 **설계**하는 것이 바람직함.
  - number[]는 [number, number]의 부분집합이 아니기 때문에 할당 불가능
  - [number, number]는 number[]에 할당 가능

```tsx
const list = [1, 2]; //type은 number[]
const tuple: [number, number] = list; // type 오류
```

- 타입스크립트의 타입 시스템은
  - 구조적(Structural) 타입 시스템 → 값의 구조(형태)를 기반으로 타입을 판단
  - 소수/정수, 정확한 속성 개수, 런타임 값을 기반으로 한 제한을 표현할 수는 없음
