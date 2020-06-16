# CLASSLION: JSON?

# 장고 강의 심화(1)

## 1) json이란 무엇인가? (자바스크립트 객체 notation)

: 데이터의 송수신을 자바스크립트 객체로 수행할 수 있게 하는 표현식

: json을 이용하여 데이터를 송수신하여, rest api와 데이터 교환을 한다.

: 예전에는 json대신에 xml을 쓰기도 했는데, xml은 다양한 데이터를 표현할 수 있지만 크기가 작아서 현재는 사용이 미비하다.

## 2) 어떻게 구성되어있는지

- restful api server를 생성되어 있다.
- 사용자의 요청에 따른 json 응답을 한다.

![CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.05.25.png](CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.05.25.png)

![CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.06.49.png](CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.06.49.png)

: 오른쪽 사진은 우리가 일반적으로 쓰던 장고의 딕셔너리 문법 형식이다.

: 이러한 장고의 딕셔너리 문법을 json문법으로 변경하여 사용한다.

## 3) json의 문법형태

: json의 문법은 아래와 같다(자바스크립트와 유사하지만, 모두 문자열 형태)

: 자바스크립트 객체를 문자열의 형태로 변환하여 이것이 json문법 상태가 된다.(직렬화, 아래에 설명)

- 직렬화

    JSON은 자바스크립트 객체 → 문자열로 변환하거나 반대로← 변환하는 작업을 진행하는데 매우 경량화된 방법이다. 

    이러한 직렬화 과정을 통해 서로 각기 다른 언어들을 모두가 알아 들을 수 있는 언어의 형태로 변환한다.

![CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.16.11.png](CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.16.11.png)

## 4) json을 왜 쓰는지

![CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.10.32.png](CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.10.32.png)

: 사용자의 요청에 따른 데이터의 응답을 자바스크립트 객체가 아닌 json으로 응답하는 이유는?

→ 모든 수신자가 자바스크립트 객체 표현식을 아는 게 아니기 때문에 좀 더 쉬운 json문법으로 송수신해준다.

→자바스크립트 문법(객체) 대신 만국 공통 자료형(문자열)을 통신하자!

---

그래서 json의 동작은 아래 그림과 같다.

![CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.17.33.png](CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.17.33.png)

사용자가 restful api 서버에 "객체→문자열"로 보내면

사용자는  "문자열→객체"로 바꿔받는다.

## 5) 간단한 문법

```python
import json
#diary에 있는 내용을 json으로 보내려면
#문자열의형태(json형식)로 만들어야함

diary = {
    'id' : 3,
    'title':'titititit'
}

diary_s = json.dumps(diary)
# 딕셔너리 등 형태의 자료형을 json에 보낼 수 있는
# 문자열형태로 변환해준다.
print(type(diary_s)) #자료형이 str형태

diary_back = json.loads(diary_s)
# json형식의 데이터를 파이썬 형식으로 변환(딕셔너리 등)
```

`import json` json문법으로 변환하기 위해, json을 import 한다.

`json.dumps()` 장고의 자료형을 json의 문자열 형태로 변환해준다.

`json.loads()` json형식의 문자열을 장고의 자료형으로 변화해준다.

---

---

# 2. http 프로토콜과 json

## 1) http란?

: 통신을 주고 받을 수 있는 주체는 서버와 클라어언트이다.

: 웹상에서 통신을 수행할 수 있게 해주는 통신규약을 http라고 한다.

## 2) 프로토콜 종류

: 장고에서 지원하는 프로토콜은 아래와 같다.

