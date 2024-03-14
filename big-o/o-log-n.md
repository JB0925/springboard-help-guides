First, I'll provide a small script I wrote as a demo. There are two functions; `binarySearch()` and `linearSearch()`.

`linearSearch()` just uses a for loop to iterate over an array of 100,000,000 elements until it finds the target.

`binarySearch()` uses a different approach, which I will explain more in a moment, and it runs in O(log n) time. It also takes the same 100,000,000 item array as input, and returns when it finds the target.
```
const binarySearch = (arr, target) => {
  let left = 0;
  let right = arr.length - 1;
  let count = 0;
  while (left <= right) {
    count += 1;
    let mid = Math.floor((left + right) / 2);
    if (arr[mid] == target) {
      console.log(`binarySearch took ${count} iterations`);
      return mid;
    } else if (arr[mid] < target) {
      left = mid + 1;
    } else {
      right = mid - 1;
    }
  }
  console.log(`binarySearch took ${count} iterations`);
  return -1;
};
const arr = Array.from({ length: 100_000_000 }, (_, i) => i + 1);
const target = 94_654_342;
console.time("binarySearch");
console.log(binarySearch(arr, target));
console.timeEnd("binarySearch");
```
```
const linearSearch = (arr, target) => {
  let count = 0;
  for (let i = 0; i < arr.length; i++) {
    count += 1;
    if (arr[i] === target) {
      console.log(`linearSearch took ${count} iterations`);
      return i;
    }
  }
  console.log(`linearSearch took ${count} iterations`);
  return -1;
};
console.time("linearSearch");
console.log(linearSearch(arr, target));
console.timeEnd("linearSearch");
```
Here are the results:
```
11:04:22 jessebrink@Jesses-MacBook-Pro testing → node binary_search.js 
binarySearch took 27 iterations
94654341
binarySearch: 6.595ms
linearSearch took 94654342 iterations
94654341
linearSearch: 282.983ms
```

As you can see, the binarySearch took many less iterations, and found the target much faster.

One of my first thoughts is that if n is small, it mostly does not matter what algorithm, data structure, etc., you use. In fact, some non-optimized sorting algorithms ( like selection sort or bubble sort ) actually perform better than the optimized ones like quicksort, merge sort, etc., when n is small. This is why I used an array of 100,000,000 items.

Ok, to the point! A logarithm is the inverse of an exponent. With computers, we are mostly concerned with base 2. If you aren't familiar with that, I'm happy to explain how that counting works; just let me know.

So we know that 2^3 == 8. Log base 2 of 8 == 3.

2^10 == 1024, and log base 2 of 1024 == 10.

In other words, if we do something in O(log n) time, it will take us approximately 10 steps if n is 1024, whereas a regular for loop ( a linear search ) could take up to 1024 steps.

Log base 2 of 1,000,000 is roughly 20. If we can, we would rather use 20 iterations to do something rather than possibly 1,000,000.

So let's look at the binary search above and talk about what is happening:
```
const binarySearch = (arr, target) => {
  let left = 0;
  let right = arr.length - 1;
  while (left <= right) {
    let mid = Math.floor((left + right) / 2);
    if (arr[mid] == target) {
      return mid;
    } else if (arr[mid] < target) {
      left = mid + 1;
    } else {
      right = mid - 1;
    }
  };
  
  return -1;
};
```

In this algorithm, we:
- Create two indices, left and right, initially set to the start (0) and end (arr.length -1) of the array.
- We run a loop until left surpasses right.
- In that loop, we create another index, called mid. Mid is the middle of left and right.
- We then check to see if arr[mid] is equal to the target. If it is, we've found our target and can return.
- If not, we check to see if arr[mid] is less than the target. If it is, we make left equal to mid + 1
- Otherwise, we make right equal to mid - 1 because, we know that arr[mid] is greater than the target.
- Finally, if we get to the last line, we never found the target, so we return -1.

The magic happens in steps 5 and 6. Each time you change left or right, you are cutting the searchable array space in half. Note that I said searchable. We aren;t actually changing the array. We just don't need to worry about the other half any more.

To put it in a different light, we can look at it like this:
```
const arr = [1,2,3,4,5,6,7,8,9,10];
const target = 9;

left = 0;
right = 9;
mid = Math.floor((left + right) / 2);     // 4
arr[mid] === 5;
// We know now, that nothing before index 5 matters in our search, so we can set left to be 5.

left = 5;
right = 9;
mid = Math.floor((left + right) / 2);     // 7
searchable array space is now [6,7,8,9,10];

arr[mid] === 8;
  // This is still less than our target, so the searchable space is now [9,10]. We don't need to worry about anything before that, because we know it is less than our target.

left = 8
right = 9
mid = Math.floor((left + right) / 2);     // 8
arr[mid] === 9
  // We found our target; we are finished.
```

So you can see it took three steps here to find the target, because we divided the searchable space in half each time. If we did the same search with a regular for loop, It would've taken 9.

Hopefully all of that is helpful and makes some sense. A couple parting thoughts:
- When n is small, it mostly does not matter which algorithm you use.
- Binary searches can only be done on sorted input.
- Any time you see an algorithm where something is being divided in half each time, that is a tell that it is potentially O(log n). It could also be O(n log n), depending on what is happening. But algorithms that divide the space in half are often log n. Some examples are things you can do on binary search trees like search and insert, and other operations on things like heaps and priority queues.
- There are always approximations. Just because log base 2 of n is 10 doesn’t mean it will take exactly 10 steps. Could be 9, or 11, etc.

