## useCallback

useMemo 에 이어서 useCallback 에 대해서 알아봅시다. <br>
이번에도 Dingdong Ditch(띵똥 벨튀) component를 뜯어서 개조를 해볼것입니다. <br>

전 포스팅에서도 말했지만 useMemo, useCallback 서로비슷합니다. <br>
useMemo 기반에서 추가된게 useCallback 이니까영 <br>

### Memoization 이란?

주어진 입력값에 대한 결과를 저장함으로써 같은 입력값에 대해 함수가 한 번만 실행되는 것을 보장을 의미합니다.

useMemo: **memoization** 된 **값을 반환** 합니다. <br>
useCallback: **memoization** 된 **함수를 반환** 합니다. <br>

useCallback의 간단한 예시를 들자면

1.  useCallback(() => fn, [**deps**]); // deps의 데이터가 변하게 된다면
2.  useCallback(**() => fn**, [deps]); // callback 함수를 반환 하게 됩니다.

```js
import React, { useState, useEffect } from "react";

const Bell202 = React.memo(({ call }) => {
  useEffect(() => {
    console.log("Rerender Bell202");
  });

  return <div>{`Bell 202!! ${call}`}</div>;
});

const Bell402 = React.memo(({ ditch, call }) => {
  useEffect(() => {
    console.log("Rerender Bell402");
  });

  return (
    <>
      <div>{`Bell 402!! ${call}`}</div>
      <button onClick={ditch}>Bell 402</button>
    </>
  );
});

const DingdongDitch = () => {
  const [call202, setCall202] = useState(0);
  const [call402, setCall402] = useState(0);

  const ditch = () => {
    setCall402(call402 + 1);
  };

  return (
    <div>
      <Bell202 call={call202} />
      <button onClick={() => setCall202(call202 + 1)}>Bell 202</button>
      <Bell402 call={call402} ditch={ditch} />
    </div>
  );
};

export default DingdongDitch;
```

![1-1](https://user-images.githubusercontent.com/96044518/161691628-b87aa5ac-a00e-4cfa-a838-162c779ca771.gif)
![1-2](https://user-images.githubusercontent.com/96044518/161691629-e7943d67-ed71-4a2a-8c12-cec9f487cc34.gif)

202호 벨을 눌렀는데 402호도 rerender 가 되는것을 볼 수 있습니다.
분명 **React.memo()** 를 적용했는데 왜 이런 현상이 일어나는걸까요? <br>

React.memo에 대해 모르신다면 해당 포스팅을 봐주세영! <br>
https://github.com/GangOn0215/dev-til/blob/main/React/ReactMemo.md <br>

call402 state가 값이 변하기 때문에 상위 컴포넌트에서 **Re-Rendering** 이 발생하고 **props에** 들어온 ditch 함수가 **계속 재선언**이 되기 때문 입니다.

```js
import React, { useState, useEffect, useCallback } from "react";

const DingdongDitch = () => {
  const [call202, setCall202] = useState(0);
  const [call402, setCall402] = useState(0);

  const ditch = useCallback(() => {
    setCall402(call402 + 1);
  }, [call402]);

  return (
    <div>
      <Bell202 call={call202} />
      <button onClick={() => setCall202(call202 + 1)}>Bell 202</button>
      <Bell402 call={call402} ditch={ditch} />
    </div>
  );
};

export default DingdongDitch;
```

![1-1](https://user-images.githubusercontent.com/96044518/161691628-b87aa5ac-a00e-4cfa-a838-162c779ca771.gif)
![1-3](https://user-images.githubusercontent.com/96044518/161691631-fa00ac85-3a36-44c6-8e29-91dcbaea0e41.gif)

![11513](https://user-images.githubusercontent.com/96044518/161692157-860d5772-33a2-4b4e-b3f1-c63204294884.jpg)

ditch 함수를 **useCallback으로 감싸서 함수를 넘겨줬더니** 202호 벨을 눌렀지만 402호 벨은 **Re-Rendering이 발생하지 않는걸** 볼수있습니다. <br>

간단하게 예제와 함께 설명을 해보았습니다. <br>
여기까지 봐주신 여러분 감사합니당.
