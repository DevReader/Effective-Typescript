## [ITEM 38] any 타입은 가능한 한 좁은 범위에서만 사용하기

> 의도치 않은 타입 안정성 손실을 피하기 위해 any의 사용 범위를 최소한으로 좁혀야 한다

### 함수에서 any의 사용

- bad

```java
function f1() {
  const x: any = expressionReturningFoo();
  processBar(x);
}
```

- good
  - any 타입이 processBar 함수의 매개변수에서만 사용된 표현식이어서 다른 코드에 영향을 미치지 않음

```java
function f2() {
    const x = expressionReturningFoo();
    processBar(x as any);
}
```

- 함수의 반환 타입을 추론할 수 있는 경우에도 함수의 반환 타입을 명시하는 게 좋음
  → any 타입이 함수 바깥으로 영향을 미치는 것을 방지할 수 있다

### 객체와 any

- bad: 객체 전체를 any로 단언하면 다른 속성들도 타입 체크가 되지 않음

```java
const config: Config = {
   a: 1,
   b: 2,
   c: {
     key: value
   }
} as any;
```

- good: 최소한의 범위에서 any 사용

```java
const config: Config = {
  a: 1,
  b: 2,
  c: {
    keyL value as any
  }
};
```
