## [ITEM 31] 타입 주변에 null 값 배치하기

- 값이 전부 null이거나 전부 null이 아닌 경우로 분명히 구분된다면, 값이 섞여 있을 때보다 다루기 쉽다
- before
  - 최솟값이나 최댓값이 0인 경우, 값이 덧씌워져 버리는 문제
  - nums 배열이 비어있는 경우, [undefined, undefined] 를 반환함

```tsx
function extent(nums: number[]) {
  let min, max;
  for (const num of nums) {
    if (!min) {
      min = num;
      max = num;
    } else {
      min = Math.min(min, num);
      max = Math.max(max, num);
    }
  }
  return [min, max];
}
```

- after
  - min, max를 한 객체 안에 넣고 null이거나 null이 아니게 하기

```tsx
function extent(nums: number[) {
	 let result: [number, number] | null = null;
	 for (const num of nums) {
	   if (!result) {
	     result = [num, num] ;
	   } else {
	     result = [Math.min(num, result[0]), Math.max(num, result[1]);
     }
   }
   return result;
 }
```

- 클래스의 경우, 클래스를 만들 때 필요한 모든 값이 준비되었을 때 생성하여 null이 존재하지 않도록 하는 것이 좋다
- strictNullChecks를 설정하면 null과 관련된 문제를 찾을 수 있기 때문에 반드시 필요하다
