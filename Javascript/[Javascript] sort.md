# sort() 정렬함수 사용하기

## Array.prototype.sort()

자바스크립트에는 배열 객체를 정렬할 수 있는 `sort()` 함수가 내장돼 있다.

`sort()`는 기본적으로 배열을 오름차순 정렬한다.

```javascript
let strings = ['하나', '둘', '십일', '셋', '넷'];
strings.sort();
console.log(strings);
// [ '넷', '둘', '셋', '십일', '하나' ]

let numbers = [1, 2, 11, 3, 4];
numbers.sort();
console.log(numbers);
// [ 1, 11, 2, 3, 4 ] 어라?
```



## 🤔뭔가 이상한데?

자바스크립트에서 `sort()`는 배열 요소를 문자열로 변환하고 유니코드 코드 포인트 값에 따라 정렬한다.

위의 예시에서 숫자 정렬은 우리가 기대한 `[1, 2, 3, 4, 11]`의 순서로 정렬되지 않았다. 자바스크립트에서 숫자를 정렬하려면 `sort()`에 별도의 정렬 기준을 작성해주어야 한다.



## compareFunction

자바스크립트의 `sort()`는 `arr.sort([compareFunction])`으로 이루어져 있다. `compareFunction`은 정렬 순서를 정의하는 함수다.  별도의 `compareFuntion`이 제공되지 않으면 문자열 기준으로 정렬된다. 



### 숫자 정렬

그렇다면 숫자를 정렬하고 싶다면 어떻게 해야 할까?

```javascript
numbers.sort((a, b) => { 
    return a - b // 오름차순 정렬
});
console.log(numbers);
// [ 1, 2, 3, 4, 11 ]
```

위 예시에서 `(a, b)`는  `compaerFunction(a, b)`로 배열 요소 `a`와 `b`를 비교한다. 숫자를 비교하기 위해 `a`에서 `b`를 뺀 결과값을 통해 오름차순 정렬할 수 있다.



### 객체 정렬

다양한 키값으로 이루어진 객체의 경우, 키별로 조건을 주어 정렬할 수 있다.

```javascript
let object = [
    { sound: '짹짹', decibel: 31 },
    { sound: '으르렁', decibel: 11 },
    { sound: '멍멍', decibel: 28 },
    { sound: '피카피카', decibel: 97 },
    { sound: '꺄악', decibel: 64 }
];

/* sound 기준으로 정렬 (오름차순) */
object.sort((a, b) => {
    if (a.sound > b.sound) {
        return 1;
    }
    if (a.sound < b.sound) {
        return -1;
    }
    // a 와 b가 같을 때
    return 0;
});
console.log(object); 
// 꺄악, 멍멍, 으르렁, 짹짹, 피카피카

/* decibel 기준으로 정렬 (오름차순) */
object.sort((a, b) => {
    return a.decibel - b.decibel
})
console.log(object); 
// 11, 28, 31, 64, 97
```



위의 `sound`기준 정렬식은 삼항연산자로 간단하게 표현할 수도 있다.

```javascript
object.sort((a, b) => {
    return a.sound < b.sound ? -1 : a.sound > b.sound ? 1 : 0;
});
```



## 😀`map`을 이용한 정렬

`compareFunction`은 배열 내 요소에 따라 여러 번 호출될 수 있다. 이런 구조는 높은 오버헤드를 유발할 수 있다. `compareFunction`이 복잡해지고 정렬할 요소가 많아질 경우, `map`을 이용한 정렬 방법이 있다. 

이 방법은 임시 배열을 하나 만들어서 여기에 실제 정렬에 사용할 값만을 뽑아서 이를 정렬하고, 그 결과를 이용해서 실제 정렬을 한다.



```javascript
let list = ['Hello', 'Annyeong', 'Bonjour', 'Nihao'];

// 임시 배열은 위치 및 정렬 값이 있는 객체를 갖는다.
let mapped = list.map((el, i) => {
    return { index: i, value: el.toLowerCase() };
});

// 매핑된 배열의 정렬
mapped.sort((a, b) => {
    if ((a.value > b.value)) {
        return 1;
    } 
    if (a.value < b.value) {
        return -1;
    }
    if (a.value === b.value) {
        return 0;
    }
});

// 결과를 담을 배열
let result = mapped.map((el) => {
    return list[el.index];
});

console.log(result);
// [ 'Annyeong', 'Bonjour', 'Hello', 'Nihao' ]
```



위의 정렬 함수는 아래와 같이 작성할 수도 있다.

```javascript
mapped.sort((a, b) => {
    return +(a.value > b.value) || +(a.value === b.value) - 1;
});
```



## 📜참고

- [Array.prototype.sort() - JavaScript | MDN[Website]. (2020.11.20)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)