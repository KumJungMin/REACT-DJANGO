# react & props

## A. What is react (1)

### a) What is react?

: 패이스북에서 만든 웹 라이브러리이다.

<br/>

### b) What is DOM?

![CLASSLION%20react%20props%20cd615825d72641e9ac1ab1f2083f9dc3/_2019-08-19__1.41.54.png](CLASSLION%20react%20props%20cd615825d72641e9ac1ab1f2083f9dc3/_2019-08-19__1.41.54.png)

: dom은 웹 페이지 자체이며 이 웹페이지 자체가 상당히 구조적으로 구성되어 있다.

: 예를 들어 div를 변경하고자 할 때 div안에 p태그가 귀속되어 있어 영향을 받는다.

: div라는 거대한 트리 구조가 있다.

<br/>

### c) What is Component?

![CLASSLION%20react%20props%20cd615825d72641e9ac1ab1f2083f9dc3/_2019-08-19__1.45.29.png](CLASSLION%20react%20props%20cd615825d72641e9ac1ab1f2083f9dc3/_2019-08-19__1.45.29.png)

: dom과 상당히 유사하다.(계층구조)

:  네모박스들은 컴포넌트이며, 노란색 컴포넌트에는 파란색, 연두색 컴포넌를 포함하고 있다.

: dom은 문서 구조 자체이며, 컴포넌트는 이 구조들을 내가 원하는 기능별로 묶었다고 볼 수 있다.


<br/><br/>

---

## B. What is react (2)

앞서 우리는 dom과 컴포넌트에 대해 알아보았다.

이번에는 이러한 컴포넌트는 리액트에서 어떻게 쓰이며 어떠한 경우에 쓰면 좋은지 알아보자.

<br/>

### a) example of react construction (1)

> 만약 강남에서 정자까지 가는 지하철이 있다고 가정해보자.

: 우리가 강남에서 정자까지 가는 길에 중간중간 위치한 역을 거쳐 승객을 내려주고 다시 태워준다.

: 이 과정은 마치 강남(데이터시작)에서 정자(클라이언트의 웹페이지 화면)로 데이터가 이동해서 보여지는 것과 유사하다.

: 각 역들은 일종의 컴포넌트라고 볼 수 있는데, 각 컴포넌트를 지날 때마다 데이터를 실거나 데이터를 내려준다.

<br/>

> 만약 어떤 컴포넌트에서 상태가 변경된다면?

![CLASSLION%20react%20props%20cd615825d72641e9ac1ab1f2083f9dc3/_2019-08-19__1.52.10.png](CLASSLION%20react%20props%20cd615825d72641e9ac1ab1f2083f9dc3/_2019-08-19__1.52.10.png)

: 우리가 코드를 대충 짰다면, 강남에서 (데이터가 변경된)판교까지 가서 데이터를 변경시키고 그리고 나서 정자(클라이언트)에게 보여진다. 이런 경우, 속도가 느리다는 단점이 있다.

(새로고침되면서 하얀 페이지가 나오고 한참이 지나서야 내가 원하는 페이지가 나오는 경우)

<br/>

> : 만약 판교에서 데이터가 변경되었다면?

![CLASSLION%20react%20props%20cd615825d72641e9ac1ab1f2083f9dc3/_2019-08-19__2.04.47.png](CLASSLION%20react%20props%20cd615825d72641e9ac1ab1f2083f9dc3/_2019-08-19__2.04.47.png)

: 이 경우에 리액트는 판교에서 데이터가 출발한다.

<br/>

> 만약 청계산에서 데이터가 변경되었다면?

![CLASSLION%20react%20props%20cd615825d72641e9ac1ab1f2083f9dc3/_2019-08-19__2.06.15.png](CLASSLION%20react%20props%20cd615825d72641e9ac1ab1f2083f9dc3/_2019-08-19__2.06.15.png)

: 청계산에서 출발을 하는데 대신 판교를 들렀다가 클라이언트가 있는 정자에 도착하게 된다. 

<br/><br/>

### b) example of react construction (2)

가상의 구조를 상상해보자!

![CLASSLION%20react%20props%20cd615825d72641e9ac1ab1f2083f9dc3/_2019-08-19__2.13.31.png](CLASSLION%20react%20props%20cd615825d72641e9ac1ab1f2083f9dc3/_2019-08-19__2.13.31.png)

: 유저는 현재 `Browser DOM (1)` 의 첫번째 구조를 보고 있다.

 그동안 `Virtual DOM (1)`의 첫번째 구조에서는 빨간색의 state 데이터에 변경이 생긴다.

: 컴퓨터는 유저가 보는 `BrowserDOM(1)`과 `VirtualDOM(1)`을 비교하여 건드릴 컴포넌트의 범위를 체크한다.(체크한 결과가 `VirtualDOM(2)`)

: `VirtualDOM(3)`에서 변경한 사항을 `BrowerDOM(3)`에 실제로 Re_render해준다.

