```js
//this exceeded the time limit
function maxSlidingWindow(nums: number[], k: number): number[] {
    const maxnums = []

    for (let i = 0; i < nums.length - k + 1; i++) {
        let max = nums[i]
        for (let j = 1; j < k; j++) if(nums[i+j]>max) max = nums[i+j]
        maxnums.push(max);
    }
    return maxnums
};
```

```js
function maxSlidingWindow(nums: number[], k: number): number[] {
  const result: number[] = [];
  const deque: number[] = [];

for (let i = 0; i < nums.length; i++) {
  // 1) Kick out old
  if (deque[0] === i - k) deque.shift();

  // 2) Remove weaker numbers
  while (deque.length && nums[i] >= nums[deque[deque.length - 1]]) {
    deque.pop();
  }

  // 3) Add new index
  deque.push(i);

  // 4) Save max if window is formed
  if (i >= k - 1) result.push(nums[deque[0]]);
}
  return result;
}

```