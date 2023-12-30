# Callback

`created at. 2023-12-20`

## Callback은 무엇인가?

간단하게 말하면 함수(function)은 JS에서 객체 이기때문에 함수 인자(arguments)로 함수를 넣어주는것을 의미

```js
    // 예시

    /**
     * @params {callback} say
     */
    function sayString(say) {
        const welcome = "welcome";

        say(welcome);
    }

    function say(str) {
        console.log(str);
    }

```

위와 같이 say callback 함수를 만들어두고 sayString을 호출 해서 arguents(인수)로 함수를 넣어주면 say 함수가 호출되어 callback으로 문자열을 출력 합니다.

## 왜Callback을 써야하는가?
Callback에는 아주중요한 개념이 몇가지가 있습니다.
1. 콜백은 태스크가 끝나기 전에 함수가 실행 되지 않는 것을 보장합니다
2. 콜백은 비동기 JS 코드를 작성 할 수 있도록 해줍니다

## Callback은 어디에 쓰이는가?
위에서 언급했다시피 비동기 함수를 사용할때 쓰게됩니다.
대표적인 예를 보여드리겠습니다.

#### 1. setTimeOut
```js
    // 1. setTimeOut
    setTimeOut(() => {
        console.log('Hello World!');
    }, 1000)

```

js에서 자주 사용되는 setTimeOut 이라는 js의 내장 함수 입니다.
setTimeOut의 첫번째 arguments(인자)는 callback, 실행하게 될 함수를 적고 두번째 argument는 시간을 적습니다.


#### 2. addEventListener
```js
    // Vanila JavaScript
    document.queryselector("#click-me").addEventListener("click", function() {
        console.log('clickd !');
    });

    // jQuery
    $('#click-me').on('click', function() {
        console.log('clickd !');
    });
```
js 에서 이벤트 등록을 할려면 위와같이 addEventListener 함수 를 사용하며 매개변수는 `addEventListener(type, listener)` 이기 때문에 두번재 인수로 callback 함수를 넣어두고 이벤트가 발생하면 두번째 인수인 callback을 실행시키겠다는 의미입니다.