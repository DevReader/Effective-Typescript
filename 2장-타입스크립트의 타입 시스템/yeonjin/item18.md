## [ITEM 18] 매핑된 타입을 사용하여 값을 동기화하기

객체의 동일성을 판단하는 방식

- 산점도 그리기 위한 UI 컴포넌트 작성 예시

```tsx
interface ScatterProps {
  // Value
  xs: number[];
  ys: number[];

  // Display
  xRange: [number, number];
  yRange: [number, number];
  color: string;

  // Event
  onClick: (x: number, y: number, index: number) => void;
}
```

### 실패에 닫힌 접근법

```tsx
function shouldUpdate(oldProps: ScatterProps, newProps: ScatterProps) {
  let k: keyof ScatterProps;
  for (k in oldProps) {
    if (oldProps[k] !== newProps[k]) {
      if (k !== "onClick") return true;
    }
  }
  return false;
}
```

- 단점: onClick이 아닌 새로운 속성이 추가되면 값이 변경될 때마다 차트를 다시 그리게 됨.

### 실패에 열린 접근법

```tsx
function shouldUpdate(oldProps: ScatterProps, newProps: ScatterProps) {
  oldProps.xs !== newProps.xs ||
    oldProps.ys !== newProps.ys ||
    oldProps.xRange !== newProps.xRange ||
    oldProps.yRange !== newProps.yRange ||
    oldProps.color !== newProps.color;
}
```

- 장점: onClick이 아닌 새로운 속성이 추가되어도 차트를 다시 그리지 않음.
- 단점: 렌더링에 영향을 주는 새로운 속성이 추가되어도 차트를 다시 그리지 않음

### 매핑된 타입과 객체를 사용한 방법

```tsx
const REQUIRES_UPDATE: { [k in keyof ScatterProps]: boolean } = {
  xs: true,
  ys: true,
  xRange: true,
  yRange: true,
  color: true,
  onClick: false,
};

function shouldUpdate(oldProps: ScatterProps, newProps: ScatterProps) {
  let k: keyof ScatterProps;
  for (k in oldProps) {
    if (oldProps[k] !== newProps[k] && REQUIRES_UPDATE[k]) {
      return true;
    }
  }
  return false;
}
```

- 인터페이스에 새로운 속성을 추가할 때, 선택을 강제하도록 매핑된 타입을 고려해야 한다.
- REQUIRES_UPDATE을 수정해서 렌더링 여부를 결정할 수 있다.
  - 렌더링 →  true를, 렌더링 영향 X →  false
