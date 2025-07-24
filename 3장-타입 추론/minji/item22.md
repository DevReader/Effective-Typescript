## 타입 좁히기

- 타입 좁히기: 타입스크립트가 넓은 타입으로부터 좁은 타입으로 진행하는 과정
- 타입 좁히기 예제1: null 체크
  ```typescript
  const el = document.getElementById("foo"); // el 타입: HTMLElement | null;
  if (el) {
    el; // el 타입: HTMLElement
    el.innerHTML = "Party Time".blink();
  } else {
    el; // el 타입: null
    alert("#foo 없엉");
  }
  ```
  일반적으로 이런 조건문에서 타입 좁히기를 잘 해내지만, 타입 별칭이 있는 경우 잘 못 해낼 수도 있음
- 타입 좁히기 예제2: 예외 던지기 / 함수 반환하기
  ```typescript
  const el = document.getElementById("foo"); // el 타입: HTMLElement | null;
  if (!el) throw new Error("#foo 찾을 수 없음");
  el; // el 타입: HTMLElement
  el.innerHTML = "Party Time".blink();
  ```
- 타입 좁히기 예제3: instanceof 사용하기
  ```typescript
  function contains(text: string, search: string | RegExp) {
    if (search instanceof RegExp) {
      search; // search 타입: RegExp
      return !!search.exec(text);
    }
    search; // search 타입: string
    return text.includes(search);
  }
  ```
- 타입 좁히기 예제4: 속성 체크
  ```typescript
  interface A { a: number }
  interface B { b: number }
  function pickAB(ab: A | B) {
    if ('a' in ab) {
      ab // ab 타입: A
    }
    ...
  }
  ```
- 타입 좁히기 예제5: 일부 내장 함수 (ex. Array.isArray)
  ```typescript
  function contains(text: string, terms: string | string[]) {
    const termList = Array.isArray(terms) ? terms : [terms];
    termList; // termList 타입: string[]
  }
  ```
- 타입 좁히기 예제6: 명시적 태그 붙이기 -> 태그된 유니온(구별된 유니온) 패턴

  ```typescript
  interface UploadEvent { type: 'upload'; filename: string; contents: string }
  interface DownloadEvent { type: 'download'; filename: string }
  type AppEvent = UploadEvent | DownloadEvent;
  function handleEvent(e: AppEvent) {
    switch (e.type) {
      case 'download':
        e // e 타입: DownloadEvent
        break;
      case 'upload';
        e // e 타입: UploadEvent
        break;
    }
  }

  ```

- 타입을 섣불리 판단하지 않도록 주의해야 함
  ```typescript
  const el = document.getElementById('foo'); // el 타입: HTMLElement | null
  if (typeof el === 'object') { ... } // null 또한 'object' 이므로 null 값을 제외시킬 수 없음
  ```
- 타입스크립트가 타입을 잘 식별하지 못할 경우 식별을 돕기 위해 커스텀 함수 도입 가능 -> 사용자 정의 타입 가드
  ```typescript
  function isInputElement(el: HTMLElement): el is HTMLInputElement {
    return "value" in el;
  }
  function getElement(el: HTMLElement) {
    if (isInputElement(el)) {
      el; // el 타입: HTMLInputElement
      return el.value;
    }
    el; // el 타입: HTMLElement
    return el.textContent;
  }
  ```
