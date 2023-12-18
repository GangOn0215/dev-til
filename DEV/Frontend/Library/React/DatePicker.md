## Date

서두 - react에서 date를 처리하는 것은 까다로운 작업일수도 있습니다. <br>
format을 처리해야 되기 때문입니다.

```html
<input type="date" value="2020-04-09" />
```

react에서 input을 생성하고 type을 date를 줍니다.
date input value의 date format은 `YYYY-MM-DD`입니다.

```js
const [date, setDate] = useState(new Date());

const getStringDate = (date) => {
  let year = date.getFullYear();
  let month = date.getMonth() + 1;
  let day = date.getDate();

  if (month < 10) {
    month = `0${month}`;
  }
  if (day < 10) {
    day = `0${day}`;
  }

  return `${year}-${month}-${day}`;
};

<input
  type="date"
  value={getStringDate(date)}
  onChange={(e) => {
    setDate(new Date(e.target.value));
  }}
/>;
```

![1-1](https://user-images.githubusercontent.com/79497314/162560326-239ba305-7d68-4209-a240-161bbcf194f2.gif)

date를 받았을 때 `YYYY-MM-DD` 형식으로 리턴해주는 함수를 만들어줬습니다. ( 한입 크기로 잘라먹는 리액트(React.js) : 기초부터 실전까지 ) 참고하였습니다. <br>

그리고 input value 에다가 date state를 삽입하고 onChange 에다가 setDate를 할 수 있는 콜백 함수를 넣어줍니다.

정상적으로 잘 작동을 하는 것을 볼 수 있습니다.

```js
useEffect(() => {
  console.log(date);
});
```

useEffect를 이용하여 date를 찍어보면 date 바뀔 때 즉 setDate 함수가 호출이 될 때 리 랜더링이 되며 console에 찍히는 걸 볼 수 있습니다. <br>

기본으로 제공해주는 input date를 사용해도 가볍고 좋긴 한데 커스텀이 불편하다는 단점이 있습니다. <br>

## Date-Picker

이걸 해결하기 위해 저는 `DatePicker Library`를 사용했습니다.

```bash
npm install react-datepicker --save
```

커맨드를 열고 npm으로 install 해줍니다.

```js
import React, { useState } from "react";
import DatePicker from "react-datepicker";

function App() {
  const [startDate, setStartDate] = useState(new Date());

  <DatePicker
    selected={startDate}
    onChange={(date) => {
      setDate(date);
    }}
    className="date-picker"
  />;
}
```

![1-2](https://user-images.githubusercontent.com/79497314/162560328-f8e285b9-ff64-4053-9d99-c74f6fc1b68b.gif)

DatePicker component를 불러와주고 DataPicker에서는 value가 아니라 selected 속성을 넣어줍니다. <br>

그리고 input date와는 다르게 따로 별로의 함수를 만들지 않아도 date를 자유롭게 사용할 수 있습니다. <br>

그런데 이걸 이용하면 커스텀이 용이하다고 했는데 한번 css로 수정을 해보겠습니다. <br>

```css
.date-picker {
  border: 1px solid #b0a8b9;
  border-radius: 5px;
  text-align: center;
  width: 90px;
}

.date-picker:focus-visible {
  outline: none;
}
```

![1-3](https://user-images.githubusercontent.com/79497314/162560329-d96826f2-15f0-4976-a0ae-c3731a8ceffc.gif)

아주 예쁘게 만들어진 모습을 볼 수 있습니다. <br>

이상 `data-picker` 에 대해 공부한것을 포스팅 해보았습니다. <br>
감사합니다