[장고 프로토콜](https://www.notion.so/e5653072451542a2b24b4bfde64eb1a7)

---

: restful framework에서는 아래의 프로토콜을 쓴다.

![CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.38.13.png](CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.38.13.png)

[restful 프로토콜](https://www.notion.so/79343b23b37447a89aa22ef8d945f640)

---

: 사용자가 json문법 형식으로 보낸 데이터를 서버는 아래와 같은 응답으로 답한다.

[http response](https://www.notion.so/70f0627d4ea7444a92e1b35785171e0c)

## 3) httpie?

: command line으로 동작하는 http 클라이언트?

: command line에서 http 요청을 보냈을 때 어떤 결과가 나오는지 보여준다.

: api  view를 호출하고 응답하는 역할을 한다.

- 설치법

    pip install —upgrade httpie

### (1). 명령어

 : http키워드로 시작한다.

```python
http  [flags]  [method]  URL  [item[item]]
```

`[flags]`: 어떤 형식으로 보내겠다는 옵션 (json, http형식으로 보내겠다 등등)

`[method]` : http method

`URL` : 요청이나 응답의 대상이 되는 주소

`[item[item]]` : 인자 (어떤 값을 처리할 때 이 인자 안에 그 값을 넣어 처리)

→ 처리 요청방식에 put, post이면 =으로 표현  ( x = a )

→ 처리 요청방식이 get         이면 ==으로 표현(x == a)

- get 방식(데이터를 요청하겠다!)

```python
http  (get) URL   인자 == 값
http example.org  x == 1 y ==2
```

- put 방식(수정하겠다!)(값을 넣는 것이므로 =)

```python
http  put  URL   get인자==값   put인자=값
#get인자가 있으면 쓰고 없으면 안쓴다.
#put인자가 있으면 쓰고 없으면 안쓴다.
```

- post방식(생성하겠다!) + json, html form 형식 요청

```python
#json형식의 요청
http --json POST URL get인자==값 post인자=값

#html form형식의 요청
http --form POST URL get인자==값 post인자=값
#get인자
```

post인자가 있으면 쓰고 안쓰며, post인지가 있으면 쓰고 없으면 안쓴다.

(2). 실습

- 터미널에서 httpie를 설치한다.

![CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.54.17.png](CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.54.17.png)

---

- get방식으로 이미 있는 주소에 요청을 보내본다.(현재 url의 내용을 보여줘라)

    : 요청에 성공하게 되면 아래와 같이 헤더에 200이 뜬다.

![CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.56.23.png](CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.56.23.png)

![CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.57.05.png](CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.57.05.png)

![CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.57.48.png](CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.57.48.png)

---

- 다른 별도의 정보 말고 응답헤더 정보만 보고싶을 때는 `http —headers "URL"`

    ![CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.59.33.png](CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.59.33.png)

---

- 장고 서버(8000번 포트)에 대한 http 요청을 보내고 싶을 때는 `http :8000`을 한다.

![CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__2.01.24.png](CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__2.01.24.png)

---

- 현재 페이지는 글 목록을 보는 페이지로 여기서 post요청을 하면 "추가"라는 글작성 요청이 응답한다.

    : `http —form post URL title='new' body='new'`

    : 하지만 인자의 내용을 작성하라고 요청해도, 장고의 보안(csrf토큰)때문에 요청이 폐기된다.

![CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__2.04.28.png](CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__2.04.28.png)

![CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__2.09.02.png](CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__2.09.02.png)

---

- `httpbin.org/method_name` httpbin주소는 http요청을 테스트하기 위한 서버이다.

![CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__2.11.52.png](CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__2.11.52.png)

![CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__2.14.03.png](CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__2.14.03.png)

---

- json으로 post요청을 하면 요청한 내용에서 data에 문자열의 형태로 나타난다.

![CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__2.15.07.png](CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__2.15.07.png)

![CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__2.15.16.png](CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__2.15.16.png)

[Copy of CLASSLION: cbv(1)](https://www.notion.so/Copy-of-CLASSLION-cbv-1-f3417a3bab21474fb33875b8f46c9a04)

[Copy of CLASSLION: Rest, Serializers](https://www.notion.so/Copy-of-CLASSLION-Rest-Serializers-67195661885b4a1eb8d09af694a70d72)

[Copy of CLASSLION:View & APIView](https://www.notion.so/Copy-of-CLASSLION-View-APIView-aedf4fcc23d14d7ab698d37d60c52e30)

[Copy of CLASSLION:Mixins & generic CBV](https://www.notion.so/Copy-of-CLASSLION-Mixins-generic-CBV-daf1bf64c61845aa8e00b967dac2e566)

[Copy of CLASSLION:ViewSet](https://www.notion.so/Copy-of-CLASSLION-ViewSet-202883770ae7457fb22b3a162688cc01)

[Copy of CLASSLION:Router](https://www.notion.so/Copy-of-CLASSLION-Router-ad1be08fbf3c42c292fb1621a78652c6)