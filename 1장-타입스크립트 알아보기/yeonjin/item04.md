## [ITEM 4] 구조적 타이핑에 익숙해지기

### 덕 타이핑

객체가 어떤 타입에 부합하는 변수와 메서드를 가질 경우 객체를 해당 타입에 속하는 것으로 간주하는 방식

### 구조적 타이핑

타입스크립트도 매개변수 값이 요구사항을 만족한다면 타입이 무엇인지 신경쓰지 않는 동작을 그대로 모델링

### 구조적 타이핑의 장점

```tsx
interface PostgresDB {
  //..
  runQuery: (sql: string) => any[];
}
interface Author {
  first: string;
  last: string;
}

function getAuthors(db: PostgresDB): Author[] {
  //..
}
```

```tsx
interface DB {
  runQuery: (sql: string) => any[];
}

function getAuthors(db: DB): Author[] {
  //..
}
```

getAuthors 함수를 테스트하기 위해 모킹한 PostgresDB를 생성하지 않아도 됨.

PostgresDB가 DB 인터페이스를 구현하는지 명확히 선언할 필요가 없음.

추상화함으로써 로직과 테스트를 특정한 구현으로부터 분리.
