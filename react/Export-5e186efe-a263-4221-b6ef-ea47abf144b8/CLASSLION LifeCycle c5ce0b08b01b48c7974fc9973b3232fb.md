# CLASSLION/LifeCycle

# 3. LifeCycle

시작하기에 앞서 리액트의 큰 구조를 다시 보자.

## 1) lifeCycle의 구조

### (1) Class Component의 구조

: Class Component는 구조는 아래와 같다.

: 클래스 정의 → lifeCycle함수 → 기능함수 → render함수 순으로 구성되어 있다.

![CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-10__4.23.26.png](CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-10__4.23.26.png)

### (2) lifeCycle이란?

: 컴포넌트의 생성과 종료까지를 라이프사이클이라고 한다.

라이프사이클은 왜 중요한가요? 

→ 컴포넌트는 계속 변하기 때문에, props가 변했을 때에 대한 대응을 해야하므로!

![CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-10__4.28.31.png](CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-10__4.28.31.png)

: 컴포넌트가 처음 생성이 되는    → rendering    → `componentDidMount`이다.

: 새로운 props, state가 있으면 → rerendering → `componentDidUpdate`를 한다.

: 작업을 끝낸 컴포넌트가 페이지에서 안 보일 때 → `componentWillUnmount`한다.

### (3) liftCycle 코드예시

: 라이프사이클의 전체적인 순서는 아래와 같다.

![CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-10__5.29.08.png](CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-10__5.29.08.png)

: 현재 우리는 전체를 감싸는 App컴포넌트와 worldClock컴포넌트가 있다. 

 우리는 라이프사이클이 언제 시작되며 언제 끝나는지 알기위해, 기존의 App.js에서 추가적인 코드를 적었다.

> A. componentDidMount

a). constructor 부분 (parent)

: 클래스를 만들 때 제일 먼저 초기화하고 구성하는 부분이다.

: App이라는 컴포넌트 안에 worldClock컴포넌트가 있는 구조이다.

```jsx
|class App extends React.Component {
|	 constructor(props){
|		 super(props)
|		 this.cityTimeData = [
|		 	 ['서울', 10],
|			 ['베이징', 9],
|			 ['시그니', 12],
|			 ['LA', 17],
|			 ['부산', 10]
|		 ]
|		 this.state = {
|			 content : ''
|		 }
|		 console.log("parent state")  //
|	 }
```

b). componentDidMount 부분 (parent)

: 컴포넌트에서 라이프사이클을 도는 순서를 보기위해 컴포넌트마운트 함수를 작성한다.

```jsx
|	 componentDidMount(){
|		 console.log('parent 마운트되었습니다.') //
|	 }
|
|	 handlingChange =(event)=> {
|		 this.setState({content : event.target.value})
|	 }
```

c). render 부분 (parent)

: render부분, 한 컴포넌트는 생성되고 랜더링 과정을 거친다.

```jsx
|	 render(){
|		 console.log('parent rendering') //
|		 return(
|			 <div className='App'>
|				 <div className={'myStyle'}>hi</div>
|				 <div className={'post'}>
|					 first post
|					 <textarea value={this.state.content} onChange={this.handlingChange}></textarea>
|				 </div>
|				 {this.cityTimeData.map((citytime, index)=>
|					 <WorldClock city={citytime[0]} time={citytime[1]} key={index} />
|				 ))
|			 </div>
|		 };
|	 }
|}
```

: 결과적으로 시작(constructor) → 랜더링(render) → 마운트완료 순으로 진행된다.

---

(d). constructor 부분 (child)

: worldClock컴포넌트에서 테스트를 해보자.

```jsx
|class WorldClock extends React.Component {
|  constructor(props){
|    super(props)
|		 this.state = {
|			 hours : this.props.time,
|			 minute : 0,
|			 stop : false,	
|		 }
|		 this.timer = setInterval(()=>{
|			 this.setState((state)=>(
|        state.minute === 59 
|        ?{hour : state.hour + 1, minute : 0}
|        ?{minute : state.minute + 1}
|			 ))
|	  }, 100)
|   console.log('child start') //
|  }
```