: 예를 들면 페이스북에서 내가 댓글을 달았다면 → 댓글단 창의 데이터가 변경되고       
  → 변경된 댓글 결과창을 다시 보여주는 것과 같다. 나머지 수백 개의 글들은 변화될 필요가 없다. (이게 바로 리액트가 하는 일!)
  
  <br/>

### c)  Props와 컴포넌트의 state

![CLASSLION%20react%20props%20cd615825d72641e9ac1ab1f2083f9dc3/_2019-08-19__2.17.49.png](CLASSLION%20react%20props%20cd615825d72641e9ac1ab1f2083f9dc3/_2019-08-19__2.17.49.png)

: 앞서 설명한 기차를 예로 들면 `Props`가 기차, 이 기차가 변경된 데이터(자식)들을 다른 컴포넌트(역)에 옮겨준다. 각각의 데이터들은 `state`에 저장이 되며 이들은 컴포넌트에 각각 존재한다.

<br/>

![CLASSLION%20react%20props%20cd615825d72641e9ac1ab1f2083f9dc3/_2019-08-19__2.24.38.png](CLASSLION%20react%20props%20cd615825d72641e9ac1ab1f2083f9dc3/_2019-08-19__2.24.38.png)

: 현재 이 구조를 보면 되게 쉬워보이지만 만약 수백개의 역과 호선들이 있다면 구현에 많은 어려움이 생길 것이다. 그래서 우리는 원활한 코드 구성을 위해 위 사진과 같은 절차를 거쳐야 한다.


<br/><br/>

---

## C. Create-react-app

### a) What is NPM ?

: 노드 패키지 매니저이다.

: js에 들어가는 여러 라이브러리들을 관리해주는 도구이다.

: 파이썬에서는 pip install django를 하면, 전체 폴더에 설치가 완료된다. 하지만 NPM의 경우, 개별 프로젝트 폴더에 따로 설치가 된다.(node modules) 만약 전체적으로 사용하도록 설치하려면 (-g, global)을 해주면 가능하다.

: 사용은 npm을 통해 라이브러리에서 필요한 모듈을 불러와 설치한다.

: npx는 따로 모듈 설치없이 사용할 수 있도록 해주는 톨이다.

<br/>

### b) starting with installed

> 노드 js와 npm을 설치해준다.

- 설치 여부 확인

    노드와 npm의 설치 여부를 알고 싶다면 `node --version`, `npm --version`을 터미널에 쳐보자! 만약 버전이 나온다면 설치가 완료된 것!

```bash
홈페이지에서 설치하거나 맥의 패키지 매니저로 node.js를 설치한다.
```

<br/>

> 빈 폴더를 생성해서, 이 폴더에 설치해보자.

: npx를 통해 lion-app이라는 리액트 앱을 설치한다.

```bash
npx create-react-app lion-app
```

<br/>

> lion-app을 실행시켜보자.

```bash
cd lion-app
npm start
```

<br/>

> 코드에서 리액트 앱을 보자.

```bash
code .
```

<br/>

### C) rules of react files

 리액트 폴더 구조는 이렇습니다.

![CLASSLION%20react%20props%20cd615825d72641e9ac1ab1f2083f9dc3/_2019-08-19__2.40.27.png](CLASSLION%20react%20props%20cd615825d72641e9ac1ab1f2083f9dc3/_2019-08-19__2.40.27.png)

