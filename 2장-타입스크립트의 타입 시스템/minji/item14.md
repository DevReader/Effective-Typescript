## 타입 연산과 제너릭 사용으로 반복 줄이기

- 타입에 이름 붙이기

  ```typescript
  // Before
  function distance(a: {x: number, y: number}, b: {x: number, y: number}) { ... }

  // After
  interface Point2D {
    x: number;
    y: number;
  }
  function distance(a: Point2D, b: point2D) { ... }
  ```

- 함수의 시그니처를 타입으로 분리하기

  ```typescript
  // Before
  function get(url: string, opts: Options): Promise<Response> { ... }
  function post(url: string, opts: Options): Promise<Response> { ... }

  // After
  type HTTPFunction = (url: string, opts: Options) => Promise<Response>;
  const get: HTTPFunction = (url, opts) => { ... };
  const post: HTTPFunction = (url, opts) => { ... };

  ```

- 한 인터페이스가 다른 인터페이스를 확장

  ```typescript
  type Person {
    firstName: string;
    lastName: string;
  }

  type PersonWithBirthdate extends Person {
    birth: Date;
  }
  ```

  일반적이진 않지만 & 연산자 사용 가능(유니온 타입에 속성을 추가하려 할 떄 유리)

  ```typescript
  type PersonWithBirthdate = Person & { birth };
  ```

- 매핑된 타입 사용하기 -> Pick 제너릭 타입

  ```typescript
  // before
  interface State {
    userId: string;
    pageTitle: string;
    recentFiles: string[];
    pageContents: string;
  }

  interface TopNavState {
    userId: string;
    pageTitle: string;
    recentFiles: string;
  }

  // after
  type TopNavState = Pick<State, "userId" | "pageTitle" | "recentFiles">;
  ```

- 매개 변수의 타입은 동일하지만 선택적 필드가 되는 경우 기존 타입 활용하기 -> Partial 제너릭 타입

  Partial 제너릭 타입은 `type OptionUpdate = { [k in keyof Options]?: Options[k] };` 와 같음

  ```typescript
  // before
  interface Options {
    width: number;
    height: number;
    color: string;
    label: string;
  }

  interface OptionUpdate {
    width?: number;
    height?: number;
    color?: string;
    label?: string;
  }

  class UIwidget {
    constructor(init: Options) { ... }
    update(options: OptionUpdate) { ... }
  }

  // after
  class UIwidget {
    constructor(init: Options) { ... }
    update(options: Partial<Options>) { ... }
  }
  ```
