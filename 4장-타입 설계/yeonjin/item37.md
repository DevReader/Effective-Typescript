## [ITEM 37] 공식 명칭에는 상표를 붙이기

> 값을 구분하기 위해 공식 명칭이 필요하다면 상표를 붙이는 것을 고려해야 한다

- 공식 명칭 개념을 타입스크립트에서 흉내 내려면 상표를 붙이면 된다
- 상표 기법은 타입 시스템에서 동작하지만 런타임에 상표를 검사하는 것과 동일한 효과를 얻을 수 있다
- 브랜딩 타입의 \_brand 필드는 타입 시스템 안에서만 존재하는 "가짜 필드"이며,
  실제 객체가 해당 속성을 가지고 있을 필요는 없다

```tsx
interface Vector2D {
 _brand: '2d';
 x: number;
 y: number;
}

function vec2D(x:number, y:number): Vector2D {
  return {x, y, _brand: '2d'};
```

### 상대경로 절대경로 예제

```tsx
type AbPath = string & {_brand: 'abs'};
function listAbPath(path: AbPath) {
  //...
}
function isAbPath(path:string): path is AbPath {
  return path.startsWith('/');
}

function f(path: string) {
  if (isAbPath(path)) {
      listAbPath(path);
  }
  listAbPath(path); // 에러
```
