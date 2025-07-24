## [ITEM 27] 함수형 기법과 라이브러리로 타입 흐름 유지하기

- ts의 많은 부분이 js 라이브러리의 동작을 정확히 모델링하기 위해서 개발됨.
- 로대시
  - ts에서는 서드파티 라이브러리를 사용하는 것이 유리
  - 타입 정보를 참고하면서 작업할 수 있음
  ```tsx
  import _ from "lodash";
  const rows = rawRows
    .slice(1)
    .map((rowStr) => _.zipObject(headers, rowStr.split(",")));
  //
  ```
  - 만약 체인을 사용하지 않는다면 다음 예제처럼 뒤에서부터 연산이 수행됨
  ```
  _.c(_.b(_.a(v)));
  ```
  - 체인을 사용한다면 다음처럼 연산자의 등장 순서와 실행 순서가 동일
  ```
  _(v).a().b().c().value();
  ```
  - \_(v)는 값을 래핑(wrap)하고, .value()는 언래핑(unwrap)

### 내장된 Array.prototype.map 대신 \_.map을 사용하려는 이유

- 콜백을 전달하는 대신 속성의 이름을 전달할 수 있음

```tsx
const namesA = allPlayers.map((player) => player.name); // 타입이 string[]
const namesB = _.map(allPlayers, (player) => player.name); // 타입이 string[]
const namesC = _.map(allPlayers, "name"); // 타입이 string[]
```
