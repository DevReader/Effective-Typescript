## [ITEM 39] any를 구체적으로 변형해서 사용하기

- any 타입에는 모든 숫자, 문자열, 배열, 객체, 정규식, 함수, 클래스, DOM 엘리먼트, null, undefiend까지 포함됨
  - any보다 구체적으로 표현할 수 있는 타입을 찾아 타입 안정성을 높여야 함
- 배열일 때 any보다 any[]를 사용하자
  - 함수 내의 array.length 타입 체크
  - 함수의 반환 타입이 any 대신 number로 추론됨
  - 함수 호출될 때 매개변수가 배열인지 체크

```java
function getLengthBad(array: any) {
  return array.length;
}

function getLength(array: any[]) {
  return array.length;
}
```

- 객체일 때 any → {[key: string]: any}
  - object 타입을 사용할 수도 있음 → 객체의 키를 열거할 수는 있지만 속성에는 접근 불가능
- 함수일 때 any → (…args: any[])
