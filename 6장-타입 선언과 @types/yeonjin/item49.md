## [ITEM 49] 콜백에서 this에 대한 타입 제공하기 (다시 정리)

### 렉시컬 스코프와 다이나믹 스코프

렉시컬 스코프: 함수를 어디서 호출하는지가 아니라 어디에 선언하였는지에 따라 결정되는 것

다이나믹 스코프: 코드를 실행한 위치에 따라 스코프가 결정됨

```tsx
const c = new C();
const method = c.logSquares;
method();

//Uncaught TypeError: undefined의 'vals' 속성을 읽을 수 없습니다.
```

- c.logSquares() → C.prototype.logSquares 호출 & this의 값을 c로 바인딩

### this 바인딩

```tsx
const c = new C();
const method = c.logSquares;
method.call(c); // 제곱 출력
```

### 콜백 함수의 this

```tsx
class ResetButton {
  render() {
    return makeButton({ text: "Reset", onClick: this.onClick });
  }
  onClick() {
    alert(`Reset ${this}`); // 오류
  }
}
```

- 생성자에서 메서드에 this 바인딩

```tsx
class ResetButton {
  constructor() {
    this.onClick = this.onClick.bind(this);
  }

  render() {
    return makeButton({ text: "Reset", onClick: this.onClick });
  }
  onClick() {
    alert(`Reset ${this}`);
  }
}
```

- 화살표 함수 사용

```tsx
class ResetButton {
  render() {
    return makeButton({ text: "Reset", onClick: this.onClick });
  }
  onClick = () => {
    alert(`Reset ${this}`); // this가 항상 인스턴스를 참조합니다
  };
}
```

### 콜백 함수의 `this` 바인딩을 정확하게 선언하는 방법

- 콜백 함수의 매개변수에 this를 추가하고, 콜백 함수를 call로 호출해서 해결
  - this 바인딩이 체크되기 때문에 실수를 방지할 수 있다.
  - 라이브러리 사용자의 콜백 함수에서 this를 참조할 수 있고 완전한 타입 안정성을 얻을 수 있다.
  - 콜백을 화살표 함수로 작성하고 this를 참조하려고 할 때 문제를 확인할 수 있다.

```tsx
function addKeyListener(
  el: HTMLElement,
  fn: (this: HTMLElement, e: KeyboardEvent) => void
) {
  el.addEventListener("keydown", (e) => {
    fn.call(el, e);
  });
}
```
