## [ITEM 48] API 주석에 TSDoc 사용하기

- JSDoc/TsDoc 형태를 사용해서 함수, 클래스, 타입에 주석달기 → 편집기가 주석 정보를 표시해줌
- 주석에 타입 정보를 포함해선 안 됨

```tsx
/**
 * 인사말을 생성합니다.
 * @param name 인사할 사람의 이름
 * @param title 그 사람의 칭호
 * @returns 사람이 보기 좋은 형태의 인사말
 */
function greetFullTSDoc(naem: string, title: string) {
  return `Hello ${title} ${name}`;
}
```
