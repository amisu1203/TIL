## 0. 리액트를 사용하는 이유?

**𝟏. 리액트는 컴포넌트 기반의 UI 라이브러리**
자주 사용되는 코드들을 기능이나 목적에 따라 컴포넌트 별로 나누어서 적재적소에 해당 컴포넌트를 사용하는 것이다.
만약 중복된 코드를 컴포넌트화 시키지 않고 총 5군데에서 사용하고 있다가, 수정이 필요할 경우에는 5군데 모두 해당 코드를 수정해줘야 한다. 하지만 컴포넌트화 시키면 해당 컴포넌트만 수정하면 끝이기 때문에 유지보수에도 용이하고 코드도 간결해진다.

**𝟐. 리액트는 선언형 프로그래밍**
절차를 하나하나 다 나열하는 명령형 프로그래밍과 달리 리액트는 선언형 프로그래밍이다. 대표적인 명령형 프로그래밍인 제이쿼리로 예를 들면,
제어할 요소를 가져옴("$#id" 이런 형태로) -> 가져온 요소의 값을 숫자형으로 변환 후 변수에 할당 -> plus를 누르면 변수에 할당된 값에 +1, minus를 누르면 -1 이런식으로 동작하게 한다.
하지만 리액트는 선언형 프로그래밍으로 저 과정을 단순히 plus를 누르면 현재 값에 +1하고, Minus를 누르면 -1한다로 정리할 수 있다.

그러니까 절차를 전부 다 정리해야 하는 명령형과 달리 목적만 명시하는 것이 선언형 프로그래밍이고 이 방법으로 설계하는 것이 리액트이다.

**𝟑. 리액트는 virtual DOM을 사용한다.**
리액트는 UI에 변화가 생기는 부분을 바로 렌더링 하지 않고 먼저 가상 DOM에 추가한 다음 그것을 실제 DOM에 반영한다. 따라서 실제 DOM에 계속해서 렌더링하는 것보다 훨씬 효율적으로 렌더링할 수 있게 된다.

<br>

## 1. React App 만들기

리액트는 Node.js 기반의 자바스크립트 라이브러리이다.
대다수의 리액트 사용자들이 웹팩(여러개 js 파일 하나로 합쳐주는 모듈 번들 라이브러리)과 바벨(JSX 문법 사용하게 해주는 라이브러리)와 함께 리액트를 사용을 하는데, 이 외에도 정말 많은 패키지가 존재한다.
따라서 이미 리액트를 사용하기 위해 환경설정까지 다 해놓은, 한 마디로 '세팅 완료된 패키지'를 이용해서 리액트를 사용할거다.
그것을 Boiler Plate라고 하는데 react의 경우 **create-react-app**이 해당한다.

> npx create-react-app

이라고 터미널에 명령하면 된다.
이 부분은 [리액트 공식문서](https://ko.reactjs.org/docs/create-a-new-react-app.html#create-react-app) 참고하샘
‘npx’는 패키지를 로컬에 글로벌로 설치하지 않고 바로 일회성으로 실행할 수 있게 해주는 도구 ) 그 다음

> npm start

라고 명령하면 리액트 앱이 실행된다(localhost 3000으로). 이 명령어는 package.json의 scripts에 적혀있는데, 내가 임의로 수정하거나 추가해도 됨

실행해서 보면

```
reactexam1/
  node_modules/
  public/
    index.html
    favicon.ico
  src/
    App.css
    App.js
    App.test.js
    index.css
    index.js
    logo.svg
  package-lock.json
  package.json
  README.md
```

이런 구조로 되어있다.
**public/index.html과 src/index.js는 엔트리 포인트가 되는 소스로, 파일이름이 변경되면 create-react-app은 작동하지 않으므로 주의**

<br>

## 2. JSX

### (1). 컴포넌트 사용시 return 값을 주자

컴포넌트는 한 마디로 정리하면 JSX를 사용해서 무엇인가를 return하는 함수라고 정의할 수 있겠음
따라서 컴포넌트 사용시 반드시 return 값을 주자

```js
// MyFooter.js
const MyFooter = () => {
  return <footer>My Footer</footer>;
};

export default MyFooter;
```

### (2). 닫힘 규칙

