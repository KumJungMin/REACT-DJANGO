# CLASSLION: Rest, Serializers

## 1. rest?

---

: http를 이용해 통신하는 네트워크상에서 정한 약속

: 분산 하이퍼미디어 시스템을 위한 소프트웨어 설계형식

: 자원을 이름으로 구분하여 상태를 전송하는 방법

ex) get/1을 하면 1을 get해준다와 같음

## 2. rest는 왜 필요한가요?

---

: 하위 호환을 유지하면서 각기 네트워크가 발전되게 해준다.

: 웹의 독립적인 진화에 도움이 됨

## 3. rest 설계 조건(rest가 되기 위한 필요충분조건)

---

: 서버와 클라이언트가 존재해야

: 클라이언트의 이전 상태를 저장하지 않는 연결방식 (ex. http)

: 다층 구조, 캐싱가능

## 4. api의 역할

---

![CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e/_2019-08-09__1.06.14.png](CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e/_2019-08-09__1.06.14.png)

: 서버와 클라이언트 사이의 메신저

: 만든 api를 이런 식으로, 이런 형식을 지켜서 쓰세요! 와 같은 api문서들이 존재한다.

## 5. rest api

---

: rest 아키텍처 스타일을 따르는 api

: http(get, post)로 crud를 구현할 수 있는 api

: http기반의 api는 json으로 동작한다.

: json은 만든 이가 정의하기 나름이라서 api문서가 존재

![CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e/_2019-08-09__1.18.25.png](CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e/_2019-08-09__1.18.25.png)

: 클라이언트와 장고레스트 프레임워크는 json문자열이 이동한다.

: is_valid()함수를 통해서 유효성 검사를 하고 데이터를 저장.

## 6. 직렬화

---

: api에서는 문자열의 데이터가 왔다갔다함.

![CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e/_2019-08-09__1.22.04.png](CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e/_2019-08-09__1.22.04.png)

: 클라이언트가 처리할 정보가 있다면 → 문자열화 시켜 → 처리하는 방식

![CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e/_2019-08-09__1.23.02.png](CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e/_2019-08-09__1.23.02.png)

### - ModelSerializer

: 직렬화의 기능을 모델에 맞게 된 것이 ModelSerializer이다.

: 쿼리셋이나 모델 직렬화를 알아서 해준다.

## 7. 실습

---

### 1). 장고의 기본적 설정

-가상환경 설치하기

```bash
python3 -m venv myvenv
```

-가상환경 키고 필요한 모듈 설치하기

```bash
source myvenv/bin/activate
pip install djangorestframework
```

-프로젝트 파일과 앱 생성하기

```bash
django-admin startproject firstrest
cd firstrest
python3 manage.py startapp post
```

-firstrest/settings.py의 설치된 앱 목록에서, rest_framework, app 등록하기

![CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e/_2019-08-14__10.21.20.png](CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e/_2019-08-14__10.21.20.png)

