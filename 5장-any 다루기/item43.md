## [ITEM 43] 몽키 패치보다는 안전한 타입을 사용하기

- 자바스크립트는 객체와 클래스에 임의의 속성을 추가할 수 있을 만큼 유연하다
- 타입 체커는 Document, HTMLElement의 내장 속성에 대해서는 알고있지만, 임의로 추가된 속성은 알지 못한다 → document, DOM으로부터 데이터를 분리하는 것이 좋다

### 분리하지 못하는 경우의 차선책

1. interface의 보강을 사용한다
   1. 장점
      - any를 사용할 때보다 타입이 더 안전하다
      - 속성에 주석을 붙일 수 있다
      - 속성에 자동완성을 사용할 수 있다
      - 몽키 패치가 어떤 부분에 적용되었는지 기록이 남는다
   2. 주의할점
      - 전역적으로 적용되기 때문에 코드의 다른 부분이나 라이브러리로부터 분리할 수 없음
      - 실행되는 동안 속성을 할당하면 실행 시점에서 보강을 적용할 수 없다

```java
inteface Document {
    monkey: string;
}
document.monkey = 'Tamarin'; //정상
```

1. 구체적인 타입 단언문을 사용한다

```java
interface MonkeyDocument extends Document {
    monkey: string;
}
(document as MonkeyDocument).monkey = 'Macaque';
```
