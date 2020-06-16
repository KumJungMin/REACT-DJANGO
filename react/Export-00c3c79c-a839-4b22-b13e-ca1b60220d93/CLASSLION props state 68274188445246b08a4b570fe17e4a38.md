# CLASSLION/ props & state

# 3. Props & State

우리는 이전 학습 자료를 통해 4가지를 알게 되었다.

- React는 유저에게 빠르게 반응한다.
- 그 이유는 컴포넌트 개별로 Re-render가 일어나기 때문이다.
- 그러므로 우리는 컴포넌트를 잘 기획하고 나누어 작성해야한다.
- create-react-app을 이용해 프로젝트를 생성한다.

## A. Props

### a) What is Props?

![CLASSLION%20props%20state%2068274188445246b08a4b570fe17e4a38/_2019-08-19__3.58.33.png](CLASSLION%20props%20state%2068274188445246b08a4b570fe17e4a38/_2019-08-19__3.58.33.png)

: 각 역들간의 이동에 있어, 역과 역 사이의 데이터 이동수단이다.(데이터를 싣고 다니는 기차!)

: 이러한 Props의 가장 큰 특징은 "Read Only"이다.

 즉, 위에서 온 props의 값은 컴포넌트들이 변경할 수 없다는 것! 두둥! 딱~

 이것 때문에 state, props를 따로 저장하게 된다.(뒤에 예시보면 알게 됨 :>)

: props의 예로, 시간을 나라별로 보여주는 경우를 생각해보자

> App.js

: 컴포넌트는 function을 통해서 정의할 수 있다.

: 모든 컴포넌트들은 단 하나의 최상단의 div태그 하나만 있어야 한다.

(하나의 최상단 태그만 존재한다.)(최상단에 div가 2개 일 수는 없음! 삐—-)

```jsx
function WorldClock(props){
	return(
		<div>
			<div className={"worldcClock"}>
				<h2>도시: {props.city} </h2>
				<p>시간: {props.time}시 </p>
			</div>
		</div>
	);	
}

function App(){
	return(
		<div className={'App'}>
			<h1 className={'myStyle'}>안녕하세요</h1>
			<div className={'post'}>첫 게시물이당</div>
			<WorldClock city={'서울'} time={10}/>
			<WorldClock city={'베이징'} time={9}/>
			<WorldClock city={'시드니'} time={12}/>
			<WorldClock city={'LA'} time={17}/>   
		</div>//(이 값이 props에 의해 worldclock에 전달)
	);
}
```

: WorldClock컴포넌트는 props를 인수로 받는데, 이때 도시와 시간은 props에서 온 데이터를 이용할 것이다. props에 있는 데이터를 쓰고자 할 때는 `{}`를 이용한다.

: 우리는 default로 App컴포넌트를 수출하므로, WorldClock컴포넌트를 App컴포넌트에 태그 형식으로 집어넣는다.(App컴포넌트에 블록처럼 WorldClock 끼워넣기)

: 만약 App컴포넌트의 `WorldClock컴포넌트 태그`에 `city`, `time`에 값을 넣으면 이 값이 props이므로 worldClock컴포넌트에 전달된다.

우리는 어떤 컴포넌트태그의 속성값에 값을 넣으면, 이것이 props로 넘어간다는 걸 알 수 있음.

![CLASSLION%20props%20state%2068274188445246b08a4b570fe17e4a38/_2019-08-19__4.13.40.png](CLASSLION%20props%20state%2068274188445246b08a4b570fe17e4a38/_2019-08-19__4.13.40.png)

---

## B. List & Key

우리는 위에서 하나의 컴포넌트를 여러 번 사용했다.

이제 이 컴포넌트에서 나타나는 중요 개념과 여러 번 작성하지 않는 편한 방법을 알아보자!

### a) 반복적으로 적었던 태그의 속성값들을 변경해보자.

> App.js

: 이때 `citytime`은 `cityTimeData에서 한 줄의 데이터`를 지칭한다.

 그러므로 `cityTimeData[0]`이라고 하면 2차원배열 안의 첫번째 원소를 말한다.(seoul, ba, ci, la)

