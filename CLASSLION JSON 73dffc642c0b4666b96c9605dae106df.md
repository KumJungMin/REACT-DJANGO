# CLASSLION: JSON?

# 장고 강의 심화(1)

## 1) json이란 무엇인가? (자바스크립트 객체 notation)

: 데이터의 송수신을 자바스크립트 객체로 수행할 수 있게 하는 표현식

: json을 이용하여 데이터를 송수신하여, rest api와 데이터 교환을 한다.

: 예전에는 json대신에 xml을 쓰기도 했는데, xml은 다양한 데이터를 표현할 수 있지만 크기가 작아서 현재는 사용이 미비하다.

<br/>

## 2) 어떻게 구성되어있는지

- restful api server를 생성되어 있다.
- 사용자의 요청에 따른 json 응답을 한다.

![CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.05.25.png](CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.05.25.png)

![CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.06.49.png](CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.06.49.png)

: 오른쪽 사진은 우리가 일반적으로 쓰던 장고의 딕셔너리 문법 형식이다.

: 이러한 장고의 딕셔너리 문법을 json문법으로 변경하여 사용한다.

<br/>

## 3) json의 문법형태

: json의 문법은 아래와 같다(자바스크립트와 유사하지만, 모두 문자열 형태)

: 자바스크립트 객체를 문자열의 형태로 변환하여 이것이 json문법 상태가 된다.(직렬화, 아래에 설명)

- 직렬화

    JSON은 자바스크립트 객체 → 문자열로 변환하거나 반대로← 변환하는 작업을 진행하는데 매우 경량화된 방법이다. 

    이러한 직렬화 과정을 통해 서로 각기 다른 언어들을 모두가 알아 들을 수 있는 언어의 형태로 변환한다.

![CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.16.11.png](CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.16.11.png)

<br/>

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

<br/>

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
<br/>

# 2. http 프로토콜과 json

## 1) http란?

: 통신을 주고 받을 수 있는 주체는 서버와 클라어언트이다.

: 웹상에서 통신을 수행할 수 있게 해주는 통신규약을 http라고 한다.

<br/>

## 2) 프로토콜 종류

: 장고에서 지원하는 프로토콜은 아래와 같다.

| get | post |
|:--------:|:--------:|
| 데이터를 가져다줘 | 데이터를 처리해줘 |
| url에 입력한 데이터의 항목이 뜸 | url에 입력한 데이터의 항목이 안 뜸 |

---

: restful framework에서는 아래의 프로토콜을 쓴다.

![CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.38.13.png](CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df/_2019-07-16__1.38.13.png)

| get | 요청받은 URL의 정보를 검색하여 응답해준다.(find) |
|:--------:|:--------:|
| post | 요청한 자원을 생성한다.(create) |
| put | 요청된 자원을 수정한다.(update) |
| delete | 요청된 자원을 삭제한다. |
| patch | 요청한 자원의 일부를 수정 및 교체한다.(일부만!) |
| option | 웹서버에서 지원되는 메소드의 종류를 확인할 수 있다. |





---

: 사용자가 json문법 형식으로 보낸 데이터를 서버는 아래와 같은 응답으로 답한다.

| 1XX(정보) | 요청을 받았으며 프로세스를 계속한다. |
|:--------:|:--------:|
| 2XX(성공) | 요청을 성공적으로 받았으며 인식했고 수용하겠다. |
| 3XX(리다이렉션) | 요청 완료를 위해 추가 작업 조치가 필요하다. |
| 4XX(클라이언트 오류) | 요청의 문법이 잘못되었거나 요청을 처리할 수 없다. |
| 5XX(서버 오류) | 서버가 명백히 유효한 요청에 대해 충족을 실패했다. |

<br/>

## 3) httpie?

: command line으로 동작하는 http 클라이언트?

: command line에서 http 요청을 보냈을 때 어떤 결과가 나오는지 보여준다.

: api  view를 호출하고 응답하는 역할을 한다.

- 설치법

    pip install —upgrade httpie

<br/>

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

<br/>

### (2). 실습

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


<br/><br/><br/>


### 장고 rest-api

[1]  <a href="https://github.com/KumJungMin/REACT-DJANGO/blob/master/CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df.md">JSON이란?</a>

[2] <a href="https://github.com/KumJungMin/REACT-DJANGO/blob/master/Export-3b5f2385-ed14-4878-9265-e1ab00dd6924/CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e.md">Rest, Serializers</a>

[3] <a href="https://github.com/KumJungMin/REACT-DJANGO/blob/master/Export-c4044b0d-ecb9-486f-9447-ddf8c5f65f64/CLASSLION%20View%20APIView%20f672defce2e34d84ada0b24cffba799c.md">View & APIView</a>

[4] <a href="https://github.com/KumJungMin/REACT-DJANGO/blob/master/Export-5c01d905-91b6-43ed-b457-d31b6252a625/CLASSLION%20Mixins%20generic%20CBV%20f8b2f7c2548b4f1997b56f9829777e45.md">Mixins & generic CBV</a>

[5] <a href="https://github.com/KumJungMin/REACT-DJANGO/blob/master/Export-ac5377dc-bf69-4da5-b2b0-5d187fc32697/CLASSLION%20ViewSet%20af35b5ecbc044947a33594edc5d3a4b0.md">ViewSet</a>

[6] <a href="https://github.com/KumJungMin/REACT-DJANGO/blob/master/Export-af386b95-5b40-4ba6-ab11-37653db27114/CLASSLION%20Router%20f7f68d9d466948d498872fca76ebc1da.md">Router</a>