JSX 문법을 사용해서 js 파일에서 html 태그를 사용할 때는 반드시 닫힘 태그를 명시해야 한다.

```jsx
// App.js
function App() {
  return (
    <div className="App">
      <MyHeader />
      <header className="App-header">hi! REACT!</header>
      <MyFooter />
    </div>
  );
}
```

div와 header 태그 모두 닫힘 규칙을 따르고 있다. a나 image 태그 역시 `<a />` 이런식으로 작성하면 된다. 얘네를 self closing tag라고 한다.

### (3). 최상위 태그 규칙

JSX 표현식은 반드시 **하나의** 부모 요소를 가져야한다.

```jsx
//App.js
function App() {
  return (
      <MyHeader />
      <header className="App-header">
        <h2>hi~</h2>
      </header>
      <MyFooter />
  );
}
```

만약 위와 같이 코드를 작성하고, npm start 명령어를 입력했을 때 리액트 앱(localhost:3000)은 아래와 같이 컴파일에 실패했음을 알려준다.
![](https://velog.velcdn.com/images/doh_0112/post/39ae1576-bc9c-40ae-a601-8058b1962711/image.png)

![](https://velog.velcdn.com/images/doh_0112/post/fdd85872-b021-458d-87df-0f13a1d1eca5/image.png)또한 해당 컴포넌트에 빨간 밑줄이 생기고 호버하면 위와 같은 메시지를 확인할 수 잇음. **JSX 표현식에는 부모 요소가 하나 있어야 합니다!**

혹 최상위 태그를 주고 싶지 않다면 `<React.Fragment>` 태그로 감싸서 만들면 된다. 이것은 리액트의 기능이기 때문에 사용 전에

> import React from 'react';

리액트를 임포트해오고 최상위 태그를 주고 싶지 않은 부분을 아래와 같이 수정하면 된다.

```jsx
function App() {
  return (
    <React.Fragment>
      <MyHeader />
      <header className="App-header">
        <h2>hi~</h2>
      </header>
      <MyFooter />
    </React.Fragment>
  );
}
```

이 경우에도 최상위 태그를 준 것과 똑같이 정상적으로 작동한다.
또한, React.Fragment를 쓰지 않고 `<>~</>`로 감싸도 똑같이 작동한다.

### (4). css 스타일링

먼저 css 파일을 통해 스타일링 하는 방법

```css
/* App.css */
.App {
  background-color: black;
}

h2 {
  color: #ffffff;
}
```

css 파일을 만들어서 사용할 js 파일에 import 하면 끝

```js
// App.js 상단에 import 문구 추가
import "./App.css";
```

### (5). inline 스타일링

```js
// App.js
import MyHeader from "./MyHeader";
import MyFooter from "./MyFooter";
import React from "react";

function App() {
  const style = {
    App: {
      backgroundColor: "orange",
    },
    h2: {
      color: "white",
    },
  };

  return (
    <div className="App" style={style.App}>
      <MyHeader />
      <header className="App-header">
        <h2 style={style.h2}>hi~</h2>
      </header>
      <MyFooter />
    </div>
  );
}

export default App;
```

이렇게 style이라는 객체를 만들어서 사용하려는 태그에 style = {style.App} 이런식으로 점 표기법(.)으로 밸류에 접근해서 작성하는 방식이 있다.

### (6). { } 안에 문자나 숫자만

중괄호 안에 js의 직접적인 값이나 표현식 혹은 함수(일급객체니까 얘도 값이지만 알아듣기 편하게 함수라고)를 문자나 숫자를 통해서만 작성할 수 있다.

```js
// App.js
function App() {
  const sayHi = () => "hi!";
  return (
    <div className="App">
      <MyHeader />
      <header className="App-header">
        <h2>{sayHi()}</h2>
      </header>
      <MyFooter />
    </div>
  );
}
```

이렇게 함수를 만들고 그걸 `{sayHi()}`로 작성하면 리턴값인 hi!가 h2에 들어가게 된다.

```js
function App() {
  let num = 2;
  return (
    <div className="App">
      <MyHeader />
      <header className="App-header">
        <h2>
          {num}은 {num % 2 ? "홀수" : "짝수"}이다.
        </h2>
      </header>
      <MyFooter />
    </div>
  );
}
```

표현식은 값으로 평가받을 수 있는 문이기 때문에 삼항연산자(표현식)도 저렇게 {}안에 넣을 수 있는거임 따라서 렌더링 해보면 h2에 2는 짝수이다라고 잘 나옴.

<br>

## 3. STATE

state는 상태.
배고픈 상태 --밥 먹기---> 배부른 상태 --소화---> 적당한 상태 --시간--->배고픈 상태가 계속해서 반복되는 것처럼
state는 계속해서 변화하는 특정 상태다.

react의 useState()를 사용하면 현재 상태와 그 상태를 변화시킬수 있는 modifier 함수를 배열로 받게 되는데 그걸 구조분해할당을 통해 할당 받는 문장이 아래와 같다. useState에 넣는 값은 초기값으로 현재 state는 0이 된다.

```js
let [state, setState] = useState(0);
```

이ㄸㅐ 버튼을 클릭했을 때 state를 1 증가하고 싶은거라면
const onIncrease = (state) => {
setState(state => state + 1);
}
이렇게 setState 함수에 현재 state (현재 0)를 넘겨서 현재의 상태를 변화시키고 그것을 렌더링하겠다는 것이다.
onIncrease 함수는 + 버튼을 눌렀을 때만 작동하게 하고 싶으니, button에 onClick={onIncrease}를 넣어주면 된다.

```js
//Counter.js
import React, { useState } from "react";

const Counter = () => {
  let [counter, setCounter] = useState(0);
  const onIncrease = (counter) => {
    setCounter((counter) => counter + 1);
  };
  const onDecrease = (counter) => {
    setCounter((counter) => counter - 1);
  };
  return (
    <div>
      <h1>{counter}</h1>
      <button onClick={onIncrease}>+</button>
      <button onClick={onDecrease}>-</button>
    </div>
  );
};

export default Counter;
```

리액트 공식문서의 예시 : React DOM은 내용이 변경된 텍스트 노드만 업데이트했습니다. --> [변경된 노드만 리렌더링 하는거 보기](https://ko.reactjs.org/c158617ed7cc0eac8f58330e49e48224/granular-dom-updates.gif)

<br>

## 4. Props

컴포넌트에 데이터를 전달하는 방법 = prop!

부모컴포넌트에서 자식 컴포넌트로 데이터를 전달하는 방법!
객체형태로 전달하기 때문에 {...data}이렇게 스프레드로 주고 자식 컴포에서 사용할 때는 사용할 값만 {name}이렇게 받아오면 (비구조화 할당)

### 4-1. spread 연산자로 값 전달

```js
// App.js
function App() {
  let num = 2;
  let counterProps = {
    a: 1,
    b: 2,
    c: 3,
  };
  return (
    <div>
      <MyHeader />
      <Counter {...counterProps} />
    </div>
  );
}
```

이렇게 counterProps라는 변수는 객체니까 그걸 전달해줄 때는 값을 바로 사용할 수 있게 `...` 스프레드 연산자로 전달함

### 4-2. 비 구조화 할당으로 값 사용

```js
// counter.js
const Counter = ({ a, b }) => {
  console.log(a); // 1
  console.log(b); // 2
  let [counter, setCounter] = useState(0);
  const onIncrease = (counter) => {
    setCounter((counter) => counter + 1);
  };
  const onDecrease = (counter) => {
    setCounter((counter) => counter - 1);
  };
  return (
    <div>
      <h1>{counter}</h1>
      <button onClick={onIncrease}>+</button>
      <button onClick={onDecrease}>-</button>
    </div>
  );
};
```

마찬가지로 자식 컴포넌트인 Counter에서 값을 받을 때는 그냥 prop 형태로 받아도 되지만 지금 그 prop = {a: 1, b: 2, c: 3} 이거임. 여기서 바로 a 값 사용하기 위해 비 구조화 할당 ㄱㄱ
그래서 ({a, b}) 형태로 그 값만 바로 받아오겠다는거 ㅇㅇ

### 4-3. defaultProps 설정

```jsx
// Counter.js
const Counter = ({ initialValue }) => {
  let [counter, setCounter] = useState(initialValue);
  중략...
  return (
    <div>
      <h1>{counter}</h1>
      <button onClick={onIncrease}>+</button>
      <button onClick={onDecrease}>-</button>
    </div>
  );
};

Counter.defaultProps = {
  initialValue: 0,
};
```

만약에 부모에서 데이터를 까먹고 안 보내주면 undefined를 받기 때문에 에러가 난다. 의도치않게 특정 props가 undefined가 되지 않게 해주기 위해 **defaultProps를 자식 컴포넌트 파일에서 설정**해주면 부모컴포에서 값을 주지 않아도 디폴트값으로 사용하므로 에러가 나지 않는다.

### 4-4. prop으로 state까지 전달?!

단순히 정적인 값들만 Prop으로 넘길 수 있는게 아님
심지어 계속해서 변화하는 state까지 넘길 수 있음 바로 예시 ㄱ

```jsx

// Counter.js
const Counter = ({ initialValue }) => {
  let [counter, setCounter] = useState(initialValue);
  ... 중략
  return (
    <div>
      <h1>{counter}</h1>
      <OddEvenResult counter={counter} />
      <button onClick={onIncrease}>+</button>
      <button onClick={onDecrease}>-</button>
    </div>
  );
};

// OddEvenResult.js
const OddEvenResult = ({ counter }) => {
  console.log(counter);
  return (
    <div>
      {counter}는 {counter % 2 ? "홀수" : "짝수"}
    </div>
  );
};
```

즉, Counter 컴포넌트에서 생성된 State인 counter를 `OddEvenResult` 컴포넌트로 넘겨줌. 이때 이 전 넘겨준 애들은 객체 그 잡채라서 바로 {변수명} 이렇게 해줬지만, 보시다 싶이 Counter는 그냥 0부터 시작하며 변화하는 정수라서 넘겨줄때 `counter={counter}` 이렇게 객체 형태로 만든 값을 넘겨줌

### 4-5. prop으로 element 전달 🙆🏻

![](https://velog.velcdn.com/images/doh_0112/post/da0a184b-80b1-48a1-8853-bab2c6caf29f/image.png)

```jsx
function App() {
  let num = 2;
  let counterProps = {
    initialValue: 0,
  };
  return (
    <div>
      <MyHeader />
      <Counter {...counterProps} />
    </div>
  );
}
```

여기서 리턴 값에 이 div 전체를 감싸주는 Container 컴포넌트를 만들고, 그 컴포넌트에 인라인 스타일링을 주려고한다.

```jsx
// App.js
function App() {
  let num = 2;
  let counterProps = {
    initialValue: 0,
  };
  return (
    <Container>
      <div>
        <MyHeader />
        <Counter {...counterProps} />
      </div>
    </Container>
  );
}

//Container.js
import React from "react";

const Container = ({ children }) => {
  const style = {
    border: "1px solid gray",
    margin: 20,
    padding: 20,
    width: 200,
  };
  return <div style={style}>{children}</div>;
};

export default Container;
```

![](https://velog.velcdn.com/images/doh_0112/post/d5a44300-4f77-4c91-a2e9-82683886e9f6/image.png)

App.js에서 App 컴포넌트의 리턴값에 Container 컴포넌트로 감싸줌. 현재 Container 컴포넌트 안에 다른 React element들이 포함되어 있는데 얘를 Container에서 prop으로 받을 수 있음.
따라서 리턴값에 받아온 프롭인 {children}을 넣어주면 그 안에 있던 element들이 렌더링 된다.

<br>
<hr>
<br>

## 5. 렌더링 되는 부분 정리

상태가 변화하면 그것을 컨트롤하는 컴포넌트가 리렌더링 되고
혹은 부모에서 자식으로 prop을 넘겨주고, 부모의 상태가 변경되면 리렌더링 되고
둘 다 아니더라도 내 부모 컨포넌트가 리렌더링 되면 자식 컴포넌트도 리렌더링 되는거임

## 참고

boiler plate : [Top 9 React Boilerplates to Know in 2023](https://www.atatus.com/blog/react-boilerplates/)
create-react-app : [CRA 설명 및 소스코드 분석](https://seongmun-hong.github.io/react/create-react-app)
