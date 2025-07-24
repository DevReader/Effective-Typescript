## 사용할 때는 너그럽게, 생성할 때는 엄격하게

- 함수의 매개변수는 타입의 범위가 넓어도 되지만, 결과를 반환할 때는 일반적으로 타입의 범위가 더 구체적이어야 함
  - 반환 타입의 범위가 넓으면 안전한 타입으로 사용하기 위해 유니온 타입의 각 요소별로 코드를 분기해야 하므로 불편해짐
- 매개변수와 반환 타입의 재사용을 위해서 기본 형태(반환 타입)와 느슨한 형태(매개변수 타입)를 도입하는 것이 좋음

  ```typescript
  interface LatLng {
    lat: number;
    lng: number;
  }
  type LatLngLike = LatLng | { lat: number; lon: number } | [number, number];

  // 반환 타입
  interface Camera {
    center: LatLng;
    zoom: number;
    bearing: number;
    pitch: number;
  }

  // 매개 변수 타입
  interface CameraOptions {
    center?: LatLngLike;
    zoom?: number;
    bearing?: number;
    pitch?: number;
  }

  // 사용
  declare function setCamera(camera: CameraOptions): void;
  declare function viewportForBounds(bound: LatLngBounds): Camera;
  ```

  - CameraOptions 의 경우 아래처럼 적을 수 있음
    ```typescript
    interface CameraOptions extends Omit<Partial<Camera, "center">> {
      center?: LatLngLike;
    }
    ```
  - `Omit`: 두 개의 제네릭 타입을 받아 앞의 모든 프로퍼티를 선택한 다음 뒤 프로퍼티를 제거한 타입을 구성하는 유틸리티 타입
  - `Partial`: 한 개의 제네릭 타입을 받아 모든 프로퍼티를 옵셔널하게 변경하는 유틸리티 타입
