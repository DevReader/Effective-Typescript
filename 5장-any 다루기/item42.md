## [ITEM 42] 모르는 타입의 값에는 any 대신 unknown을 사용하기

### any와 unknown

- any
  - 어떤 타입이든 any 타입으로 할당 가능하다
  - any 타입은 어떠한 타입으로도 할당 가능하다 (never 제외)
- unknown
  - 어떤 타입이든 unknown에 할당 가능
  - unknown은 오직 unknown과 any에만 할당 가능

값이 있지만 그 타입을 모르는 경우 unknown을 사용한다

### unknown에서 원하는 타입으로 변환

- 타입 단언문
- instanceof
- 사용자 정의 타입 가드

### 제네릭보다는 unknown을 반환

제너릭보다 unknown을 반환하고 사용자가 직접 단언문을 사용하거나 원하는 대로 타입을 좁히도록 강제하는 것이 좋음

```java
function safeParseYAML<T>(yaml: string): T {
  return parseYAML(yaml);
}
```

### 이중 단언문에서 사용

```java
declare const foo: Foo;
let betAny = foo as any as Bar;
let barUnk = foo as unknown as Bar;
```

기능적으로 동이하지만, 단언문을 분리하는 리팩토링을 한다면 unknown 형태가 더 안전하다

### unknown과 object, {}

- {} 타입은 null과 undefined를 제외한 모든 값을 포함한다
  - null과 undefined가 불가능하다고 판단되는 경우에만 unknown 대신 {}를 사용하면 된다
- object 타입은 모든 비기본형 타입으로 이루어진다