(e). componentDidMount 부분 (child)

: 컴포넌트에서 라이프사이클을 도는 순서를 보기위해 컴포넌트마운트 함수를 작성한다.

```jsx
|	 componentDidMount(){
|		 console.log('child 마운트되었습니다.') //
|	 }
```

: 결과적으로 app컴포넌트 시작→랜더링 → worldclock시작→마운트 → app마운트 순으로 진행된다. 

![CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-10__6.06.15.png](CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-10__6.06.15.png)

 

(f). 결론

: 이 말은 곧, 상위컴포넌트는 랜더링까지 진행이 되지만, 하위 컴포넌트의 마운팅이 끝나야만 상위컴포넌트의 마운팅이 진행된다.

![CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-10__6.08.29.png](CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-10__6.08.29.png)

---

> B. componentDidUpdate

a). constructor 부분 (parent)

: 클래스를 만들 때 제일 먼저 초기화하고 구성하는 부분이다.

: state와 props가 변할 때 사용된다. 

: 현재 우리는 아래 코드에서 시계가 계속 변하고 있다.

```jsx
|class WorldClock extends React.Component {
|  constructor(props){
|    super(props)
|		 this.state = {
|			 hours : this.props.time,
|			 minute : 0,
|			 stop : false,	
|		 }
|		 this.timer = setInterval(()=>{
|			 this.setState((state)=>(
|        state.minute === 59 
|        ?{hour : state.hour + 1, minute : 0}
|        ?{minute : state.minute + 1}
|			 ))
|	  }, 100)
|   console.log('child start') 
|  }
|  componentDidMount(){
|   console.log('child mount')
|  }
```

b). componentDidUpdate 부분 

: 클래스를 만들 때 제일 먼저 초기화하고 구성하는 부분이다.

: state와 props가 변할 때 사용된다. 

: 현재 우리는 아래 코드에서 시계가 계속 변하고 있다.

```jsx
  componentDidUpdate(){
    console.log('update')
  }
```

: 위와 같이 코드를 작성하고 서버를 구동시켜보면, 아래와 같이 업데이트가 될 때마다 (시간이 변할 때마다) child 업데이트가 그 횟수만큼 계속 log된다.

![CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__7.46.06.png](CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__7.46.06.png)

c). 변화를 일으키는 setInterval을 componentDidMount로 옮겨보자

```jsx
|class WorldClock extends React.Component {
|  constructor(props){
|    super(props)
|		 this.state = {
|			 hours : this.props.time,
|			 minute : 0,
|			 stop : false,	
|		 }
|   console.log('child start') 
|  }
|  componentDidMount(){
|		 this.timer = setInterval(()=>{    //이부분
|			 this.setState((state)=>(
|        state.minute === 59 
|        ?{hour : state.hour + 1, minute : 0}
|        ?{minute : state.minute + 1}
|			 ))
|	  }, 100)
|   console.log('child mount')
|  }
```

: 그러면 아래와 같이, 

부모시작 

→ 부모 랜더링 

→ 자식 시작 → 자식 랜더링 

→ 자식 마운트 → 부모 마운트 

→ 자식 리랜더링(업데이트)

![CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__7.49.11.png](CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__7.49.11.png)

그런데 만약에 위와 같이 자식이 아닌 부모가 업데이트가 되면 어떤 순서로 진행될까?? 

: 이번에는 DidUpdate를 부모 컴포넌트 쪽으로 옮겨서 확인해보자.

: parent컴포넌트인 app에서 변동되는 값을 input에 들어가는 값이다.

```jsx
|class App extends React.Component {
|	 constructor(props){
|		 super(props)
|		 this.cityTimeData = [
|		 	 ['서울', 10],
|			 ['베이징', 9],
|			 ['시그니', 12],
|			 ['LA', 17],
|			 ['부산', 10]
|		 ]
|		 this.state = {
|			 content : ''
|		 }
|		 console.log("parent state")  //
|	 }
|  componentDidMount(){
|    console.log('parent mount')
|  }
|  componentDidUpdate(){
|    console.log('parent update')
|  }
|  handlingChage=(event)=>{
|    this.setState({content: event.target.value})
|  }
```

