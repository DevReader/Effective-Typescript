## 아이템 49

### Lexical Scope & Dynamic Scope

- Lexical Scope: 함수를 어디에 선언했는지에 따라 상위 스코프가 결정되는 것. 정적 스코프라고 부르기도 함

  ```js
  var x = 1;

  function foo() {
    var x = 10;
    bar();
  }

  function bar() {
    console.log(x);
  }

  foo(); // 1 출력
  bar(); // 1 출력
  ```

- Dynamic Scope: 함수를 어디에서 호출했냐에 따라 상위 스코프가 결정되는 것. 위 예제에서 경우 bar가 호출된 곳이 foo 안쪽일 때는 상위 스코프를 foo 영역으로 봐서 10이 출력되게 됨
- this 는 전형적으로 객체의 현재 인스턴스를 참조하는 클래스에서 가장 많이 사용됨

  ```js
  class C {
    vals = [1, 2, 3];
    logSquares() {
      for (const val of this.vals) {
        console.log(val * val);
      }
    }
  }

  const c = new C();
  c.logSquares(); // 정상적으로 호출

  const method = c.logSquares();
  method(); // undefined의 'vals' 속성을 읽을 수 없다는 에러 발생
  method.call(); // 명시적으로 this 를 바인딩함
  ```

- 위 예제에서 this는 반드시 C의 인스턴스에 바인딩되어야 하는 것이 아니고 어떤 것이든 바인딩할 수 있음 -> 일부 라이브러리들은 API의 일부에서 this 값을 사용할 수 있게 함(ex. DOM)

- 콜백 함수에서도 this는 자주 사용됨

  ```js
  declare function makeButton(props: {
    text: string,
    onClick: () => void,
  }): void;
  class ResetButton {
    render() {
      return makeButton({ text: "Reset", onClick: this.onClick });
    }
    onClick() {
      alert(`Reset ${this}`); // this 바인딩 문제로 인해 Reset 이 정의되지 않았습니다라는 경고가 뜸
    }
  }
  ```

  ```js
  declare function makeButton(props: {
    text: string,
    onClick: () => void,
  }): void;
  class ResetButton {
    // `onClick() { ... }`은 ResetButton.Prototype의 속성을 정의하며 리셋버튼의 모든 인스턴스에 공유되지만 이처럼 생성자에서 바인딩하면 onClick 속성에 this가 바인딩되어 해당 인스턴스에만 생김
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

  ```js
  declare function makeButton(props: {
    text: string,
    onClick: () => void,
  }): void;
  class ResetButton {
    render() {
      return makeButton({ text: "Reset", onClick: this.onClick });
    }
    onClick = () => {
      // 화살표 함수로 바꾸면 ResetButton이 생성될 때마다 제대로 바인딩된 this를 가지는 새 함수를 생성함
      alert(`Reset ${this}`);
    };
  }
  ```

- this 바인딩은 자바스크립트의 동작이기 때문에 타입스크립트에서도 this 바인딩을 그대로 모델링하게 됨
- 콜백 함수 매개변수에 this를 추가한 다음 call 로 호출하면 인수 호출 오류와 this 바인딩 체크 실수를 방지할 수 있고, 라이브러리 사용자도 this를 참조할 수 있어 타입 안정성을 얻을 수 있음
- 콜백함수에서 this를 쓰면 API의 일부가 되는 것이므로 반드시 타입 선언에 포함해야 함
