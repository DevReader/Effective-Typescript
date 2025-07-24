## string 타입보다 더 구체적인 타입 사용하기

- string 은 매우 넓은 타입이므로, 구체적인 타입을 사용할 수 있다면 구체적인 타입을 쓰는 것이 좋음

  ```typescript
  // stringly typed
  interface Album {
    artist: string;
    title: string;
    releaseDate: string; // YYYY-MM-DD
    recordingType: string; // "live" 또는 "studio"
  }

  // 문자열 리터럴 타입의 유니온으로 구체화
  type RecordingType = "live" | "studio";

  interface Album {
    artist: string;
    title: string;
    releaseDate: Date;
    recordingType: RecordingType;
  }
  ```

- 장점
  - 타입을 명시적으로 정의함으로써 다른 곳으로 값이 전달되어도 타입 정보가 유지됨
  - 타입을 명시적으로 정의하고 해당 타입의 의미를 설명하는 주석을 붙여넣을 수 있음(아이템 48)
  - keyof 연산자로 더욱 세밀하게 객체의 속성 체크가 가능해짐
- 객체의 속성 이름을 함수 매개변수로 받을 때는 string 보다는 keyof T 사용하기
