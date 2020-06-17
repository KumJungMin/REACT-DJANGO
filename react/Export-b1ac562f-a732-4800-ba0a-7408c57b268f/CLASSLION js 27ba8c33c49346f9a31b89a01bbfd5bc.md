# 리액트를 배우기 전 자바스크립트 기초

## A. arrow function

### a) 문법

아래와 같은 코드를 2가지 방식의 arrow function으로 변경시켜보자.

```jsx
function plusTwo(s){
	return s+2
}
console.log(plusTwo(10))     //12
```

첫 번째 방법은 1단계로 최소화하는 방법이다.

```jsx
plusTwo=(s)=>{
	return s+2
}
console.log(plusTwo(10))     //12
```

-먼저 함수 function이라는 키워드를 삭제한다.

-함수명 뒤에 바로 `=`을 붙이고 매개변수를 `()`안에 넣는다. 

-매개변수를 괄호안에 넣었다면 다음으로 `⇒`을 하고 `{}`안에 코드를 입력한다. 

두 번째 방법은 코드를 한 줄로 줄이는 arrow function방식이다.

```jsx
plusTwo =(s)=> a+2
colsole.log(plusTwo(10))   //12
```

-괄호안에 실행시킬 코드를 적었는데 이 괄호를 지운다.

-return키워드를 삭제한다.

-파라미터가 한 개인 경우 괄호를 없애도 된다.

<br/>

### b) 활용

우리는 왜 arrow function을 쓰는 걸까?

이 이유에는 Map, Filter함수의 이용과 관련이 있다.

map함수는 파이썬의 `for i*i in arr`와 같이 배열안의 원소값을 변경시킨다.

아래는 자바스크립트에서 map함수를 쓰는 조금 긴 방법이다.

```jsx
arr = [1,2,3,4,5,66,13,56,658,22467]
arr_map = arr.map(function(v){return v*2}) //long
console.log(arr_map)  //arr의 원소들이 2배가 된 값들이 호출
```

아래 코드에서는 function부분을 arrow function버전으로 작성하면 코드를 줄일 수 있다.

```jsx
arr_map = arr.map(v=>v*2) //short
```

filter함수는 어떤 조건에 만족하는 값을 뽑아낸다.

```jsx
arr_filter = arr.filter(v => v > 10) //10보다 큰 원소를 뽑도록 한다.
```

<br/><br/>

---

## B. Class & Super

### a) class와 constructor란?

파이썬에서는 초기값을 선언할 때는 `__init__()`을 썼는데, 자바스크립트에서는 constructor()이 이 역할을 수행한다. constructor()에서 선언한 변수들을 get, set메소드를 이용하여 값을 조정하거나 출력할 수 있다.

```jsx
class Lion{
	constructor(name){ //초기값 설정
		this.name = name
  }
	getName(){
		console.log("my name is" + this.name)
	}
}
mylion = new Lion("king")
mylion.getName()              //my name is king
```

<br/>

### b) 상속

중복적으로 쓰이는 코드를 모듈화하여 상속을 통해 사용이 가능하다. 

상위 클래스에서 선언하여 하위클래스에서 이 코드를 사용한다.

```jsx
class Animal{
	constructor(leg){
		this.leg = leg
	}
	printAnimal(){
		colsole.log(this.name+"="+String(this.leg)+"개의 다리를 가진다.")
	}

}
//Lion -> animal부터 유용한 코드 가져다쓰기!
class Lion extends Animal{
	constructor(name, leg){       //초기값 설정
		super(leg)                  //부모의 input파라미터
		this.name = name
  }
	getName(){
		console.log("my name is" + this.name)
	}
}

mylion = new Lion("king", 4)
mulion.printAnimal()
```

-먼저 부모클래스를 정의한다.

-부모클래스에 있는 코드들을 상속받기 위해 `extends 부모 클래스`를 해준다.

-자식클래스에서 부모클래스를 상속하기 위해서 `super()`를 해줘야한다.

<br/><br/>

---

## C. Asynchronous비동기, 동기

### a)  비동기란

비동기는 모든 게 절차적으로 실행되지 않는다. 실행을 하고 그 결과를 가져오기 전에 다른 실행을 작동시킬 수 있다. 아래 코드는 비동기의 대표적인 예이다. 일반적으로 생각해보면, hi가 뜨고 그다음에 bye가 뜬다고 생각한다.(동기적 사고)

하지만 실제로 코드를 실행시켜보면 bye가 먼저나오고 2초 뒤에 hi가 호출된다.

이 코드의 경우, hi가 2초뒤에 실행된다고 해도 bye는 기다리지 않고 실행되는 것이다.

```jsx
setTimeout( ()=>{console.log("hi") }, 2000 )
console.log("bye")
```

자바스크립트는 인터넷 속도에 의존적이며, 유저 인터랙션, 속도가 느려도 인터랙션이 되어야 한다.

![CLASSLION%20js%2027ba8c33c49346f9a31b89a01bbfd5bc/_2019-08-19__11.05.36.png](CLASSLION%20js%2027ba8c33c49346f9a31b89a01bbfd5bc/_2019-08-19__11.05.36.png)

대표적인 비동기로는 패이스북을 예로 들 수 있는데, 패이스북의 경우 사용자가 접속시 바로 화면이 안뜨면 불편해하므로 일단 배경을 띄우고, 그동안 글을 로딩하는 형태로 진행된다.

