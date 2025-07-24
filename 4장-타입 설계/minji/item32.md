## 유니온의 인터페이스보다는 인터페이스의 유니온을 사용하기

- 유니온의 인터페이스 vs 인터페이스의 유니온

  ```typescript
  // 유니온의 인터페이스
  interface Layer {
    type: "fill" | "line" | "point";
    layout: FillLayout | LineLayout | PointLayout;
    paint: FillPaint | LinePaint | PointPaint;
  }

  // 인터페이스의 유니온
  interface FillLayer {
    type: "fill";
    layout: FillLayout;
    paint: FillPaint;
  }
  interface LineLayer {
    type: "line";
    layout: LineLayout;
    paint: LinePaint;
  }
  interface PointLayer {
    type: "point";
    layout: PointLayout;
    paint: PointPaint;
  }

  type Layer = FillLayer | LineLayer | PointLayer;
  ```

  - 유니온의 인터페이스의 경우 layout 은 FillLayout 인데 paint 는 LinePaint 인 무효한 상태를 가질 수 있어 문제가 됨
  - interface에서 type 속성은 태그로 런타임에 어떤 타입의 Layer 가 사용되는지 판단하는데 도움을 주고, 타입의 범위를 좁히는데 사용됨

- 여러 개의 선택적 필드가 동시에 값이 있거나 동시에 undefined 인 경우에도 태그된 유니온 패턴이 잘 맞음
  ```typescript
  interface Person {
    name: string;
    birth?: {
      // placeOfBirth, dateOfBirth 로 쓰는 대신 묶기
      place: string;
      date: Date;
    };
  }
  ```
