##useMemo

useMemo에 대해 공부를 하였지만 어떤 예제를 들어야 할지 애매모호 해서 상당히 골치 아팠습니다.
그러다 어제 올린 React.memo 에서 띵똥 벨튀 예제를 가져와서 쓰면 괜찮을것 같아서 DindongDitch를 개조해 보았습니다.

useMemo 를 공부하면 useCallback 은 필연으로 공부를 하게 됩니다.
두가지 비슷하지만 확연히 차이가 있습니다.

###Memoization 이란?

주어진 입력값에 대한 결과를 저장함으로써 같은 입력값에 대해 함수가 한 번만 실행되는 것을 보장을 의미합니다.

useMemo: **memoization** 된 **값을 반환** 합니다.
useCallback: **memoization** 된 **함수를 반환** 합니다.

useMemo의 간단한 예시를 들자면

1.  useMemo(() => fn, [**deps**]); // deps의 데이터가 변하게 된다면
2.  useMemo(**() => fn**, [deps]); // callback 함수가 실행 된 값을 반환 하게 됩니다.

**[ deps ]** 저 부분은 **dependency ( 의존 )** 이라는 의미이며 저 데이터가 변할때마다 **callback 함수**가 실행 된 **값을 반환** 받게 됩니다.

```js
import React, { useState, useEffect, useMemo } from "react";

const DingdongDitch = () => {
  const [call202, setCall202] = useState(0);
  const [call303, setCall303] = useState(0);
  const [call402, setCall402] = useState(0);

  const getTotalBell202303 = () => {
    console.log("define totalBell202303");

    return call202 + call303;
  };

  const totalBell202303 = getTotalBell202303();

  return (
    <div>
      <span>Bell202 + Bell303: {totalBell202303}</span> <br />
      <Bell202 call={call202} />
      <button onClick={() => setCall202(call202 + 1)}>Bell 202</button>
      <Bell303 call={call303} />
      <button onClick={() => setCall303(call303 + 1)}>Bell 303</button>
      <Bell402 call={call402} />
      <button onClick={() => setCall402(call402 + 1)}>Bell 402</button>
    </div>
  );
};

export default DingdongDitch;
```

![1-1](https://user-images.githubusercontent.com/96044518/161689900-ba5925aa-d3df-4e12-ae6e-565e8f7287a7.gif)
![1-2](https://user-images.githubusercontent.com/96044518/161689908-7c1109cc-d610-405d-bd9a-c26ba6f3718d.gif)

202호, 303호 벨 눌렀던 횟수를 합치는 함수 **getTotalBell202303()** 를 만들고 변수 **totalBell202303** 에다가 데이터를 받습니다. <br>

totalBell202303가 선언 될때 console로 뜨게 했습니다. <br>

분명 402호 벨을 눌렀는데 totalBell202303 변수가 계속 선언이 되고 있습니다. <br>

402호 state가 값이 변할때 **Component Re-Rendering**이 발생하는건데 그래서 **totalBell202303** 변수가 계속하여 **재선언**이 되고 있던것입니다. <br>

```js
import React, { useState, useEffect, useMemo } from "react";

const DingdongDitch = () => {
  const [call202, setCall202] = useState(0);
  const [call303, setCall303] = useState(0);
  const [call402, setCall402] = useState(0);

  const getTotalBell202303 = () => {
    console.log("define totalBell202303");

    return call202 + call303;
  };

  const totalBell202303 = useMemo(getTotalBell202303, [call202, call303]);

  return (
    <div>
      <span>Bell202 + Bell303: {totalBell202303}</span> <br />
      <Bell202 call={call202} />
      <button onClick={() => setCall202(call202 + 1)}>Bell 202</button>
      <Bell303 call={call303} />
      <button onClick={() => setCall303(call303 + 1)}>Bell 303</button>
      <Bell402 call={call402} />
      <button onClick={() => setCall402(call402 + 1)}>Bell 402</button>
    </div>
  );
};

export default DingdongDitch;
```

![1-1](https://user-images.githubusercontent.com/96044518/161689900-ba5925aa-d3df-4e12-ae6e-565e8f7287a7.gif)
![1-3](https://user-images.githubusercontent.com/96044518/161689914-bbd8725b-be19-4cb7-b051-fa4fd636ec19.gif)

`const totalBell202303 = useMemo(getTotalBell202303, [call202, call303]);`

**useMemo의 첫 번째 인자**는 **callback 함수**와 **두번째 인자**는 **dependency(의존) 변수**를 넣어줍니다. <br>

그렇게 의존성 부분의 데이터가 변할때만 값으로 할당이 되어 **totalBell202303** 변수가 component에 의해 **Re-Rendering**될 때 마다 **재선언** 하는 일이 없어집니다. <br>

이상 useMemo에 관한 이야기를 해보았습니다. <br>
여기까지 따라와주신 여러분 수고하셧습니다. <br>

비고) Bell202, Bell303, Bell402 컴포넌트는 코드가 너무 길어져 아래에 넣어두겠습니다.

```js
const Bell202 = React.memo(({ call }) => {
  useEffect(() => {
    console.log("Rerender Bell202");
  });

  return <div>{`Bell 202!! ${call}`}</div>;
});

const Bell303 = React.memo(({ call }) => {
  useEffect(() => {
    console.log("Rerender Bell303");
  });

  return <div>{`Bell 303!! ${call}`}</div>;
});

const Bell402 = React.memo(({ call }) => {
  useEffect(() => {
    console.log("Rerender Bell402");
  });

  return <div>{`Bell 402!! ${call}`}</div>;
});
```
