- 웹페이지의 특정 부분을 주기적으로 업데이트해줘야 하거나, 어떤 API로 부터 변경된 데이터를 주기적으로 받아와야 하는 경우가 있다.

- 따라서 이 경우 [[setTimeout()]] 대신 setInterval() 함수를 사용할 수 있다.
- [[setTimeout()]] 함수와 거의 비슷한데, 첫번째 인자로 실행할 코드를 담고 있는 함수를 받고, 두번째 인자로 반복 주기를 밀리초(ms) 단위로 받는다.

- setInterval() [[함수(Function)]]는 어떤 코드를 일정한 시간 간격을 두고 반복해서 실행하고 싶을 때 사용한다.


- 밑의 코드는 setInterval() 함수를 사용하여 콘솔에 현재 시간을 2초마다 출력하는 예시이다.

```js
setInterval(() => console.log(new Date()), 2000);
```

```bash
Sun Dec 12 2021 12:29:06 GMT-0500 (Eastern Standard Time)
Sun Dec 12 2021 12:29:08 GMT-0500 (Eastern Standard Time)
Sun Dec 12 2021 12:29:10 GMT-0500 (Eastern Standard Time)
Sun Dec 12 2021 12:29:12 GMT-0500 (Eastern Standard Time)
Sun Dec 12 2021 12:29:14 GMT-0500 (Eastern Standard Time)
Sun Dec 12 2021 12:29:16 GMT-0500 (Eastern Standard Time)
Sun Dec 12 2021 12:29:18 GMT-0500 (Eastern Standard Time)
```

- 밑의 코드는 0과 9 사이의 수를 무작위로 생성하여 2초마다 출력하는 예시이다.

```js
setInterval(() => console.log(Math.floor(Math.random() * 10)), 2000);
```

```bash
3
2
8
3
1
9
4
8
3
0
9
5
1
3
1
```

- 숫자를 변수에 저장해놓고 2초 마다 1씩 증가시키는 것도 가능하다.

```js
let count = 0;
setInterval(() => console.log(++count), 2000);
```

```bash
1
2
3
4
5
6
7
8
9
10
```

## clearInterval()

- setInterval() 함수는 인터벌 아이디(Interval ID)라고 불리는 숫자를 반환한다.
- 인터벌 아이디는 setInterval() 함수를 호출할 때 마다 내부적으로 생성되는 타이머 객체를 가리키고 있다.

- 이 값을 인자로 clearInterval() 함수를 호출하면 코드가 주기적으로 실행되는 것을 중단시킬 수 있다.

```js
const intervalId = setInterval(() => console.log(new Date()), 2000);
// >> Sun Dec 12 2021 12:45:31 GMT-0500 (Eastern Standard Time)
// >> Sun Dec 12 2021 12:45:33 GMT-0500 (Eastern Standard Time)
// >> Sun Dec 12 2021 12:45:35 GMT-0500 (Eastern Standard Time)

clearInterval(intervalId); // 이후로 중단
```