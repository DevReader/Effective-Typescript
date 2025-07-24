## 공식 명칭에는 상표를 붙이기

- 타입을 정교하게 구분하기 위해 brand(상표)를 붙일 수 있음

  ```typescript
  interface Vector2D {
    _brand: "2d";
    x: number;
    y: number;
  }

  function vec2D(x: number, y: number): Vector2D {
    return { x, y, _brand: "2d" };
  }

  function calculateNorm(point: Vector2D) {
    return Math.sqrt(point.x ** 2 + point.y ** 2);
  }

  const myVec2D = vec2D(3, 4);
  const myVec3D = { x: 3, y: 4, z: 1 };

  calculateNorm(myVec2D); // 정상
  calculateNorm(myVec3D); // _brand 속성이 없으므로 에러를 반환
  ```

- 의도를 표현하기 위해서 상표 기법을 사용할 수 있음(ex. 이진 탐색 함수에서 입력 받은 목록이 정렬 되어있음을 표현할 때)
- number 나 string 타입에 상표를 붙여 유용하게 사용할 수 있음
