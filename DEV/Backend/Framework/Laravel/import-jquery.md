# PHP Laravel Import Jquery

#### 📝 2023-12-18
`Laravel` 에서 `vite`로 `jquery` 집어넣고 돌릴려다가 `$ is not define` 에러가 계속 나서 오만상 삽질하고 기록을 남겨봅니다

## 사전 준비
https://jquery.com/download/

위 링크를 통해 jquery를 받아줍니다.

```php
<script src="{{ asset('/assets/js/lib/jquery-3.7.1.js') }}"></script>
``` 

`/public/assets/js/lib` 에 `jquery.js` 를 추가 해준 뒤 `@vite()`로 js 들을 로드하기 전에 윗 부분에 위와 같이 script 링크를 해줍니다.


```php
<script src="{{ asset('/assets/js/lib/jquery-3.7.1.js') }}"></script>
<script src="{{ asset('/assets/js/lib/jquery-ui.js') }}"></script>
@vite(['resources/js/app.js', 'resources/js/layout.js'])
@vite(['resources/css/app.css', 'resources/css/common.css', 'resources/css/layout.css', 'resources/css/lib/jquery-ui.css'])
```

제 사이트는 다음과 같이 사용합니다.
정말 단순한건데.. 방법을 찾지 못하여 위와 같은 방법을 사용 했습니다.