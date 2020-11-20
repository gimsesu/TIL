# Math.min(max) - Maximum call stack size exceeded

## 개요

Javascript에서 Math.min 혹은 Math.max를 이용하여 최솟값/최댓값을 구할 때, 

함수들이 받을 수 있는 인수의 개수에는 한계가 존재한다.



## 에러 - "최대 호출 스택 크기를 초과했습니다."

1,000,000 개의 수 중에서 최솟값을 찾는다고 했을 때, Math.min을 이용하면 다음과 같이 에러가 난다.

```javascript
const n = 1000000;
let nums = [];

for (let i=1; i <= n; i++) {
    nums.push(i);
}

let min = Math.min(...nums);

console.log(min)

// Uncaught RangeError: Maximum call stack size exceeded
// ... 
// let min = Math.min(...nums);
//                ^
// 
// RangeError: Maximum call stack size exceeded
// ...
//
```



### 재귀 호출

스택 메모리 부족 에러는 재귀함수에서 자주 볼 수 있다.

재귀함수는 자기 자신을 연속해서 호출하며 연산을 진행한다. 그 호출은 스택 메모리에 쌓이게 되며,

그 수가 많아지면 메모리가 부족해 위와 같은 에러를 일으킨다.



### Math.min 또한 재귀 호출의 형태

Math.min(max)이 연산을 수행하는 방식도 재귀 함수로 이루어져 있다는 것을 짐작할 수 있다. 최소 1,000,000번 이상 자기 자신을 연속해서 호출하다가 메모리가 부족해진 것이다.



## 대안 

Math.min(max)은 매우 간편한 방식이지만 한계가 있다는 것을 알았다. 위와 같은 예제는 for문으로 해결할 수 있다. 



```javascript
const n = 1000000;
let nums = [];

for (let i = 1; i <= n; i++) {
    nums.push(i);
}

let min = nums[1];
for (let i = 0; i < nums.length; i++) {
    min = min > nums[i] ? nums[i] : min
}

console.log(min)
// 1
```



## 참조

- [javascript - Maximum call stack size exceeded with Math.min and Math.max - Stack Overflow[Website]. (2020.11.15)](https://stackoverflow.com/questions/42623071/maximum-call-stack-size-exceeded-with-math-min-and-math-max/52613386#52613386)

