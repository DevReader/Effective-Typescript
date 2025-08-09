## [ITEM 61] 의존성 관계에 따라 모듈 단위로 전환하기

- 다른 모듈에 의존하지 않는 최하단 모듈부터 작업을 시작해서 의존성의 최상단에 있는 모듈을 마지막으로 완성해야 한다.
- 서드파티 라이브러리 타입 정보를 가장 먼저 해결해야한다.
- 마이그레이션 할 때는 타입 정보 추가만 하고, 리팩토링을 해서는 안 된다. (나중에 리팩)

### 선언되지 않은 클래스 멤버

- 자바스크립트는 클래스 멤버 변수를 선언할 필요가 없지만, 타입스크립트에서는 명시적으로 선언해야 함

### 타입이 바뀌는 값

- 한번에 생성하기 곤란한 경우에는 타입 단언문을 사용할 수도 있음

```tsx
const state = {};
state.name = 'New York'; // 오류
state.capital = 'Albany'; //오류

// 한번에 생성
const state = {
 name: 'New York';
 capital: 'Albany';
}; // 정상

//타입 단언문 사용
interface State {
  name: string;
  capital: string;
}

const state = {} as State;
state.name = 'New York';
state.capital = 'Albany';

```
