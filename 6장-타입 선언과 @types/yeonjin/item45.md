## [ITEM 45] devDependencies에 typescript와 @types 추가하기

### 의존성 관리

- dependencies: 프로젝트를 실행하는 데 필수적인 라이브러리
- devDependencies: 프로젝트를 개발하고 테스트하는 데 사용되지만, 런타임에는 필요없는 라이브러리
  - 타입 정보는 런타임에 존재하지 않기 때문에 typescript 관련 라이브러리는 devDependencies에 속함
- peerDependencies: 런타임에 필요하긴 하지만, 의존성을 직접 관리하지 않는 라이브러리

### 고려해야할 2가지 의존성

1. 타입스크립트 자체 의존성: 타입스크립트를 시스템 레벨로 설치하기보다는 devDependencies에 넣는 것이 좋다. 모든 팀원이 정확한 버전의 타입스크립트를 설치할 수 있음.
2. 타입 의존성: 원본 라이브러리 자체가 dependencies에 있더라도 @types 의존성은 devDependencies에 있어야함.
