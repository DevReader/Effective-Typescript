## [ITEM 51] 의존성 분리를 위해 미러 타입 사용하기

- 만약 작성 중인 라이브러리가 의존하는 라이브러리의 구현과 무관하게 타입에만 의존한다면, 필요한 선언부만 추출하여 작성 중인 라이브러리에 넣는 것(미러링)을 고려해 보는 것이 좋다

```tsx
interface CsvBuffer {
  toString(encoding: string): string;
}
function parseCSV(contents: string | CsvBuffer): {[column: string]: string}[] }
// ..
}
```
