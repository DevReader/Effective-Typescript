## 코드 생성과 타입이 관계없음을 이해하기

- 타입스크립트의 컴파일러는 최신 타입스크립트/자바스크립트를 브라우저에서 동작할 수 있도록 구버전의 자바스크립트를 트랜스파일하는 것과 코드의 타입오류를 체크하는 두 가지 역할을 수행함
- 두 가지 역할은 완벽히 독립적으로, 타입스크립트가 자바스크립트로 변활될 때 코드 내의 타입에는 영향을 주지 않음

### 타입 오류가 있는 코드도 컴파일이 가능합니다

- 컴파일은 타입 체크와 독립적으로 동작하기 떄문에 타입 오류가 있어도 컴파일이 가능함
- 이 덕분에 웹 애플리케이션을 만들면서 어떤 부분에 문제가 발생하더라도 여전히 컴파일된 산출물이 나오기 때문에 다른 부분을 테스트할 수 있음
- 오류가 있을 때 컴파일하지 않으려면 `noEmitOnError`를 설정하거나 빌드 도구에 동일하게 적용하면 됨

### 런타임에는 타입 체크가 불가능합니다

- 타입스크립트 타입은 제거 가능하므로 자바스크립트로 컴파일되는 과정에서 모든 인터페이스, 타입, 타입 구문은 제거 되어버림
- 런타임에 타입 정보를 유지하려면 속성이 있는지를 체크하거나 런타입에 접근 가능한 타입 정보를 명시적으로 저장하는 '태그' 기법을 사용하면 됨

  ```typescript
  // 속성 있는지 체크
  interface Square {
    width: number;
  }

  interface Rectangle extends Square {
    height: number;
  }

  type Shape = Square | Rectangle;

  function calcArea(shape: Shape)  {
    if ('height' in shape) {
      ...
    }
  }

  // 태그 기법
  interface Square {
    kind: 'square';
    width: number;
  }

  interface Rectangle {
    kind: 'rectangle';
    width: number;
    height: number;
  }

  type Shape = Square | Rectangle;

  function calcArea(shape: Shape)  {
    if (shape.kind === 'rectangle') {
      ...
    }
  }
  ```

- 타입(런타임 접근 불가)과 값(런티임 접근 가능)을 둘다 사용하려면 타입을 클래스로 만들면 됨

  ```typescript
  class Square {
    constructor(public width: number) {}
  }

  class Rectangle extends Square {
    constructor(public width: number, public height: number) {
      super(width)
    }
  }

  type Shape = Square | Rectangle;

  function calcArea(shape: Shape) {
    if (shape instanceof Rectangle) {
      ...
    }
  }
  ```

- `Shape` 타입은 태그된 유니온의 예시로, 런타임에 타입 정보를 손쉽게 유지할 수 있는 방법

### 타입 연산은 런타임에 영향을 주지 않습니다

- `as number`는 타입 연산이고 런타임 동작에는 아무런 영향을 주지 않음
- 값을 정제하기 위해서는 런타임의 타입을 체크해야 하고 자바스크립트 연산을 통해 변환을 수행해야 함

### 런타임 타입은 선언된 타입과 다를 수 있습니다

- 런타임에는 타입 선언문이 제거되므로 언제든지 달라질 수 있음

### 타입스크립트 타입으로는 함수를 오버로드할 수 없습니다

- 타입과 런타임의 동작이 무 관하기 떄문에 함수 오버로딩이 불가능
- 하나의 함수에 대해 여러개의 선언문을 작성할 수는 있지만 구현체는 오직 하나뿐임

### 타입스크립트 타입은 런타임 성능에 영향을 주지 않습니다

- 런타임 오버헤드가 없는 대신 빌드타임 오버헤드가 있음. 오버헤드가 커지면 빌드 도구에서 transpile only 를 설정하여 타입 체크를 건너뛸 수 있음
- 오래된 런타임 환경을 지원하기 위해 호환성을 높이고 성능 오버헤드를 감안할지, 호환성을 포기하고 성능 중심의 네이티브 구현체를 선택할지의 문제에 맞닥뜨릴 수 있음