[React 핵심 파일](https://www.notion.so/1ef603d906e54d17ac54bf7d2eb63a9d)

- index.js

    ```jsx
    //index.js
    import App from './App'; 

    ReactDOM.render(<App />, document.getElementById('root'));
    ```

    : 리액트 DOM이 App이라는 컴포넌트를 index.html document의 `id = 'root'`인 곳에 뿌려줄거야!

    : 해당 컴포넌트들을 불러야 해당 파일에서 사용이 가능하므로 import를 해준다.

    (이때, 해당 컴포넌트를 불러올 때 같은 파일형식인 경우에는 파일형식을 생략해도 됨)

    : index.html에는 `id = 'root'`인 document가 있는데 여기에 해당 컴포넌트가 적용되어 사용자에게 해당 컴포넌트 내용이 보여진다.

    ```jsx
    //index.js
    ReactDOM.render(<h1>안녕 세상앍!</h1>, document.getElementById('root'));
    ```

    : 만약 우리가 App(컴포넌트, 파일단위)이 아닌 h1태그를 이용하여 특정 내용을 적는다면, 아래와 같은 화면이 적용된다.

    ![CLASSLION%20react%20props%20cd615825d72641e9ac1ab1f2083f9dc3/_2019-08-19__2.51.08.png](CLASSLION%20react%20props%20cd615825d72641e9ac1ab1f2083f9dc3/_2019-08-19__2.51.08.png)

<br/>

- index.html
- App.js

    : App.js 코드를 보면 App이라는 컴포넌트가 이미 생성되어 있다. 우리는 이 컴포넌트에 아래와 같은 코드를 작성하면 이 컴포넌트 내용이 → index.js → index.html로 전송된다.

    ```jsx
    //App.js
    function App(){        //컴포넌트
    	return(
    		<div className="App">
    			<h1>안녕하세요</h1>
    			<table>
    				<tr>
              <td>1</td>
              <td>2</td>   
            </tr>
    			</table>
    	);
    }
    export default App;   
    ```

    : 함수로 정의된 `App()`은 컴포넌트이며, `export default App;`을 통해 해당 컴포넌트를 수출한다.

    : `default`의 의미는 기본적으로 이거 하나만 수출하겠다는 말이다.

    : 아래 사진은 우리가 App.js에서 작성한 코드 내용이다.

    ![CLASSLION%20react%20props%20cd615825d72641e9ac1ab1f2083f9dc3/_2019-08-19__2.59.24.png](CLASSLION%20react%20props%20cd615825d72641e9ac1ab1f2083f9dc3/_2019-08-19__2.59.24.png)

    : 만약 App2라는 컴포넌트를 만들고 App2를 export한다면, 이 컴포넌트를 쓰는 index.js에서는 이 컴포넌트를 `import App from './App';`으로 받기때문에 app2를 App이라는 이름으로 별칭을 붙이고 쓴다.

    ```jsx
    //App.js
    function App(){        //컴포넌트
    	return(
    		<div className="App">
    			<h1>안녕하세요</h1>
    			<table>
    				<tr>
              <td>1</td>
              <td>2</td>   
            </tr>
    			</table>
    	);
    }

    function App2(){        //컴포넌트
    	return(
    		<div className="App">
    			<h1>안녕하세요2</h1>
    			<table>
    				<tr>
              <td>1</td>
              <td>2</td>   
            </tr>
    			</table>
    	);
    }

    export default App2;   
    ```

<br/><br/>

---

## D. JSX

앞서 생성한 컴포넌트에 실제로 내용을 채워보자.

그리고 이 컴포넌트들은 어떻게 꾸미는지도 알아보자.

jsx를 어떻게 리액트에 적용할 수 있을까?

<br/>

### a) What is JSX?

: 자바스크립트 xml의 약자이다.

: html, css와 구조가 유사하다.

: 태그 이름은 기존 HTML과 비슷하다.

: Attributes나 css는 낙타방식이다.(ex. className, textAlign)

: js코드를 활용하고 싶다면 `{ }`를 이용한다.

<br/>

### b) start

: App.js파일의 코드를 건드려보자.

> App.js

: 컴포넌트안에 적힌 코드 형식은 모두 JSX이다.

: html에서 h1태그에 스타일을 적용할 때, inline방법으로 `<h1 style="color: red;">`를 했었는데 JSX에서는 불가하다.

: inline방식으로 스타일을 적용하고자 할 때는 `<h1 style={ }>`을 하여 `{}`안에 코드를 작성해줘야한다.

: 첫번째 괄호는 "여기에 자바스크립트 코드가 들어갈거야"라는 의미

: 두번째 괄호는 "그런데 여기에 넣을 코드는 자바스크립트의 딕셔너리 형태로 쓸거야" 의미

```jsx
import React from 'react';
import logo from './logo.svg';
import './App.css';

function App() {
  return (
		<div>
			<h1 style={{color : 'red'}}>앍</h1>
		</div>
  );
}

export default App;
```

![CLASSLION%20react%20props%20cd615825d72641e9ac1ab1f2083f9dc3/_2019-08-19__3.36.26.png](CLASSLION%20react%20props%20cd615825d72641e9ac1ab1f2083f9dc3/_2019-08-19__3.36.26.png)

: 아니면 아래 방법과 같이 딕셔너리 형태의 자바스크립트 코드를 변수에 넣고, 이 변수를 집어넣어도 된다.

```jsx
import React from 'react';
import logo from './logo.svg';
import './App.css';

const myStyle = {color: 'red'}
function App() {
  return (
		<div>
			<h1 style={myStyle}>앍</h1>
		</div>
  );
}

export default App;
```

: 그런데 myStyle에 font-weight가 안된다? 쀼슝빠슝:>

이 경우는 JSX의 문법 중 하나인 낙타형식을 적용하지 않아서이다. JSX문법에서는 하이폰(-)을 허락하지 않기 때문에 `font-weight`는 낙타형식인 `fontWeight`라고 해야한다.

(이때 10px이라고 적으면 적용이 안되므로 그냥 숫자만 적을 것!)

<br/>

> App.css

: 인라인 형식으로 우리는 h1태그에 스타일을 적용했다. 하지만 이런 방식으로 스타일 코드를 쓰면 코드의 규모가 커졌을 때 상~당히 더러워진다. 이 경우를 방지하기 위해 스타일을 클래스로 선언해보자.

```jsx
//App.js
import React from 'react';
import logo from './logo.svg';
import './App.css';

function App() {
  return (
		<div>
			<h1 className={'myStyle'} >앍</h1>
		</div>
  );
}

export default App;
```

: `'myStyle'`이라는 클래스는 App.css에서 관리하게 된다.

```css
/* App.css */

.myStyle {
	color: red;
	font-weight : 100;
}
```
