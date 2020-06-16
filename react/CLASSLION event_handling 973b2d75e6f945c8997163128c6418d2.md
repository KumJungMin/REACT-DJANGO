# CLASSLION/event_handling

# 1. event

이벤트를 왜 배우나요?

: 이벤트는 웹 상에서 모든 행위로, 많이 존재하는데 이걸 이용해보자.

ex) 창을 열었을 때 팝업창이 뜨게 하는 경우

: 이벤트를 진행하고자 한다면 on을 앞에 붙여준다.

```jsx
on + (Event)
document.onClick =()=> console.log('document clicked');
```

---

# 2. event_handling

앞서 이벤트에 대해 잠깐 알아보았다. 그렇다면 이러한 이벤트는 리액트에서 어떻게 이용해야할까

- state를 만든다.(단, state를 생략하는 경우도 있음!)

    : props로 해결이 안되고 데이터를 보여줘야하는 경우에 state를 씀

- handling함수를 만든다.
- 이벤트가 발생하는 html 태그에서 onEvent명을 통해 handling함수를 부른다.

## A. 실습

a) 어떤 버튼을 누르면 시간이 멈추게 해보자

> App.js

- WorldClock컴포넌트에서 state stop을 만든다.

    처음 시작할 때는 시간이 무한정으로 흐르니까 stop : false로 초기값을 설정한다.

```jsx
//방법1
|class WorldClock extends React.Component{
|	constructor(props){
|		this.state = {
|			hour : this.props.time,
|			minute : 0,
|			stop : false,    //
|		}
|	}	
```

- WorldClock컴포넌트안에 arrow_function형태의 핸드링함수를 정의한다.

    : 왜 arrow function 형태로 작성하나요?(코드가 더 쉬움)

    : 이벤트 정보`(매개변수 event)`가 오면 this`(worldclock)`의 state를 설정한다.

    : event.target는 이벤트를 눌렀을 때 일어나는 특정 범위로, 우리가 stop버튼을 눌렀을 때, value = true가 핸드링범위에 넘어온다.

```jsx
|	handlingClick =(event)=> {
|   this.setState({stop : event.target.value});
|   clearInterval(this.timer)  //시간을 멈추게 하는 과정2
|	}
```

- WorldClock컴포넌트의 return부분에 onclick 코드를 쓴다.

```jsx
|	render(){
|   return(
|     <div className ={"WorldClock"}> 
|       <h1>{this.props.city}</h2>
|       <p>{this.state.hour}시 {this.state.minute}분</p>
|       <button value={true} onClick={this.handlingClock}>stop</button>
|     </div>
|    )
|  }
|}
```

- WorldClock의 setInterval함수를 timer변수에 저장한다.

    저장한 timer를 이 컴포넌트 변수(this)단위에 두고, 이것을 clearInterval을 하면 이벤트에서 시간을 멈추게 만들어준다.

```jsx
|		this.timer = setInterval(()=> {  //시간을 멈추게 하는 과정1
			this.setState((state)=> (
				state.minute === 59
	      ?{hour : state.hour + 1, minute : 0}
	      ?{minute : state.minute + 1}
			))
		}, 100)
	}
```

b) input이벤트를 만들어보자.

> App.js

- 먼저 function 형태로 저장한 App을 class형식으로 변경한다.

```jsx
class App extends React.Component{
	consrtuctor(props){
		super(props)
		this.cityTimeData = [
			['seoul', 10],
			['ba', 9],
			['si', 12],
			['la', 17],
			['busan', 10]
		]
		this.state={
			content : ''
		}
	}

	handlingChange =(event)=> {
		this.setState({content : event.target.value})
	}

	render(){
		return(
			<div className='App'>
				<h1 className={'myStyle'}> hi </h1>
				<div className={'post'}>
					first post
					<textarea value={this.state.content} onChange={handlingChange}></textarea>
//빈칸에 입력 -> onChange시 -> content에 저장
				</div>
				{this.cityTimeData.map((citytime, index)=>
					<WorldClock city={citytime[0]} time={citytime[1]} key={index} />
				)}
		  </div> 
   );
 }
}
```

[CLASSLION/LifeCycle](https://www.notion.so/CLASSLION-LifeCycle-67b1e312fb9e48398557399419799642)