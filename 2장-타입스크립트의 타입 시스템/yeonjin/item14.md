## [ITEM 14] 타입 연산과 제너릭 사용으로 반복 줄이기

### 타입에 이름 붙여서 타입 중복 제거하기

- 기존 코드

```tsx
function distance(a: { x: number; y: number }, b: { x: number; y: number }) {
  return Math.sqrt(Math.pow(a.x - b.x, 2) + Math.pow(a.y - b.y, 2));
}
```

- 타입에 이름 붙이기

```tsx
interface Point2D {
  x: number;
  y: number;
}

function distance(a: Point2D, b: Point2D) {}
```

### 인터페이스 확장해서 반복을 제거하기

- 확장해서 추가 필드만 작성하기

```tsx
interface Person {
  firstName: string;
  lastName: string;
}

interface PersionWithBirthDate extends Person {
  birth: Date;
}
```

- 두 인터페이스가 필드의 부분 집합을 공유한다면, 공통 필드만 골라서 기반 클래스로 분리할 수도 있음.

### 인덱싱해서 타입 중복 제거하기

```tsx
interface State {
  userId: string;
  pageTitle: string;
  recentFiles: string[];
  pageContents: string;
}

interface TopNavState {
  userId: string;
  pageTitle: string;
  recentFiles: string[];
}
```

- State 인덱싱 (여전히 중복 존재)

```tsx

type TopNavState {
  userId: State['userId'];
  pagetitle: State['pageTitle'];
  recentFiles: State['recentFiles'];
}
```

- 매핑된 타입 사용하기

```tsx
type TopNavState = {
  [k in "userId" | "pageTitle" | "recentFiles"]: State[k];
};
```

- Pick (표준 라이브러리 사용하기)

```tsx
type TopNavState = Pick<State, "userId" | "pageTItle" | "recentFiles">;
```

### 태그된 유니온에서 중복 제거하기

```tsx
interface SaveAction {
    type: 'save'
    //...
}

interface LoadAction {
    type: 'load';
    // ..
}

type Action = SaveAction | LoadAction;
type ActionType = 'save' | 'load' // 타입 반복
-> type ActionType = Action['type']
```

### 업데이트되는 클래스 작성에서 중복 제거하기

```tsx
type OPtionsUpdate = {k in keyof OPtions]?: Options[k]};

// type OptionsKeys = keyof Options;
```

→ partial 사용해서 쉽게 만들 수 있음

### 값의 형태에 해당하는 타입 정의하기

```tsx
const ONIT_OPTIONS = {
		width: 540,
		height: 450,
		color: "'#FFFFFF',
		label: 'bbbb',
}

type Options = typeof INIT_OPTIONS;
```

### 함수나 메서드의 반환값에 명명된 타입 만들기

```tsx
type UserInfo = ReturnType<typeof getUserInfo>;
```

### 제네릭 타입에서 매개변수 제한하기

- extends를 사용

```tsx
inteface Name {
    first: string;
    last: string;
}

type DancingDuo<T extends Name> = [T,T];
```