: 그런 다음에 App컴포넌트 안에 이 WorldClockList를 자바스크립트 괄호`{ }` 안에 넣어서 코드를 작성해준다.

```jsx
function App(){
	const cityTimeData = [
		['seoul', 10],
		['ba', 9],
		['ci', 12],
		['la', 17]
	]
//cityTimeData에 맞게 worldClock에 데이터를 넣을 수 있다.
//파이썬에서 for i*i in [1,2,3,4]와 같은 map함수	
	const WorldClockList = cityTimeData.map((citytime, index)=>
		{
			<WorldClock city={cityTimeData[0]} time={cityTimeData[1] key={index}}/>

		}
)

	return(
		<div className="App">
			<div className={'myStyle'}>안녕하세요</div>
			<div className={'post'}>
				첫 게시물 입니다.
			</div>
			{WorldClockList}
		</div>
		
	);
}

export default App;
```

: 이를 통해 우리는 map을 통해 각각의 원소들을 컴포넌트에 넣을 수 있다는 걸 알게 되었다.

: 이렇게 여러개의 child컴포넌트들을 만드는데 중요한 점이 하나 있다. 

이때 각 컴포넌트들을 식별할 수 있는 key값도 지정해줘야한다는 점! 다행스럽게도 우리는 `index`라는 매개변수를 통해 해당 데이터에 대한 인덱스 번호를 가져올 수 있다! `key={index}`를 통해 각 child컴포넌트간에 식별가능한 성질을 부여한다.

: 이것은 map함수의 매개변수 특징때문에 가능하다. map함수는 첫번째 인자로 무조건 함수를 받지만, 그 다음 부터 두번째 인자로는 무조건!! 인덱스를 받게 된다.

![CLASSLION%20props%20state%2068274188445246b08a4b570fe17e4a38/_2019-08-19__4.13.40.png](CLASSLION%20props%20state%2068274188445246b08a4b570fe17e4a38/_2019-08-19__4.13.40.png)

: 공식문서에 보면 key에 대해서 이렇게 설명한다.

우리는 키를 통해 이 컴포넌트의 값이 삭제, 변경사항을 식별하고 인지할 수 있게 해준다

---

## C. State (1)

state는 각 역(component)들이 가지고 있는 데이터의 집합이다.

이 정보들은 props를 통해 자식(하위)컴포넌트에 넘길 수 있다.

![CLASSLION%20props%20state%2068274188445246b08a4b570fe17e4a38/_2019-08-19__3.58.33.png](CLASSLION%20props%20state%2068274188445246b08a4b570fe17e4a38/_2019-08-19__3.58.33.png)

이번 시간에는 컴포넌트 각각이 가지고 있는 state라는 상태에 대해 알아보자.

(주의, state라는 컴포넌트에 있을 수도 없을 수도 있음 쾅쾅!)

이 state는 비용이 드는 작업이므로, 정말 필요한 곳에만 state를 넣어야 한다.

### a) When we use state?

: state를 써야 하는 경우는 아래와 같다.

- props만으로도 표현할 수 있는가  —> (props만으로 표현할 수 없엉!)
- render로 표시되지 않는 값인가    —> (render로 표시되는 값이야!)

: 이 2개에 모두 해당하지 않는다면 state를 써야한다.

: 여기 state와 stateless(state를 쓰지 않는)컴포넌트가 있다.

 좌측에는 function, class 형태로 컴포넌트를 선언하냐에 대해 말하고 있다.

> 과거

function은 state를 가질 수 없었음 → 이 말은 state를 써야 하는 경우에는 function 컴포넌트를 사용할 수 없었다는 말

아니 그러면 걍 class로 다 만들면 안됨?

: ㅇㅇ 안됨!! class로 다 만들어버리면 function으로 만들었을 때보다 더 시간이 걸리고 코드가 더 무거워진다. 그러므로 때에 따라 class로 선언해야한다.

