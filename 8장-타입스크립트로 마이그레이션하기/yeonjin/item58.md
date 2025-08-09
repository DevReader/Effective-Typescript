## [ITEM 58] 모던자바스크립트로 작성하기

### ECMAScript 모듈 사용하기

```tsx
// a.ts
import * as b from "./b";
console.log(b.name);

// b.ts
export const name = "Module B";
```

### 프로토타입 대신 클래스 사용하기

- 프로토타입으로 구현한 Person 객체보다 클래스로 구현한 Person 객체가 문법이 간결하고 직관적임.

```tsx
class Person {
  first: string;
  last: string;

  constructor(first: string, last: string) {
    this.first = first;
    this.last = last;
  }

  getName() {
    return this.first + " " + this.last;
  }
}

const marie = new Person("Marie", "Curie");
const personName = marie.getName();
```

### var 대신 let/const 사용하기

### for(;;) 대신 for-of 또는 배열 메서드 사용하기

- 코드가 짧고 인덱스 변수를 사용하지 않기 때문에 실수를 줄일 수 있다.
- 인덱스가 필요한 경우는 forEach 메서드 사용

```tsx
for (const el of array) {
  // ...
}

array.forEach((el, i) => {
  // ...
});
```

### 함수 표현식보다 화살표 함수 사용하기

- 화살표 함수를 사용하면 상위 스코프의 this를 유지할 수 있다.
- 컴파일 옵션에 noImplicitThis(또는 strict)를 설정하면 타입스크립트가 this 바인딩 관련 오류를 표시해줌

```tsx
class Foo {
  method() {
    console.log(this);
    [1, 2].forEach((i) => {
      console.log(this);
    });
  }
}

const f = new Foo();
f.method();
// 항상 Foo, Foo, Foo을 출력합니다.
```

### 단축 객체 표현과 구조 분해 할당 사용하기

```tsx
const x = 1,
  y = 2,
  z = 3;
const pt = { x, y, z };

const { props } = obj;
const { a, b } = props;
```

### 함수 매개변수 기본값 사용하기

```tsx
function parseNum(str, base = 10) {
  return parseInt(str, base);
}
```

### 저수준 프로미스나 콜백 대신 async/await 사용하기

- 코드가 간결해져 버그나 실수를 방지한다
- 비동기 코드에 타입 정보가 전달되어 타입 추론을 가능하게 한다.

```tsx
async function getJSON(url: string) {
  const response = await fetch(url);
  return response.json();
}
```

### 연관 배열에 객체 대신 Map과 Set 사용하기

```tsx
function countWords(text:string) {
  cosnt counts: {[word: string]: number) = {};
  for (const word of text.split(/[\s,.\+/)) {
   counts[word] = 1 + (counts[word] || 0);
 }
 return counts;
}

console.log(countWords('Objects have a constructor'));

{
 Objects: 1,
 have: 1,
 a: 1,
 constructor: "1function Object() { [native code] }" // 예상하지 못한 동작
}

// Map 사용하기
function countWordsMap(text: string) {
  const counts = new Map<string, number>();
  for (const word of text.split(/[\s,.\+/)) {
    count.set(wrod, 1 + (counts.get(word) }} 0));
  }
  return counts;
}
```

### 타입스크립트에 use strict 넣지 않기

- 타입스크립트에서 수행되는 안전성 검사가 엄격 모드보다 훨씬 더 엄격한 체크를 하기 때문에, 타입스크립트 코드에서 ‘user strict’는 무의미하다.
