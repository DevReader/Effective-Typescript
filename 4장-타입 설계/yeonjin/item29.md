## [ITEM 29] 사용할 때는 너그럽게, 생성할 때는 엄격하게

### 함수 시그니처

> 입력은 느슨하게 (수용 범위를 넓게), 출력은 엄격하게 (일관성을 높게)

- before
  - 수많은 선택적 속성을 가지는 반환 타입과 유니온 타입은 viewportForBounds를 사용하기 어렵게 만듦

```tsx
declare function setCamera(camera: CameraOptions): void;
declare function viewportForBounds(bounds: LngLatBounds): CameraOptions;

interface CameraOptions {
  center?: LngLat;
  zoom?: number;
  bearing?: number;
  pitch?: number;
}

type LngLat =
  | { lng: number; lat: number }
  | { lon: number; lat: number }
  | [number, number];

type LngLatBounds =
  | { northeast: LngLat; southwest: LngLat }
  | [LngLat, LngLat]
  | [number, number, number, number];
```

- after
  - LngLat
    - `LngLat`은 내부에서 사용하는 **정규화된 좌표 타입**
    - `LngLatLike`는 외부 입력으로 들어오는 다양한 좌표 표현
  - Camera
    - Camera (엄격한 타입) → CameraOptions (느슨한 타입)
    - Omit<Partial<Camera>, 'center'> (더 유연하게 대체)
    - 구체적인 `Camera`를 유연한 `CameraOptions` 자리에 넣는 것은 타입 호환성 측면에서 문제 없음
  - 사용하기 편한 API일수록 반환 타입이 엄격하다

```tsx
interface LngLat: {lng: number, lat: number} ;
type LngLatLikee = LngLat | {lon: number; lat: number; } | [number, number];

interface Camera {
  center: LngLat;
  zoom: number;
  bearing: number;
  pitch: number;
}

interface CameraOptions extends Omit<Partial<Camera>, 'center'> {
  center?: LngLatLike;
}

// or
interface CameraOptions {
 center?: LngLatLike;
 zoom?: number;
 bearing?: number;
 pitch?: number;
}

type LngLatBounds =
  {northeast: LngLatLike, southwest: LngLatLike} |
  [LngLatLike, LngLatLike] |
  [number, number, number ,number];

declare function setCamera(camera: **CameraOptions**): void;
declare function viewportForBounds (bounds: LngLatBounds): **Camera**;
```
