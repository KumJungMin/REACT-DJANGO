# CLASSLION:Router

# 6. Router

---

지금까지 우리는 urlpattern을 이용하여 url설정을 했다. 그렇다면 이 urlpattern을 router방식으로 작성하기 위해선 어떻게 해야할까?

<br/>

### 하나의 path함수 만으로 모든 뷰셋의 CRUD를 처리할 수 있을까?

: 하나의 뷰 클래스에서는 여러가지 액션에 대한 응답을 할 수 있었다. 그렇다면 하나의 path함수 만으로 모든 뷰셋의 CRUD를 처리할 수 있을까?

: NO! 필연적으로 두 개이상의 path함수가 생긴다.

: 그렇다면 이제 우리가 할 일은 이 여러개의 path함수를 router를 이용하여 하나로 묶는 것!

![CLASSLION%20Router%20f7f68d9d466948d498872fca76ebc1da/_2019-08-22__2.44.35.png](CLASSLION%20Router%20f7f68d9d466948d498872fca76ebc1da/_2019-08-22__2.44.35.png)

<br/>

### 그럼 path함수를 묶어준다는 건 무슨 의미일까? 

: path 함수로 묶는다는 건, path함수의 2번째 인자를 묶어주고 매핑해주는 것이다.

: path함수의 2번째 인자로 오는 함수들은 `list(), create(), retrieve(), update(), partial_update(), destroy`가 있으며 이들을 묶어주는 게 중요하다.

![CLASSLION%20Router%20f7f68d9d466948d498872fca76ebc1da/_2019-08-22__2.45.50.png](CLASSLION%20Router%20f7f68d9d466948d498872fca76ebc1da/_2019-08-22__2.45.50.png)

<br/>

### path함수를 뭐로 묶어주나요?

아니;; 묶어줘야하는 건 알겠고 또 왜 묶는지도 알겠는데, 뭐로 묶어주냐거;;

: path()함수를 묶어주는 건 `.as_view()`가 수행한다. :>

: `as_view()`는 딕셔너리 형식의 데이터를 받는데, `http_method`로 처리할 함수를 이 형식으로 작성한다. 

: (get요청이 들어오면 retrieve함수를 이용해서 이 요청을 처리할거야!)

![CLASSLION%20Router%20f7f68d9d466948d498872fca76ebc1da/_2019-08-22__2.52.08.png](CLASSLION%20Router%20f7f68d9d466948d498872fca76ebc1da/_2019-08-22__2.52.08.png)

: 만약 이 as_view함수의 이 과정을 mypath변수에 담아준다면, 이 url과 함수 → 이 매핑의 결과가 mypath라는 변수에 담기게 된다. 

그리고 매핑결과를 가진 변수 mypath를 path함수의 2번째 인자로 넣어주면 된다.(2번째 인자는 request요청에 따른 함수를 호출하는 곳)

![CLASSLION%20Router%20f7f68d9d466948d498872fca76ebc1da/_2019-08-22__2.55.33.png](CLASSLION%20Router%20f7f68d9d466948d498872fca76ebc1da/_2019-08-22__2.55.33.png)

<br/>

*편한 방법 알려준다고 했는데, 그럼 매번 저렇게 http요청에 해당하는 함수를 쓰는 것도 번거롭지 않남?*

: 그래서 등장하게 된 것이 `Router!!` ( 매핑관계를 자동으로 구현해줌 )

: `DefaultRouter`는 앞서 작성한 관례적인 매핑관계를 자동으로 해주는데, 이 디폴트 라우터의 `register`를 이용해서 뷰셋을 등록한다. 

`register(path함수의 첫번째 인자 같은 느낌, 매핑관게를 구현해줄 수 있는 뷰셋)`

```python
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from snippets import views

#router를 만들고, 뷰셋을 라우터에 등록한다.
router = DefaultRouter()
router.register(r'snippets', views.SnippetViewSet)
#http:0.0.0.0:8000/snippets라고 url을 설계하겠다.
router.register(r'user', views.UserViewSet)

urlpatterns=[
	path('', include(router.urls)),
#라우터에 등록된 url을 적어주기만 하면 됨
]
```

<br/><br/><br/>


### 장고 rest-api

[1]  <a href="https://github.com/KumJungMin/REACT-DJANGO/blob/master/CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df.md">JSON이란?</a>

[2] <a href="https://github.com/KumJungMin/REACT-DJANGO/blob/master/Export-3b5f2385-ed14-4878-9265-e1ab00dd6924/CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e.md">Rest, Serializers</a>

[3] <a href="https://github.com/KumJungMin/REACT-DJANGO/blob/master/Export-c4044b0d-ecb9-486f-9447-ddf8c5f65f64/CLASSLION%20View%20APIView%20f672defce2e34d84ada0b24cffba799c.md">View & APIView</a>

[4] <a href="https://github.com/KumJungMin/REACT-DJANGO/blob/master/Export-5c01d905-91b6-43ed-b457-d31b6252a625/CLASSLION%20Mixins%20generic%20CBV%20f8b2f7c2548b4f1997b56f9829777e45.md">Mixins & generic CBV</a>

[5] <a href="https://github.com/KumJungMin/REACT-DJANGO/blob/master/Export-ac5377dc-bf69-4da5-b2b0-5d187fc32697/CLASSLION%20ViewSet%20af35b5ecbc044947a33594edc5d3a4b0.md">ViewSet</a>

[6] <a href="https://github.com/KumJungMin/REACT-DJANGO/blob/master/Export-af386b95-5b40-4ba6-ab11-37653db27114/CLASSLION%20Router%20f7f68d9d466948d498872fca76ebc1da.md">Router</a>
