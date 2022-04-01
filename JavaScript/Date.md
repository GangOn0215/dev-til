# Monthly Weekly Date
월간 주간 구하는 코드를 한번 짜보도록 합시다.

## JS 에서 Date 받아오기
```js
let today = new Date(); // Fri Apr 01 2022 22:39:17 GMT+0900 (한국 표준시)
```
`new Date()` 함수 호출을 통해 간단하게 날짜를 받아올수있습니다.

## Honey TIP ( 꿀팁 )
```js
today.getTime(); // 1648820357030
```
이러한 형식으로 `getTime();` 함수를 호출하게 되면 지정된 날짜의 시간에 해당하는 숫자 값(밀리 초)을 반환하게 됩니다.

```javascript
let anyDay = new Date(2022, 3, 1) // 2022.04.01 00:00:00
    anyDay = new Date(2022, 3, 1, 23, 59, 59) // 2022.04.01 23:59:59
```
`new Date(2022, 3, 1)` 이런식으로 날짜 `YYYY-MM-DD` 를 적어주면 `2022년 4월 1일 00시 00분 00초` 나오는걸 볼수 있습니다. <br \>
만약 비교를 하기위해 사용하고 싶다면 `new Date(2022, 3, 1, 23, 59, 59)` 이러한 형태로 사용하면 됩니다.

### Get Weekly FirstDay ~ LastDay (주간 첫날부터 마지막날 구하기)

```js
const todayDate = new Date();
let firstDay = null;
let lastDay = null;
let subDay = todayDate.getDay();     // (주간 시작일 일요일) weekly start - sunday
let subDay = todayDate.getDay() - 1; // (주간 시작일 월요일) weekly start - monday

// 만약 오늘이 일요일이라면 subday가 0이 되기때문에 subday를 6으로 맞춰줍니다.
if (todayDate.getDay() === 0) {
  subDay = 6;
}

firstDay = new Date(
  todayDate.getFullYear(),
  todayDate.getMonth(),
  todayDate.getDate() - subDay
)

lastDay = new Date(
  todayDate.getFullYear(),
  todayDate.getMonth(),
  todayDate.getDate() + (6 - subDay),
  23,
  59,
  59
);

console.log({firstDay, lastDay}); // {firstDay: 2022.03.27, lastDay: 2022.04.02}
```

### Get Monthly FirstDay ~ LastDay (월간 첫날부터 마지막날 구하기)
```js
const firstDay = new Date(
  todayDate.getFullYear(),
  todayDate.getMonth(),
  1
);

const lastDay = new Date(
  todayDate.getFullYear(),
  todayDate.getMonth() + 1,
  0,
  23,
  59,
  59
);

console.log({firstDay, lastDay}); // {firstDay: 2022.04.01, lastDay: 2022.04.30}
```

### get increaseWeekly (next Week)
```js 
const increaseWeekly = new Date(todayDate.getFullYear(), todayDate.getMonth(), todayDate.getDate() + 7);
```

### get decreaseWeekly (last Week)
```js 
const decreaseWeekly = new Date(todayDate.getFullYear(), todayDate.getMonth(), todayDate.getDate() - 7);
```

### get increaseMonthly (next Month)
```js
const increaseMonthly = new Date(curDate.getFullYear(), curDate.getMonth() + 1);
```

### get decreaseMonthly (last Month)
```js
const increaseMonthly = new Date(curDate.getFullYear(), curDate.getMonth() - 1);
```

### blog
https://coxemonkey.tistory.com/20
