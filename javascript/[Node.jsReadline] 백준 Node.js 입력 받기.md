# [Node.js/Readline] 백준 Node.js 입력 받기.

## 개요

[Baekjoon]: https://www.acmicpc.net/

 에선 문제 입력을 받을 때, 직접 사용자가 입력을 수신할 수 있는 코드를 짜야 한다.

입출력을 제어하는 모듈로는 `fs`와 `readline`이 있다. 그 중 `readline`의 사용법을 알아보자.

## 예시

### 한 줄 입력 받기

```javascript
const readline = require('readline');

const r = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

r.on("line", (input) => {
    console.log(`Received: ${input}`);
    r.close();
});
```



### 여러 줄 입력 받기

```javascript
const readline = require('readline');

const r = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let input = [];

r.on("line", (line) => {
    input.push(line);
}).on("close", () => {
    const len = input.length;
    for (let i = 0; i < len; i++) {
        console.log(`Received: ${input}`);
    }

    console.log(`Have a good day!`);
    process.exit();
});
```



## 코드 설명
### readline 인터페이스 생성

- `readline` 모듈을 호출하고, `readline.createInterface`함수를 이용해 입출력을 제어하는  `readline.Interface`를 생성한다.

```javascript
const readline = require('readline');

const r = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});
```

### line 이벤트

- `line` 이벤트는 한 줄씩 입력을 받는다. `\n, \r, \r\n`으로 줄의 끝임을 알리는 신호를 수신한다. 일반적으로는 사용자가 `Enter` 또는 `Return`을 누를 때 발생한다. 

  `line` 이벤트는 입력이 들어올 때까지 무한정 기다리며 종료하려면 `clsoe` 이벤트를 반드시 호출해주어야 한다.

```javascript
r.on("line", (input) => {
    console.log(`Received: ${input}`);
    r.close();
});
```

- `r.close` 함수는 `close` 이벤트를 호출하며 입력을 종료한다.

### close 이벤트

- `close` 이벤트는 아래 조건 중 하나를 만족하면 호출된다.

  - `r.close()`
  - (발견하지 못함) ~~`end` 이벤트 수신~~
  - `Ctrl + D`
  - `Ctrl + C`

  `close` 이벤트가 발생하면 `readline.Interface`는 입출력에 대한 제어권을 상실하고 종료된다.

```javascript
.on("close", () => {
    console.log(`Have a good day!`);

    process.exit(0);
});
```



## 🤔의문

백준 사이트에서 여러 줄의 입력을 받을 때, `r.close`를 따로 호출하지 않는데 어떻게 `close`이벤트를 호출하는 걸까?

-> 백준 사이트에서는 입력 데이터가 파일 데이터다. 파일은 끝에 다르면 `EOF` 값을 반환하기 때문에 `r.close`를 하지 않아도 `EOF` 값을 인식하여 입력 후 자동으로 종료한다.



## 참조

- [readline 모듈](https://nodejs.org/api/readline.html#readline_event_close)