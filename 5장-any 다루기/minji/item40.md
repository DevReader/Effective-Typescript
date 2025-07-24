## 함수 안으로 타입 단언문 감추기

- 함수의 모든 부분을 안전한 타입으로 구성하는 것이 이상적이지만, 불필요한 예외 상황까지 고려해가며 타입 정보를 힘들게 구성할 필요는 없음 -> 함수 내부에는 타입 단언 사용, 함수 외부로 드러나는 타입 정의를 명확하게 명시
- 타입 단언문 사용

  ```typescript
  declare function shalloEqual(a: any, b: any): boolean;

  function shallowObjectEqaul<T extends object>(a: T, b: T): boolean {
    if (Object.keys(a).length !== Object.keys(b).length) {
      return false;
    }

    for (const [k, aVal] of Object.entries(a)) {
      if (!(k in b) || b[k] !== aVal) {
        // ~~~ '{}' 타입에 인덱스 시그니처가 없으므로 요소에 암시적 any 형식이 있습니다.
        return false;
      }
    }

    return true;
  }
  ```

  - 앞에서 b에 k 가 있는지 확인했으므로 (b as any)[k] 로 타입 단언문을 사용해도 됨
