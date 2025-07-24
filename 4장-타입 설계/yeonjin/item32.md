## [ITEM 32] 유니온의 인터페이스보다는 인터페이스의 유니온을 사용하기

### Layout 예제

- before
  - 문제: layout이 LineLayout이면서 paint이 FillPaint 인 것은 말이 되지 않음

```tsx
interface Layer {
  layout: FillLayout | LineLayout | PointLayout;
  paint: FillPaint | LinePaint | PointPaint;
}
```

- after
  - 각 Layer는 "구조 정의"이기 때문에 `interface`가 적합
  - 유니온 타입은 `type`으로만 가능
  - **일관성을 유지하고 싶다면, "구조 = interface", "조합 = type"으로 구분하는 것이 실용적**
  - 태그를 추가해서 태그를 참고하여 Layer타입의 범위를 좁힐 수 있음 (태그된 유니온)

```tsx
interface FillLayer {
  layout: FillLayout;
  paint: FillPaint;
}

interface LineLayer {
  layout: LineLayout;
  paint: LinePaint;
}

interface PointLayer {
  layout: PointLayout;
  paiint: PointPaint;
}

type Layer = FillLayer | LineLayer | PointLayer;
```

### 태그된 유니온 패턴

- 여러 개의 선택적 필드가 동시에 값이 있거나 동시에 undefined인 경우에도 태그된 유니온 패턴이 잘 맞음

```tsx
interface Person {
  name: string;
  birth?: {
    place: string;
    date: string;
  };
}
```

- 구조 기반 유니온
  - `구조 기반 유니온`은 태그 없이도 특정 필드의 존재를 기반으로 타입을 좁히는 유연한 방식
  - 외부 API처럼 타입 구조를 수정할 수 없는 경우에는 구조 기반 타입 가드가 실용적

```tsx
interface Name {
  name: string;
}

interface PersonWithBirth extends Name {
  placeOfBirth: string;
  dateOfBirth: Date;
}

type Person = Name | PersonWithBirth;
```
