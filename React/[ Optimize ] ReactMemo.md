## React.Memo
React 최적화를 하기 위해 useMemo, **React.memo**, useCallback 세가지를 보통 이용 합니다 (제가 아직 배운게 이것밖에 없어서) <br>

그중에서 **React.memo**에 대해 공부하며 예제를 간단히 만들어보며 연구를 했는데 괜찮은 예제가 떠올라서 이렇게 공부한걸 올리는 겸 예제를 공유 해봅니다.

React.memo를 배우기전에 Memoized에 대해서 알아야하는데 <br>
##Memoized란?
주어진 입력값에 대한 결과를 저장함으로써 **같은 입력 값에 대해 함수가 한 번만 실행되는 것을 보장**하는것을 의미합니다. ( 알고리즘 방식이기도 합니다 )

**React.memo**는 **Component의 결과값을 Memoized**를 합니다.

![i13862545432](https://user-images.githubusercontent.com/96044518/161547747-79efa2db-16f0-40a6-a16b-b672d4104e82.gif)

한국에서 어릴때 유행했던 **벨튀(DingDong Ditch)** 를 바탕으로 **Component 화** 시켜서 만들어본 예제 입니다. 

```js
import React, { useState, useEffect } from "react";

const Bell202 = ({ call }) => {
  useEffect(() => {
    console.log('Rerender Bell202');
  });

  return <div>{`Bell 202!! ${call}`}</div>
};

const Bell303 = ({ call }) => {
  useEffect(() => {
    console.log('Rerender Bell303');
  });

  return <div>{`Bell 303!! ${call}`}</div>
};

const DingdongDitch = () => {
  const [call202, setCall202] = useState(0);
  const [call303, setCall303] = useState(0);

  return (
    <div>
      <Bell202 call={call202}/>
      <button onClick={() => setCall202(call202 + 1)}>Bell 202</button>

      <Bell303 call={call303}/>
      <button onClick={() => setCall303(call303 + 1)}>Bell 303</button>
    </div>
  )
}

export default DingdongDitch;
```
![a](https://user-images.githubusercontent.com/96044518/161551553-c344b663-29da-459c-8477-335b6266eef9.jpg)
![1-1](https://user-images.githubusercontent.com/96044518/161553248-b0ce949c-945c-4b8d-af48-2e5b98cca696.gif)
![1-2](https://user-images.githubusercontent.com/96044518/161553278-577608db-dbe7-4d4d-9235-46fec1ac0aae.gif)

아파트가 하나 있고 **202호 벨**을 누르고 튀었습니다. <br>
그런데 **303호 벨**도 같이 울립니다. <br>
 
**Ball202 state 값**이 변했기 때문에 **Component Re-Rendering 현상이 발생한 것**인데 <br>

상위 컴포넌트인 DingdongDitch **state 값이 변하여 Re-Rendering**이 되면서 <br>
하위 컴포넌트인 **Bell303 Component**도 **Re-Rendering**이 되어버립니다. <br>

 

**Component가 Re-Rendering이 되는 기준**은 <br>
1. 새로운 **props**가 들어오거나 (업데이트) <br>
2. 부모 컴포넌트가 **Re-Rendering**이 되었을때 <br>
 

부모 컴포넌트가 **Re-Rendering**이 되면서 데이터가 바뀌며 **202호 벨(props)** 를 눌렀지만 **303호 벨** 까지 눌러지는 상황이 발생이 된겁니다. 또한 303호 벨을 누르자 202호 벨까지 울리게 되는거죠. <br>

 

**rerender 기준**의 **2번째인 부모 컴포넌트**가 **re-rendering**이 되었기 때문인데 <br>
이런경우 **React.memo**를 이용하여 **Component**를 감싸면 의미없는 **Re-Rendering** 이 해결이 됩니다. <br>


```js
import React, { useState, useEffect } from "react";

const Bell202 = React.memo(({ call }) => {
  useEffect(() => {
    console.log('Rerender Bell202');
  });

  return <div>{`Bell 202!! ${call}`}</div>
});

const Bell303 = React.memo(({ call }) => {
  useEffect(() => {
    console.log('Rerender Bell303');
  });

  return <div>{`Bell 303!! ${call}`}</div>
});

const DingdongDitch = () => {
  const [call202, setCall202] = useState(0);
  const [call303, setCall303] = useState(0);

  return (
    <div>
      <Bell202 call={call202}/>
      <button onClick={() => setCall202(call202 + 1)}>Bell 202</button>

      <Bell303 call={call303}/>
      <button onClick={() => setCall303(call303 + 1)}>Bell 303</button>
    </div>
  )
}

export default DingdongDitch;
```
![a](https://user-images.githubusercontent.com/96044518/161551489-f903c3e0-8b00-4899-9120-b1ae35418f12.jpg)
![1-1](https://user-images.githubusercontent.com/96044518/161554933-beb5c3a6-558c-4b75-b133-69ccaa3b744c.gif)
![2-1](https://user-images.githubusercontent.com/96044518/161555032-a6bf9628-71e5-453a-8664-dfb6b892a9be.gif)

**React.memo**를 이용하여 **Component**를 감싸면 **상위 Component**가 **Re-Rendering**이 되었다 해도 **props**가 동일하다면 바꾸지 않습니다. <br>

이상 띵똥 벨튀를 이용한 예시였습니다. <br>
감사합니다.