: 그리고 나서 input칸에 값을 넣으면, 그 순간마다 업데이트가 되는 걸 볼 수 있다.

![CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__7.54.58.png](CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__7.54.58.png)

![CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__7.57.28.png](CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__7.57.28.png)

: 결론적으로 아래 그림과 같이, 한 자 한 자 입력때마다 부모업데이트 → 자식 리랜더링&업데이트 순이 되는 걸 알 수 있다. 

그런데 생각해보면, 부모를 업데이트를 하는데 자식을 항상 리렌더링 하는 건 낭비가 아닐까요?

![CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__8.01.04.png](CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__8.01.04.png)

: 그래서 리액트에서는 자식컴포넌트를, 부모컴포넌트의 업데이트가 다 끝난 후 한번만 하는 기능이 있다. 

: 자식컴포넌트인 WorldClock에서, `ClassWorldClock extends React.Component` → `ClassWorldClock extends React.PureComponent`로 바꾸면 한 자 한 자를 적을 때마다 업데이트되던 자식이 모든 글씨를 쓰고나서 한번만 업데이트 한다.

![CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__7.54.58.png](CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__7.54.58.png)

한자한자 적을 때마다 자식이 업데이트(낭비적)

![CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__8.02.47.png](CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__8.02.47.png)

input에 모든 글씨를 적은 후에, 자식이 한번만 업데이트(비낭비적)

그러면 ShouldComponentUpdate와 PureComponent의 차이는 뭔가요?

: ShouldComponentUpdate는 그때 그때마다 원하는 업데이트 구간을 코딩할 수 있으며, PureComponent는 리액트에서 미리 제공되는 기능이다.

: 웬~~~만하면 PureComponent를 쓰는 게 더 편하다.(일일히 코딩할거면 상관 없지만.)

---

> C. UnMount

: 컴포넌트를 없앨 때 쓰는 것이므로, 일단 버튼을 생성해보자.

a). 부모 컴포넌트의 return부분에서 버튼 하나를 생성한다.

: 손꾸락 튕기기라는 버튼을 클릭했을 때 → 어떠한 이벤트가 발생하게 한다. 

`(이벤트에 맞는 state생성 → 버튼을 핸들링할 함수 선언→ 버튼에 핸드링 함수 추가)`

- 이벤트에 맞는 state생성(처음에는 보여지므로 true)

```jsx
|class App extends React.Component {
|	 constructor(props){
|		 super(props)
|		 this.cityTimeData = [
|		 	 ['서울', 10],
|			 ['베이징', 9],
|			 ['시그니', 12],
|			 ['LA', 17],
|			 ['부산', 10]
|		 ]
|		 this.state = {
|			 content : ''
|      show : true,   //
|		 }
```

- 버튼 이벤트를 핸들링할 함수를 선언한다.

```jsx
|		 console.log("parent state")  //
|	 }
|	 componentDidMount(){
|		 console.log('parent 마운트되었습니다.') //
|	 }
|
|	 handlingChange =(event)=> {
|		 this.setState({content : event.target.value})
|	 }
|
//(2)버튼 이벤트를 핸들링할 함수를 선언한다. 
|  handlingClock=(event)=>{
|    this.setState({show: false})
|  }
```

- 여기에 버튼을 생성한다.

    : 여기서 자바스크립트 코드를 이용해서 버튼 클릭시 사라지게 해보자

    : 일단 자바스크립트에서 true&&true→true, true&&false→false임을 기억하자.

    : `this.state.show && (~~) => {  }`, 이 부분에서 show와 뒤의 (~~)가 둘 다 true면 `⇒{  }` 가 보여지며, show가 false이면 `⇒{  }`가 안 보여지게 된다.   

