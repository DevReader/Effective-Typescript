## [ITEM 53] 타입스크립트 기능보다는 ECMAScript 기능을 사용하기

자바스크립트에 새로 추가된 기능과, 초기 타입스크립트가 독립적으로 개발했던 기능의 호환성 문제로 피해야할 몇가지 기능들

### 열거형(enum)

- 열거형은 상황에 따라 다르게 동작한다.
  - 숫자 열거형에 0,1,2 외의 다른 숫자가 할당되면 위험하다.
  - 상수 열거형은 보통의 열거형과 달리 런타임에서 완전히 제거된다.
  - preserveConstEnums 플래그를 설정한 상태의 상수 열거형은 보통의 열거형처럼 상수 열거형 정보를 유지한다.
  - 문자열 열거형은 명목적 타이핑을 사용한다. (타입의 이름이 같아야 할당 허용)
- 열거형 대신 리터럴 타입의 유니온을 사용한다.

  ```tsx
  type Flavor = "vanilla" | "chocolate" | "strawberry";

  let flavor: Flavor = "chocolate";
  flavor = "mint chip"; //~~~~ "mint chip" 유형은 'Flavor' 유형에 할당할 수 없습니다.
  ```

### 매개변수 속성

- 아래 예시에서 public name은 매개변수 속성이다.

```tsx
class Person {
  constructor(public name: string) {}
}
```

- 일반 속성

```tsx
class Person {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
}
```

- 일반 속성과 매개변수 속성 중 한 가지를 통일성 있게 사용하는 것이 좋다.

### 네임스페이스와 트리플 슬래시 임포트

- 타입스트립트 자체 모듈 시스템 → module 키워드, 트리플 슬래시 임포트
- ECMAScript 공식 모듈 시스템과 호환되기 위한 namespace 키워드 추가 (호환을 위함. 기본적으로는 import, export를 사용해야 함)

```tsx
namespace foo {
  function bar() {}
}

/// <reference path="ohter.ts"/>
foo.bar();
```

### 데코레이터

클래스, 메서드, 속성에 annotation을 붙이거나 기능을 추가하는 데 사용

```tsx
class Greeter {
  greeting: string;
  constructor(message: string) {
    this.greeting = message;
  }
  @logged
  greet() {
    return "Hello, " + this.greeting;
  }
}

function logged(target: any, name: string, descriptor: PropertyDescriptor) {
  const fn = target[name];
  descriptor.value = function () {
    console.log(`Calling ${name}`);
    return fn.apply(this, arguments);
  };
}

console.log(new Greeter("Dave").greet());
// 출력:
// Calling greet
// Hello, Dave
```

- tsconfig.json에 experimentalDecorators 속성 추가
- 표준화 X → 데코레이터가 표준이 되기 전에는 타입스크립트에서 데코레이터를 사용하지 않는 게 좋다
