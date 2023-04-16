## 0. 프로젝트 S.A
내가 리액트로 진행해볼 프로젝트는, 감정 다이어리 프로젝트로 사용자의 감정을 글로 기록하고 수정 및 삭제 할 수 있으며 오늘의 감정 점수를 매기는 웹 다이어리 프로젝트이다.

프로젝트의 핵심인 다이어리를 생성할 수 있는 Diary Editor 부분에 대한 간단한 S.A는 아래와 같다.

![](https://velog.velcdn.com/images/doh_0112/post/11333d03-e798-43d4-a897-afd1de3e1c6b/image.png)

기능적인 부분은 작성자 입력과 일기 작성, 감정 점수 매기기와 저장으로 이루어져 있다. 

## 1. 사용자 입력처리
### 1-1. input과 textarea 사용자 입력 처리
사용자 값을 받아야 하는 부분은 작성자 부분과 일기 작성 부분이다.
각각 `input`과 `Textarea` 태그로 만들었고 useState()를 이용해서 사용자가 입력한 값을 각 태그의 value(태그의 속성) 값으로 지정해줄 것이다. 나중에 value를 활용할 수 있게!

``` jsx
// DiaryEditor.js
const DiaryEditor = () => {
  const [author, setAuthor] = useState("");
  const [content, setContent] = useState("");
  return (
    <div className="DiaryEditor">
      <h2>Diary</h2>
      <div>
        <input name="author" value={author} onChange={(e) => setAuthor(e.target.value)}></input>
      </div>
      <div>
        <textarea name="content" value={content} onChange={(e) => setContent(e.target.value)}></textarea>
      </div>
    </div>
  );
};
```
처음에 이 부분을 onChange에서 바로 콜백함수를 작성하지 않고 따로 빼서 작성했었는데, 매개변수 중복 때문에 계속 에러가 나와서 한참 헤매고 수정했다. [에러 게시글 참고](https://velog.io/@doh_0112/react%EC%97%90%EC%84%9C-setState-%EC%82%AC%EC%9A%A9%EC%8B%9C-%EC%97%90%EB%9F%AC)

아무튼, 지금 위 코드가 똑같이 초기값이 빈 문자열이고 둘 다 똑같은 로직을 수행하고 있으니 얘네를 합쳐주는게 낫다. 따라서 아래와 같은 코드로 수정해준다.
```jsx


const DiaryEditor = () => {
  const [state, setState] = useState({
    author: "",
    content: "",
  });
  return (
    <div className="DiaryEditor">
      <h2>Diary</h2>
      <div>
        <input name="author" value={state.author} onChange={(e) => setState({ ...state, author: e.target.value })}></input>
      </div>
      <div>
        <textarea name="content" value={state.content} onChange={(e) => setState({ ...state, content: e.target.value })}></textarea>
      </div>
    </div>
  );
};
```

차례대로 짚어보면

1. 초기값이 문자열이고 똑같은 로직을 수행하는 modifier 함수들이므로, 하나의 useState()로 생성한다. 이때, author와 content는 객체 안에 담아서 초기값을 설정해준다.
 
2. onChange가 일어났을 때, author의 값을 변경해줘야 하는 경우에서,초기값을 객체에 담아줬으므로 setState() modifier 함수에도 객체로 값을 전달해야한다. 객체에 정보가 많이 담겨 있을 때 그것을 일일이 나열할 수 없으므로 spread 연산자를 사용해 객체를 펼쳐주고, update 될 author만 다시 명시해준다.

3. 같은 로직으로 content의 값 또한 setState() modifier 함수에 객체로 전달하고, update될 content 내용을 명시해준다.

4. 이때 주의할 점은 `{ ...state, content: e.target.value }` 이 순서가 바뀐다면, content를 명시해줬지만 뒤에 오는 ...state로 인해서 content가 기존의 state에 있던 값으로 덮어쓰기 됨

### 1-2. select 사용자 입력 처리

그 다음 사용자 값을 받아야하는 부분은 감정 점수 부분으로, select와 option 태그로 만들었고 useState()를 통해 사용자가 선택한 값으로 value(태그의 속성) 값을 바꿔줘야한다.

이 또한 위의 input, textarea 로직과 동일하기 때문에 또 다른 state를 만들기보다 객체에 감정 점수의 값을 하나 넣어주는 것이 낳다.
따라서 아래와 같이 만들었다.
```js

const DiaryEditor = () => {
  const [state, setState] = useState({
    author: "",
    content: "",
    emotion: 1,
  });
  const handleChangeState = (e) => {
    setState({ ...state, [e.target.name]: e.target.value });
  };
  return (
    <div className="DiaryEditor">
      <h2>Diary</h2>
      <div>
        <input name="author" value={state.author} onChange={handleChangeState}></input>
      </div>
      <div>
        <textarea name="content" value={state.content} onChange={handleChangeState}></textarea>
      </div>
      <div>
        <select name="emotion" value={state.emotion} onChange={handleChangeState}>
          <option>1</option>
          <option>2</option>
          <option>3</option>
          <option>4</option>
          <option>5</option>
        </select>
      </div>
    </div>
  );
};
```

1. state의 초기값에 emotion을 추가하고 초기값을 1로 설정했다(점수는 1점부터 5점까지 있으므로). 
2. input과 textarea 태그 안에 바로 적어줬던 콜백함수를 변수에 할당해줬다. select의 state 또한 이 함수로 컨트롤 할 수 있기때문에 재사용성 있게 하나의 함수로 처리하게 한다.
3. `handleChangeState` 함수는 이벤트 객체를 받아서 setState() 함수로 현재의 state를 변경할 수 있다. 만약 input에서 onchange가 발생한 경우  `{author : e.target.value}`를 setState() 함수로 넘겨 현재 author의 state를 e.target.value로 바꿔주는 로직이다. 
또 다른 예시로, textarea에서 onchange가 발생한 경우 `{content: e.target.value}`를 setState() 함수로 넘겨 현재 content의 state를 e.target.value로 바꿔주는 로직이다.

4. 따라서 아래와 같은 코드로 작성하면, e.target.name으로 접근하여 key값으로 사용하고, e.target.value를 바뀔 state 값으로 사용하게 된다.
```jsx
setState({ ...state, [e.target.name]: e.target.value })
``` 
객체 리터럴 사용시 대괄호 사용하는 부분은 [이 포스팅 참고](https://velog.io/@doh_0112/%EA%B0%9D%EC%B2%B4-%EB%A6%AC%ED%84%B0%EB%9F%B4-%EC%82%AC%EC%9A%A9%EC%8B%9C-%EC%9D%B4%EA%B1%B4-%EC%99%9C)

## 2. 다이어리 저장

다이어리 작성 후 저장 하기 위한 버튼을 만들어야한다.
```jsx

const DiaryEditor = () => {
  const [state, setState] = useState({
    author: "",
    content: "",
    emotion: 1,
  });
  ...중략
  const handleSubmit = () => {
    console.log(state); // 현재 사용자 값 확인
    alert("저장 완료!");
  };
  return (
    <div className="DiaryEditor">
      <h2>Diary</h2>
      ...중략
      <div>
        <button onClick={handleSubmit}>Save!</button>
      </div>
    </div>
  );
};
```
버튼에 onClick 시 handleSumbit 핸들러 함수를 연결해서, 지금까지의 state(=객체 형태의 사용자 입력 값)을 콘솔에 보여주고 alert로 저장 완료! 메세지를 띄우게 했다. 일단은 간단한 형태만 만들어주고 추후에 다시 저장 로직을 연결해야 한다.