```jsx
|	 render(){
|		 console.log('parent rendering') 
|		 return(
|			 <div className='App'>
|				 <div className={'myStyle'}>hi</div>
|				 <div className={'post'}>
|					 first post
|					 <textarea value={this.state.content} onChange={this.handlingChange}></textarea>
|				 </div>
|      //
|        <button onClick=this.handlingClock> 손꾸락 튕겨버리기 </button>
|
|        {this.state.show&&   //
|				 this.cityTimeData.map((citytime, index)=>
|					 <WorldClock city={citytime[0]} time={citytime[1]} key={index} />
|				 ))
|			 </div>
|		 };
|	 }
|}
```

: 그럼 아래 사진과 같이, 버튼을 클릭시 아래 항목들이 사라지는 걸 볼 수 있다.

![CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__8.29.11.png](CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__8.29.11.png)

아 그런데, 저는 버튼을 1번 눌렀을 때 안나오고 2번째로 눌렀을 떄 아래 항목이 보여지게 하고 싶어요..ㅜㅠ

: 그러면 버튼 이벤트 함수를 아래와 같이 고쳐봅시다.

: prevState 변수를 이용하면 이전에 state값을 가져올 수 있다.

```jsx
|		 console.log("parent state")  //
|	 }
|	 componentDidMount(){
|		 console.log('parent 마운트되었습니다.') //
|	 }
|
|	 handlingChange =(event)=> {
|		 this.setState({content : event.target.value})
|	 }
|
//(2)버튼 이벤트를 핸들링할 함수를 선언한다. 
|  handlingClock=(event)=>{
|    this.setState((prevState)=>({show : !prevState.show}))
|  }
//이전 state의 반대를 가져오게 하면 클릭 때마다 state.show의 값이 반대로 변한다.
//true <---> false
```

아니 위에서 말한 것처럼 했는데, 왜 때문에 오류가 뜰까요? UnMountComponent에서는 state update를 할 수가 없다는데요? ㅜㅜ

: componentWillUnMount에서 이러한 비동기적인 부분을 cancel시켜야합니다.(시간부분을 없어지게 하는것이므로, 시간 state도 삭제해야징!)

: 자식컴포넌트인 WorldClock에 가서 코드를 고쳐봅시다.

```jsx
|class WorldClock extends React.Component {
|  constructor(props){
|    super(props)
|		 this.state = {
|			 hours : this.props.time,
|			 minute : 0,
|			 stop : false,	
|		 }
|   console.log('child start') 
|  }
|  componentDidMount(){
|		 this.timer = setInterval(()=>{    //이부분
|			 this.setState((state)=>(
|        state.minute === 59 
|        ?{hour : state.hour + 1, minute : 0}
|        ?{minute : state.minute + 1}
|			 ))
|	  }, 100)
|   console.log('child mount')
|  }
|  componentWillUnmount(){  //
|    console.log('child unmount')
|    clearInterval(this.timer)  //타이머를 없앤다!
|  }
```

: 그러면 무사히 시간컴포넌트가 사라지며, console에서는 아래 사진과 같은 과정이 이루어진다.(자식 언마운트 → 부모 업데이트 )

![CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__8.40.37.png](CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__8.40.37.png)

---

> E. 결론

![CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__8.42.28.png](CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__8.42.28.png)

: 대부분의 비동기적인 요청(모든 다운로드, 타이머 등)을 작업한다.

: 만약 이 과정들을 취소하려면 unMount를 한다.

![CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__8.43.02.png](CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__8.43.02.png)

: 위에서 props를 받아서 변화되었는지 체크하고, state를 업데이트한다.

: if라는 조건을 걸어서 state를 설정하게 하자.(if라는 분기처리하자!)

![CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__8.44.03.png](CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__8.44.03.png)

: 아래와 같은 중지를 해야 데이터나 서버의 손실을 막을 수 있다.

![CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__8.45.14.png](CLASSLION%20LifeCycle%20c5ce0b08b01b48c7974fc9973b3232fb/_2019-09-15__8.45.14.png)

[CLASSLION/Hook&review](https://www.notion.so/CLASSLION-Hook-review-08f22eb624d6432ba0ac6ce98c695aca)