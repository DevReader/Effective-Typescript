## [ITEM 44] 타입 커버리지를 추적하여 타입 안전성 유지하기

### noImplicitAny를 설정해도 any 타입이 존재하는 경우

- 명시적 any 타입
- 서드파티 타입 선언

→ 프로젝트에서 any의 개수를 추적하는 것이 좋음

### type-coverage 패키지 활용해서 추적하기

```bash
npx type-coverage

npx type-coverage --detail
```
