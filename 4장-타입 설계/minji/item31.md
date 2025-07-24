## 타입 주변에 null 값 배치하기

- 변수들이 있을 때 각 변수의 null 여부가 서로에게 영향이 있는 경우, 각 변수별로 null 여부를 관리하는 대신 모두 null 또는 모두 null 아님으로 구분해야 값을 다루기 쉬움

  ```typescript
  // 개별적으로 관리했을 때
  function extent1(nums: number[]) {
    let min, max;

    for (const num of nums) {
      if (!min) {
        // 자바스크립트에서는 0이 falsy 값이므로 최솟값, 최댓값이 0인 경우 값이 덧씌워질 수 있음
        min = num;
        max = num;
      } else {
        min = Math.min(min, num);
        max = Math.max(max, num);
        // ~~~ 'number | undefined' 타입을 number 타입에 할당할 수 없습니다.
      }
    }
    return [min, max]; // nums 가 비어있는 경우 [undefined, undefined] return 함
  }

  // 한 번에 관리할 때
  function extent2(nums: number[]) {
    let result: [number, number] | null = null;

    for (const num of nums) {
      if (!result) {
        result = [num, num];
      } else {
        result = [Math.min(num, result[0]), Math.max(num, result[1])];
      }
    }
    return result;
  }
  ```

- 한 값의 null 여부가 다른 값의 null 여부에 암시적으로 관련되도록 설계하면 안됨
- 클래스를 만들 때는 필요한 모든 값이 준비되었을 때 생성하여 null이 존재하지 않도록 하는 것이 좋음

  ```typescript
  class UserPosts {
    user: UserInfo; // UserInfo | null 로 쓰지 않기
    posts: Posts[]; // Posts[] | null 로 쓰지 않기

    constructor(user: UserInfo, posts: Posts[]) {
      this.user = user;
      this.post = posts;
    }

    static async init(userId: string): Promise<UserPosts> {
      const [user, posts] = await Promise.all([
        fetchUser(userId),
        fetchPostsForUser(userId),
      ]);
      return new UserPosts(user, posts);
    }
  }
  ```

- strictNullChecks 설정하기
