## [ITEM 56] 정보를 감추는 목적으로 private 사용하지 않기

- 자바스크립트는 클래스에 비공개 속성을 만들 수 없다.
- 비공식적으로 앞에 \_언더스코어를 붙여서 비공개 속성을 표현했다. (실제론 접근할 수 있음)
- ts의 public, protected, priavte 키워드는 컴파일 이후에 제거된다.
- 정보를 감추기 위해 private을 사용하면 안 된다.

### 클로저 사용하기

- 정보를 숨기기위한 방법
- 외부에서 passwordHash 변수에는 접근할 수 없다.
- passwordHash에 접근해야 하는 메서드는 생성자 내부에 정의되어야 한다.
- 메서드 정의가 생성자 내부에 존재하면 인스턴스를 생성할 때마다 각 메서드의 복사본이 생성되기 때문에 메모리를 낭비하게 된다는 것을 기억해야 한다.
- 동일한 클래스로부터 생성된 인스턴스라고 하더라도 서로의 비공개 데이터에 접근하는 것이 불가능하다. (일반적인 객체지향 언어에서는 클래스 단위 비공개이지만, 클로저 방식은 인스턴스 단위 비공개)

```tsx
declare function hash(text: string): number;

class PasswordChecker {
  checkPassword: (password: string) => boolean;
  constructor(passwordHash: number) {
    this.chekcPassword = (password: string) => {
      return hash(password) === passwordHash;
    };
  }
}

const checker = new PasswordChecker(hash("s3cret"));
checker.checkPassword("s3cret"); // 결과는 true
```

### 비공개 필드 기능(#) 사용하기

- 접두사로 #을 붙여서 타입 체크와 런타임 모두에서 비공개로 만든다.
- 클래스 메서드나 동일한 클래스의 개발 인스턴스끼리는 접근이 가능하다.

```tsx
class PasswordChecker {
  #passwordHash: number;
  constructor(passwordHash: number) {
    this.#passwordHash = passwordHash;
  {

   checkPassword(password: string) {
     return hash(password) === this.#passwordHash;
   }
 }

 const checker = new PasswordChecker(hash('s3cret'));
 checker.checkPassword('secret');
 checler.checkPassword('s3cret');
```
