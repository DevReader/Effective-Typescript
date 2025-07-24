## [ITEM 41] any의 진화를 이해하기

> any 타입의 진화는 noImplicitAny가 설정된 상태에서 변수의 타입이 암시적 any인 경우에만 일어난다.

### any의 진화

- any[] 배열
  - any[]로 선언되었지만, number 타입의 값을넣는 순간부터 number[] 로 진화한다
- 조건문에서 분기

```java
let val ; // 타입 any
if (Math.random() < 0.5) {
  val = /hello/;
  val // 타입 RegExp
} else {
  val = 12;
  val // 타입 number
}
val // 타입 number | RegExp
```

- 초깃값이 null인 경우

```java
let val = null; // 타입 any
try {
  somethingDangerous();
  val = 12;
  val // 타입 number
} catch (e) {
  console.warn('alas!');
}
val // 타입 number | null
```

→ 명시적으로 any를 선언한 경우에는 타입이 그대로 유지됨

→ any 타입의 진화는 암시적 any 타입에 어떤 값을 할당할 때만 발생함

→ 어떤 변수가 암시적 any 상태일 때 값을 읽으려 하면 오류 발생

→ 암시적 any 타입은 함수 호출을 거쳐도 진화하지 않음

→ 타입을 안전하게 지키기 위해선 암시적 any를 진화시키는 방식보다 명시적 타입 구문을 사용하는 것이 더 좋다
