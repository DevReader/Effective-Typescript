## 일관성 있는 별칭 사용하기

- 별칭을 남발해서 사용하면 제어 흐름 분석이 어려워짐
  ```typescript
  function isPointInPolygon(polygon: Polygon, pt: Coordinate) {
    const box = polygon.bbox;
    if (polygon.box) {
      polygon.box; // polygon.bbox 타입: BoundingBox
      box; // box 타입: BoundingBox | undefined
    }
  }
  ```
- 객체 비구조화를 통해 간결한 이름으로, 일관된 이름 사용하기
  ```typescript
  function isPointInPolygon(polygon: Polygon, pt: Coordinate) {
    const {bbox} = polygon;
    if (bbox) { ... }
  }
  ```
- 함수 호출이 객체 속성의 타입 정제를 무효화할 수 있음 -> 속성보다 지역 변수를 사용하면 타입 정제를 믿을 수 있음
