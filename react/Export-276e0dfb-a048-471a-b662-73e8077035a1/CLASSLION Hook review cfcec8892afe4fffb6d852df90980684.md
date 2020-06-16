# CLASSLION/Hook&review

# 4. Hook

: Hook을 이용하여 class를 작성할 필요없이 상태값과 react의 여러 기능(라이프사이클)을 쓸 수 있다.

![CLASSLION%20Hook%20review%20cfcec8892afe4fffb6d852df90980684/_2019-09-15__8.49.19.png](CLASSLION%20Hook%20review%20cfcec8892afe4fffb6d852df90980684/_2019-09-15__8.49.19.png)

> A. useState의 기본 사용

a) 사용 예시

![CLASSLION%20Hook%20review%20cfcec8892afe4fffb6d852df90980684/_2019-09-15__9.06.11.png](CLASSLION%20Hook%20review%20cfcec8892afe4fffb6d852df90980684/_2019-09-15__9.06.11.png)

: 기본적으로 state는 아래와 같은 순서로 import → setState순으로 코딩된다.

```jsx
//1. useState를 import한다.
|import React, {useState} from 'react';
|
|function Example(){
//"count"라는 새로운 상태 값을 정의한다.
//2. useState(초기값)을 하면 count라는 상태변수가 생성와
//이 count의 변수값을 조정할 수 있는 setCount가 함께 생긴다. 
| const [count, setCount] = useState(0);
|	
|	return(
|		<div>
|			<p> you clicked {count} times </p>
|			<button onClick={() => setCount(count+1)}>
|				click me
|			</button>
|		</div> 
|	);
|}
```

b) state변수의 사용

: 하나의 컴포넌트 내에서 state hook을 여러 개 사용할 수 있으며 ()안에 여러 형태의 초기값을 넣을 수 있다.

```jsx
function Example(){
	const [age, setAge] = useState(10);
	const [fruit, setFruit] = useState("banana");
	const [todos, setTodos] = useState([{text : "aa"}]);
}
```

> B. Effect Hook

:   라이프사이클을 대체하는 hook으로, 실제로 작동방식은 조금 다르다. (라이프사이클을 흉내낸 문법형태)(자세한 건 react 공식문서에 있음!)

```jsx
import React, {useState, useEffect} from 'react';

function Example(){
	const [count, setCount] = setState(0);

	//componentDidMount, componentDidUpdate와 비슷함
	useEffect(()=>{
	//브라우저 api를 이용해 문서의 타이틀을 업데이트함.
		document.title = 'you clicked ${count} times';
	});

	return(
		<div>
			<p> you clicked {count} times </p>
			<button onClick={() => setCount(count + 1)}>
				Click me
			</button>
		</div>
	);
}
```

# 5. REVIEW

> react는...

![CLASSLION%20Hook%20review%20cfcec8892afe4fffb6d852df90980684/_2019-09-15__9.07.34.png](CLASSLION%20Hook%20review%20cfcec8892afe4fffb6d852df90980684/_2019-09-15__9.07.34.png)

: 결국 react는 `DOM > APP > PARENT_COMPONENT > CHILD_COMPONENT`로 구성된다.

: 상위컴포넌트는 prop를 통해 해당 상위컴포넌트값을 가져올 수 있으며, 하위 컴포넌트는 state를 통해서 값을 변경할 수 있다.

하지만 이제는 여러 페이지의 리액트 컴포넌트를 다루고자 할 때는?

![CLASSLION%20Hook%20review%20cfcec8892afe4fffb6d852df90980684/_2019-09-15__9.09.49.png](CLASSLION%20Hook%20review%20cfcec8892afe4fffb6d852df90980684/_2019-09-15__9.09.49.png)

: 리액트 라우터를 쓰면 여러 페이지를 만들고, 링크를 통해 페이지간 이동할 수 있다.

하위 컴포넌트에 존재하는 여러 state를 일괄 관리하고자 할 때는?

![CLASSLION%20Hook%20review%20cfcec8892afe4fffb6d852df90980684/_2019-09-15__9.11.42.png](CLASSLION%20Hook%20review%20cfcec8892afe4fffb6d852df90980684/_2019-09-15__9.11.42.png)

: 리덕스를 이용하면 하위컴포넌트의 state값을 일괄 관리할 수 있다.

: 또는 컨테스트라는 것을 이용하면 되는데 어려움;;

---

> 추가

리액트 컴포넌트로 페이지를 구성할 때의 구조를 보려면 아래 페이지로 가보자.

[Divjoy](https://divjoy.com/build)

[https://divjoy.com/build](https://divjoy.com/build)