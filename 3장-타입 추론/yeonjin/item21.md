## [ITEM 21] 타입 넓히기

### 넓히기

지정된 단일 값을 통해 할당 가능한 값들의 집합을 유추한다

### 넓히기 제어하기

- const 사용

```tsx
interface Vector3 {
  x: number;
  y: number;
  z: number;
}
function getComponent(vector: Vector3, axis: "x" | "y" | "z") {
  return Vector[axis];
}

const x = "x";
let vec = { x: 10, y: 10, z: 30 };
getComponent(vec, x); // 정상
```

- 명시적 타입 구문 제공하기

```tsx
const v: { x: 1 | 3 | 5 } = { x: 1 };
```

- 타입 체커에 추가적인 문맥 제공하기
- const 단언문 사용하기

```tsx
cosnt v3 = { x:1, y:2, } as const;
```
