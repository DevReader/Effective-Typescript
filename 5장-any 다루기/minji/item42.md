## 모르는 타입의 값에는 any 대신 unknown을 사용하기

- unknown 타입은 모든 타입을 할당 받을 수 있지만, 오직 unknown 과 any 타입에만 unknown 을 할당할 수 있음
- unknown 타입인 채로 값을 사용거나 unknown 값에 함수를 호출하려고 하거나 연산을 하려고 하면 오류가 발생함
- unknown 타입 에러는 타입 단언을 통해 타입을 지정하거나, instanceof 를 체크해서 원하는 타입으로 변환하여 해결 가능함
- unknwon 타입과 유사하게는 {} 타입, object 타입이 있음
  - {} 타입: null 과 undefined 를 제외한 모든 값을 포함함
  - object 타입: 모든 비기본형 타입으로 이루어짐. true, 12, "foo" 는 포함되지 않고 객체와 배열은 포함됨
- unknown 타입 도입 이전에는 {}가 더 일반적이었지만 최근에는 {}를 사용하는 경우는 꽤 드문 편