<br/>

### b) callback

이렇게 비동기적인 형태에서 하나의 문제가 발생할 수 있다. 현재 내가 로딩 중인 데이터를 꼭 필요로 한다면 상당히 답답할 것이다.

이에 대한 대안책으로 사용되는 게 `callback`이다.(다 되면 알려줘!)

아래 코드는 콜백을 활용한 코드이다. 콜백기능을 쓰고자 할 때는 매개변수로 전달되는 모든 것이 함수형이어야 한다.

```jsx
function sayHello(ByeCallback){
	setTimeout( ()=>{
        colsole.log("hi")
        ByeCallback()
  }, 2000);
}

sayHello(() => console.log("bye"))  //void형태의 함수

//2초의 딜레이후 hi , bye가 순서대로 출력된다.
```

-콜백을 하고자 하는 함수를 정의한다.(이때 인수로 함수형 매개변수를 받음)

-이 함수내부에 setTimeout()에 2초 후에 hi를 실행시키고, 그 다음에 인수로 받은 함수를 실행한다.

-결과적으로 이 코드는 2초후에 hi를 출력하고 그다음에 bye를 콜백해줘! 하는 함수가 된다.

<br/><br/>

---

## D. Promise, Async - Await

### a) promise?

promise란, 언젠가 해결할 것이란 약속이다. 이 약속이 해결되면 해결됐다면 알려준다.

![CLASSLION%20js%2027ba8c33c49346f9a31b89a01bbfd5bc/_2019-08-19__11.25.55.png](CLASSLION%20js%2027ba8c33c49346f9a31b89a01bbfd5bc/_2019-08-19__11.25.55.png)

그 결과를 then, catch으로 반환받고 이제 동기적인 처리가 가능해진다.

```jsx
function sayHello(name, ByeCallback){
	setTimeout( ()=>{
          colsole.log(name+"hi")
          ByeCallback()
      }, 2000);
}

sayHello("mike", function(){
	console.log("bye")
})

function sayHello2(name) {
	return new Promise(resolve, reject) => {
		setTimeout(()=>{
			colsole.log(name+"님 hi")
			resolve()    
		},3000)
	}
}
//frank를 보내서 실행시키는 약속을 하는데 만약 잘 실행되면 bye를 실행할게
sayHello2("frank")   
	.then(()=> colsole.log("bye"))
```

우리는 promise를 통해 어떤 코드를 실행시키는 것에 대한 약속을 한다.

이 약속이 만약 지켜진다면 then을 통해 특정 코드를 또한 실행시킬 수 있다.

-함수에 `return new Promise(resolve, reject)`를 선언한다.

-약속함수에 `resolve()`를 넣어, 이 부분이 잘 실행된다면 → `then`을 통해 알려준다.

```jsx
sayHello("mike", function(){
	console.log("bye")
})

function sayHello2(name) {
	return new Promise(resolve, reject) => {
		setTimeout(()=>{
			colsole.log(name+"님 hi")
			resolve("서울")    
		},3000)
	}
}
//frank를 보내서 실행시키는 약속을 하는데 만약 잘 실행되면 bye를 실행할게
sayHello2("frank")   
	.then((seoul)=> colsole.log(seoul+"bye"))
```

resolve()에 인수를 넣을 수도 있다.

resolve()에 입력한 인수값이 then의 seoul이라는 매개변수에 전달된다.

그런데 then이 너무 직관적이지 않아! then을 더 직관적으로 쓰고 싶을 수도 있다.

이 경우에는 Async Function을 이용하여 then을 더 직관적으로 사용할 수 있다.

<br/>


### b) Async Function(then의 대체!)

이 함수의 사용은 쉽다.

![CLASSLION%20js%2027ba8c33c49346f9a31b89a01bbfd5bc/_2019-08-19__11.44.11.png](CLASSLION%20js%2027ba8c33c49346f9a31b89a01bbfd5bc/_2019-08-19__11.44.11.png)

-먼저 어떤 함수가 있을 때 그 함수 앞에 `async`를 적어준다.

-어떤 비동기적인 함수(ex. promise)앞에 `await`를 앞에 적어주면, 어떤 결과가 나올때까지 기다려준다(동기적으로 하겠다).

-이 결과로 result를 받거나 아무것도 안 받을 수도 있다.

```jsx
sayHello("mike", function(){
	console.log("bye")
})

function sayHello2(name) {        //4. name = sang
	return new Promise(resolve, reject) => {
		setTimeout(()=>{
			colsole.log(name+"님 hi")    //5.sang님 hi
			resolve("서울")               //6.resolve의 인자 = 서울
		},3000)
	}
}

//new
async function sayHelloBye(name){ //2.name = sang
	loc = await sayHello2(name)     //3.sayHello2(sang)
//await가 있으므로 이 함수를 실행시킨후 다음 코드를 실행하게 한다.
	console.log(loc+"bye")          //7. loc = 서울
}  

sayHelloBye("sang")                //1. name = sang
//sang님 hi
//bye 
```

---

![CLASSLION%20js%2027ba8c33c49346f9a31b89a01bbfd5bc/_2019-08-19__1.35.20.png](CLASSLION%20js%2027ba8c33c49346f9a31b89a01bbfd5bc/_2019-08-19__1.35.20.png)