![CLASSLION%20props%20state%2068274188445246b08a4b570fe17e4a38/_2019-08-19__4.52.58.png](CLASSLION%20props%20state%2068274188445246b08a4b570fe17e4a38/_2019-08-19__4.52.58.png)

> 현재

그럼 state를 쓰면 가볍고 코드가 짧은 function 못씀?

: ㄴㄴ 과거에는 안됐는지 지금은 가능! 리액트의 `hook`을 통해서 function 컴포넌트에서도 state를 쓸 수 있게 되었다.

![CLASSLION%20props%20state%2068274188445246b08a4b570fe17e4a38/_2019-08-19__4.52.11.png](CLASSLION%20props%20state%2068274188445246b08a4b570fe17e4a38/_2019-08-19__4.52.11.png)

하지만 아직도 class component에서 state를 쓴 경우가 있다. 그래서 오늘은 class component에서 state를 쓰는 방법을 먼저 알아보고나서 hook을 배워보자!

### b) Class Component에서 state쓰기

> App.js

: 이전에 만들었던 App.js의 컴포넌트를 클래스컴포넌트로 변경하자.

: constructor(props)의 `super(props)`는 `react.component의 input`이다!

: 우리는 클래스에서 React.Component를 extends 했으므로 리액트의 컴포넌트 기능을 사용할 수 있다.

: 그런데 앞서 우리는 state를 어떠한 조건에서만 되도록 사용하고자 했다.

- [ ]  요구사항1 ( 시간, 분의 변화를 보고 싶다면!)

- [ ]  요구사항2 ( 동적으로 보고싶다면! )

```jsx
function App(){
	const cityTimeData = [
		['seoul', 10],
		['ba', 9],
		['ci', 12],
		['la', 17]
	]
//cityTimeData에 맞게 worldClock에 데이터를 넣을 수 있다.
//파이썬에서 for i*i in [1,2,3,4]와 같은 map함수	
	const WorldClockList = cityTimeData.map((citytime, index)=>
		{
			<WorldClock city={cityTimeData[0]} time={cityTimeData[1] key={index}}/>
    //이 값들이 props라는 기차를 타고 WorldClock으로 이동 슉슉
		}
)

	return(
		<div className="App">
			<div className={'myStyle'}>안녕하세요</div>
			<div className={'post'}>
				첫 게시물 입니다.
			</div>
			{WorldClockList}
		</div>
		
	);
}

export default App;
```

```jsx
class WorldClock extends React.Component {
	constructor(props){
		super(props)
	
	setInterval(()=>{                //1초에 1번씩 state값을 변경한다.
		this.setState({ (state)=>      //이전의 state값을 가져와서
//그런데 계속 두게 되면 60분을 넘어가서 100이상으로 가버리므로 우리가 설정을 해준다
			state.minute === 59 
      ?{hour : state.hout + 1, minute : 0}
			:{minute : state.minute + 1} //+1을 해서 state의 minute에 저장
		})

	},1000)

//render함수를 통해 어떤 데이터를 어떻게 보여줄지 적어준다.
//render라는 약속괸 void형태의 함수라고 생각하자
//우리는 이걸 리액트 컴포넌트에 보여줄거야
	render(){
		return(
//여기서 this는 이 WorldClock를 지칭
//
			<div className={'WorldClock'}>
				<h1> city : {this.props.city} </h1>
				<p> time : {this.state.hour}시 {this.state.minute} </h1>
			</div>	
		);
	}
}
```

: state의 값을 변경할 때는 setState를 이용해야한다.

  이때 우리는 리액트 컴포넌트에 있는 setState모듈을 쓰는 것이므로 this를 붙여준다. 

  하지만 우리는 라이프사이클을 학습하지 않았으므로 동적으로 값이 변화하는 걸 표현할 수 없다. 

  현재로서는 `setInterval()`이라는 모듈을 이용한다. 

[CLASSLION/event_handling](https://www.notion.so/CLASSLION-event_handling-973b2d75e6f945c8997163128c6418d2)