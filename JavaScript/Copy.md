# COPY

프로그래밍을 공부하다보면 shallow copy(얕은 복사) deep copy(깊은 복사)를 경험하신적이 있으실것입니다.

c언어 에서는 포인터, java에서는 reference variable 등등 을 관리하고 데이터 처리를 할때 많이 만나게 됩니다.

대표적인 예를 javascript 에서 확인해보겠습니다.

## Example) Shallow Copy
```js
// A 라는 사람이 메모장을 쓴다고 했을때 가정을 해봅시다.
const memoA = { author: 'Person A', content: 'plan to make a coffee' };

// 그리고 B 라는 사람이 A 라는 사람의 메모장을 복사하여 받은 뒤 수정한다고 해봅시다.
const memoB = memoA;

// B라는 사람은 author, content 의 내용만 지우고 B 사람의 이름과 메모를 적습니다.
memoB.author = 'Person B';
memoB.content = 'take a bus';

// memoA의 값 까지 바뀌는 현상이 발생 했습니다.
console.log('memoA, ', memoA); // memoA, {author: 'Person B', content: 'take a bus'}
console.log('memoB, ', memoB); // memoA, {author: 'Person B', content: 'take a bus'}

// memoA 와 memoB가 같다고 나옵니다.
console.log(memoA === memoB)   // true

// 이러한 현상을 reference copy (주소 복사), shallow copy(얕은 복사) 라고 하는데 왜 이런 문제가 발생할까요?

```

```js
const aVar = 10;
```
![aVar](https://user-images.githubusercontent.com/96044518/161382293-cca582c6-0f57-4ac7-886b-d229f0f43cb4.PNG) <br>
상수로 aVar 데이터를 만들고 10의 값을 넣는다면 다음과 같이 aVar 변수 안에 10이라는 데이터가 존재합니다.

하지만 reference 데이터들인 Object, Array 인 경우에는 다르게 동작을 합니다.


```js
const memoA = {author: 'Person A', content: 'Plan to make a coffee'};
```
![memoA](https://user-images.githubusercontent.com/96044518/161382406-71067f42-6c02-4eee-afc1-c0f94d7b7e46.PNG) <br>

바로 객체의 **주소**를 가진다는 점입니다.
**Java**로 예를 든다면 **참조 변수, 참조** 한다고 합니다.

```js
const memoB = memoA;
```
![memoB-1](https://user-images.githubusercontent.com/96044518/161382491-c1026940-1c86-460e-b744-3e4fb8820580.PNG) <br>
그래서 만약 memoB 변수를 만들고 memoA 변수를 바로 대입을 하여 복사를 한다면 **shallow copy (얕은 복사)** 가 되어버립니다.

```js
memoB.author  = 'Person B';
memoB.content = 'Take a bus';
```
![memoB-2](https://user-images.githubusercontent.com/96044518/161382530-a1e7abda-6bfd-4bb2-9119-da9d0213c24d.PNG) <br>
만약 이 상황에서 **memoB의 객체 프로퍼티의 값을 변경**한다면 어떻게 될까요? <br> <br>
![memoB-4](https://user-images.githubusercontent.com/96044518/161382565-771a9c34-b57a-4066-b5d1-9d9420d68ff5.PNG) <br>
memoB의 객체 프로퍼티 값들이 변경 되면서 memoA 또한 같은 주소를 가지고 있기 때문에 memoB만 변경을 했을뿐인데 **memoA 까지 데이터가 변하게 된 이유** 입니다.

이걸 방지하기 위해선 **Deep Copy (깊은 복사)** 를 해줘야 합니다.

## Example) Shallow Copy
```js
const memoA = { author: 'Person A', content: 'plan to make a coffee' };
const memoB = {...memoA};

memoB.author  = 'Person B';
memoB.content = 'take a bus';

console.log('memoA, ', memoA); // memoA, {author: 'Person A', content: 'plan to make a coffee'}
console.log('memoB, ', memoB); // memoB, {author: 'Person B', content: 'take a bus'}

console.log(memoA         === memoB)         // false
console.log(memoA.author  === memoB.author)  // false
console.log(memoA.content === memoB.content) // false
```
```js
const memoA = { 
  author: 'Person A', 
  content: ['one', 'two', 'three', 'four', 'five'] 
};

const memoB = {...memoA};

memoB.author  = 'Person B';
memoB.content[1] = 'a';

console.log('memoA, ', memoA); // memoA, {author: 'Person A', content: ['one', 'a', 'three', 'four', 'five']}
console.log('memoB, ', memoB); // memoB, {author: 'Person B', content: ['one', 'a', 'three', 'four', 'five']}

console.log(memoA         === memoB)         // false
console.log(memoA.author  === memoB.author)  // false
console.log(memoA.content === memoB.content) // true
```
내가 생각한 해결 방법 1. [ ES6 ] Spread Operator (스프레드 연산자) - **Deep Copy가 아닙니다.** <br>
ES6 에서 나온 Spread Operator 을 사용하여 Deep Copy(깊은 복사)를 구현 할 수 없습니다. <br>

Spread Operator 을 이용한다면 1차원 배열 or 객체만 데이터 복사가 됩니다. <br>
다차원 배열, 객체로 들어간다면 **Shallow Copy(얕은 복사)** 가 되어 버립니다. <br>

## Example) Deep Copy
### 해결 방법 1. JSON 메소드 이용하기

```js
const memoA = { 
  author: 'Person A', 
  content: ['one', 'two', 'three', 'four', 'five'] 
};

const memoB = JSON.parse(JSON.stringify(memoA));

memoB.author  = 'Person B';
memoB.content[1] = 'a';

console.log('memoA, ', memoA); // memoA, {author: 'Person A', content: ['one', 'two', 'three', 'four', 'five']}
console.log('memoB, ', memoB); // memoB, {author: 'Person B', content: ['one', 'a', 'three', 'four', 'five']}

console.log(memoA         === memoB)         // false
console.log(memoA.author  === memoB.author)  // false
console.log(memoA.content === memoB.content) // false
```

`const memoB = JSON.parse(JSON.stringify(memoA));`  이런식으로 **JSON 메서드를 이용하여 string**으로 바꾼다음 다시  <br>
**json.parse로** 바꿔서 **원시 데이터** 처럼 데이터를 복사 할 수 있습니다.

## Example) Deep Copy
### 해결 방법 2. Lodash
```js
import _ from 'lodash';

const memoA = { 
  author: 'Person A', 
  content: ['one', 'two', 'three', 'four', 'five'] 
};

const memoB = _.cloneDeep(memoA);

memoB.author = 'Person B';
memoB.content[1] = 'a';

console.log('memoA, ', memoA); // memoA, {author: 'Person A', content: ['one', 'two', 'three', 'four', 'five']}
console.log('memoB, ', memoB); // memoB, {author: 'Person B', content: ['one', 'a', 'three', 'four', 'five']})

console.log(memoA         === memoB)         // false
console.log(memoA.author  === memoB.author)  // false
console.log(memoA.content === memoB.content) // false
```
자바스크립트에서 유명한 라이브러리중 하나인 **lodash** 를 이용하여 **Deep Copy**를 지원하는 메소드가 있습니다. <br>
`cloneDeep();` 이라는 메소드 입니다. <br>
lodash는 _ 이라는 걸로 불러와서 많이들 쓰는것 같아서 _로 불러왔고 `_.cloneDeep()` 하여 인자에 복사할 데이터를 넣게 되면 Deep copy 를 할수있습니다.