-앱폴더 안에 [urls.py](http://urls.py) 생성

: url을 계층적으로 관리하기 위해 include를 사용한다.

: firstrest/urls.py에 include를 이용하여 apps의 urls를 연결시킨다.

```python
from django.contrib import admin
from django.urls import path, include
#url을 계층적으로 관리가 가능
import post.urls
#post앱 안에 만들어준 urls를 import한다.
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('post.urls')),
]
```

-post/models.py에 가서 데이터를 정의한다.

: 아래와 같이 코드를 정의한 후 2가지 명령어를 쉘에 입력해준다.

`python3 [manage.py](http://manage.py) makemigrations`

`python3 [manage.py](http://manage.py) migrate`

```python
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length = 100)
    body = models.TextField()
```

-post앱에 [serializers.py](http://serializers.py) 생성하기

: 모델 데이터를 직렬화하는 코드를 작성한다.

```python
#모델을 직렬화
from .models import Post
from rest_framework import serializers

class PostSerializer(serializers.ModelSerializer):
    class Meta:
        model = Post #post를 기반으로 직렬화 시킬거야
        fields = '__all__' #모든 필드를 직렬화 시킬거야
        #fields = ['title', 'body']
```

-post/views.py에서 cbv형태로 뷰셋을 생성하기

: 뷰셋에 대한 설명은 다음 강의에서 진행된다.

```python
from django.shortcuts import render
from .models import Post
from .serializers import PostSerializer
from rest_framework import viewsets

#cbv
class PostViewset(viewsets.ModelViewSet):
    quertset = Post.objects.all() #모든 객체를 끌어다 오겠다.
    serializer_class = PostSerializer
```

-post/urls.py에서 라우터를 등록하기

: rest framework는 router를 이용하여 url를 결정한다.

```python
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from . import views
#라우터 개념을 통해 url을 결정한다.

router = DefaultRouter()
router.register('post', views.PostViewset)
#api상에서 postViewset은 crud를 가능하게 해줌

urlpatterns = [
    path('', include(router.urls)), #실제 url을 결정
]
```

-서버 구동 시켜보기

: 지금까지 작성한 코드를 토대로 서버를 구동시켜보자!

`python3 [manage.py](http://manage.py) runserver`

: 8000번의 api서버가 구동된다.

: 우리는 urls.py에서 DefaultRouter를 정의했는데 이 디폴트 라우터가 우리를 기본적으로 이동시킨 곳이 api root이다.

![CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e/_2019-08-20__10.12.14.png](CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e/_2019-08-20__10.12.14.png)

: 우리가 api 서버를 만들건데 이 api서버 구축에 있어 가장 기본이 되는 곳이기도 하다.

: 오른쪽 상단에 있는 (파란색버튼)GET은 우리가 get요청을 했을 경우 뜨게되는 내용이라는 말이다.

: 우리는 127.0.0.1:8000포트에 get요청을 보내면 이 화면으로 응답한다.

```bash
#우리가 앞서 httpd를 깔았으므로 이 api에 요청을 보내면 응답이 오는 지 확인(터미널에서 진행)
http GET http://127.0.0.1:8000/
```

: 위와 같이 터미널에 해당 ip주소로 get요청을 보내면 API ROOT에서 보이던 내용들이 응답해준다.("서버에 접속이 되었당! :>")

![CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e/_2019-08-20__10.17.55.png](CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e/_2019-08-20__10.17.55.png)

: `Allow`는 우리가 이 주소에서 요청할 수 있는 항목들이다.

: `content-type`은 json형식으로 데이터가 이동한다.

: `"post" : "http://127.0.0.1:8000/post/"`는 우리가 사용할 수 있는 api 목록들이다.

: 제일 위에  있는 `django REST framework`를 클릭하여 장고 레스트 프레임워크 공식 홈페이지로 이동한다. 

: options는 이 데이터들이 어떤 형식(integer)으로 되어 있는지에 대한 상세 내역을 보여준다.

: [http://127.0.0.1:8000/post에](http://127.0.0.1:8000/post에) get방식으로 요청을 보내보자사진과 같이 뜨며, 홈페이지에서 보이는 `[]`은 현재 post List에 어떠한 데이터도 등록되어 있지 않아서 비어있는 상태 

![CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e/_2019-08-20__10.25.13.png](CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e/_2019-08-20__10.25.13.png)

: 이번에는 아까와 다르게 `Allow`에 post 방식의 요청이 가능하다고 적혀있다. 이 말은 사진하단에 보이는 `TITLE, BODY`에 내용을 입력해서 데이터를 처리해달라는 공간이 생겼다는 말이다.

: 만약 아래 빈칸에 내용을 넣고 제출을 하면, "이 url에 대해 post요청을 보낸다"와 같다. 결과적으로 이 url의 []에, 방금전 입력한 데이터가 json형식으로 post요청되었다는 걸 알 수 있다.

![CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e/_2019-08-20__10.29.56.png](CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e/_2019-08-20__10.29.56.png)

: 데이터전송을 해당 url페이지가 아닌 터미널상에서 json형식으로 post요청을 해볼 수 있다.

![CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e/_2019-08-20__10.31.51.png](CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e/_2019-08-20__10.31.51.png)

: 이렇게 우리는 title, body 데이터를 json형식으로 post요청을 했다. 앞서 우리는 2개의 데이터객체를 전송했다. Post List를 예로 들자면 게시판의 글목록과 같은데, 그렇다면 이 각각의 데이터는 각각의 detail페이지와 같다. 

: 우리가 url에 1 혹은 2를 쳐서 접속하여 각각의 detail 페이지에 접속가능하다.

: 이 페이지는 아까와 다르게 put, patch, delete가 가능하다.

![CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e/_2019-08-20__10.36.19.png](CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e/_2019-08-20__10.36.19.png)

: 그런데 이 각각의 데이터 객체를 식별가능하게 하기 위해서는 어떻게 해야할까? 

```python
#모델을 직렬화
from .models import Post
from rest_framework import serializers

class PostSerializer(serializers.ModelSerializer):
    class Meta:
        model = Post #post를 기반으로 직렬화 시킬거야
        fields = ['id', 'title', 'body']
```

: pk를 해주면 좋을 거 같다.  serializers.py에 들어가서 fields에 id를 추가해보자! 그러면 각각의 데이터에 대해 고유한 번호가 부여된다. 이렇게 부여된 id값은 `/post/id` 형식으로 접근이 가능하게 한다.

(이때 fields = '__all__'을 하면 굳이 id를 추가하지 않아도 자동 추가됨)