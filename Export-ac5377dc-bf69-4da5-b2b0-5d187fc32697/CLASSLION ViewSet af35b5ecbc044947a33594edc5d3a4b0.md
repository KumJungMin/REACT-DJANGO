# CLASSLION:ViewSet

: 공식문서에 있는 viewset을 보기에 앞서, viewsets.py에 정의된 뷰 클래스를 보자.

: 이 뷰 클래스는 총 4가지(Generic, ReadOnly, Model, ViewSet)가 있다.

<br/>

# 5. ViewSet

---

## 1) viewsets.py의 코드보기

### (1) 소개

: viewsets.py에 있는 클래스들은 인자를 묶어주는 역할을 수행한다.

: 우리는 여기서 ReadOnlyModelViewSet, ModelViewSet을 다룰 것이다.

```python
class ViewSet(ViewSetMixin, Views.APIView):
	pass
"""이 뷰셋은 아무런 액션, 아무런 동작을 하지 않는다."""
```

```python
class GenericViewSet(ViewSetMixin, generics.GenericAPIView):
```

<br/>

### (2) ReadOnlyModelViewSet

: `list(), retrieve()`메소드를 묶어준다.
: retrieve()은 특정 객체를 가져오며, `list()`는 객체 리스트를 가져와주는 역할을 한다.
: 특정 레코드를 읽는 역할을 수행하는 클래스이다.

```python
class ReadOnlyModelViewSet(mixins.RetrieveModelMixin, mixins.ListModelMixin, GenericViewSet):
```

```python
class ModelViewSet(mixins.CreateModelMixin, mixins.RetrieveModelMixin, mixins.UpdateModelMixin, mixins.DestroyModelMixin)
```

<br/>

## 2) viewset코드 작성하기

: 우리는 이제까지 List, Detail을 각각의 클래스로 정의했다.

: 하지만 이제는 하나의 클래스(뷰셋)에서 이 2가지를 구현할 수 있다. 

: 위에서 보이는 코드처럼 이번에는 List, detail을 인자로 상속받아 하나의 클래스에서 진행가능하다.

<br/>

### (1) ReadOnlyModelViewset

: `ReadOnlyModelViewset`는 viewsets.py에 있는 클래스이다. 이 클래스를 상속받아, 쿼리셋과 직렬화클래스 변수를 재정의하는데, 이것이 가능한 이유가 `ReadOnlyModelViewSet`이 GenericViewSet을 상속받기 때문이다.

```python
from rest_framework import viewsets

class UserViewSet(viewsets.ReadOnlyModelViewset):
#retrieve, list기능을 함.
	queryset = User.object.all()
	serializer_class = UserSerializer
```

<br/>

### (2) ModelViewSet

: queryset, serializer_vlass변수에 다룰 모델과 직렬화 객체를 설정해준다.

: permission_classes는 이 액션을 수행할 수 있는 권한을 설정하는데, 소유자가 아닌 사람들은 읽기만 가능하게 한다.

```python
|from rest_framework.decorators import action
|from rest_framework.response import Response
|
|class SnippetViewSet(viewsets.ModelViewSet): #create,update,delete
|	queryset = Snippet.objects.all()            #다룰 모델, 직렬화 설정
|	serializer_class = SnippetSerializer
|	permission_classes = [permissions.IsAuthenticatedOrReadOnly, IsOwnerOrReadOnly] 
|#이 액션을 수행할 수 있는 권한 설정
```

<br /> <br />

*아니 뷰셋을 이용해서 코드길이가 반이 되는 건 알겠는데, 이 방법은 CRUD만 구현할 수 있나? 나만의 Custom API는 어떻게 만듬?*
<br />
### (3) 나만의 API

`@ + 함수들 → 데코레이터, 장식자를 이용하자!`

: 데코레이터를 써주고, 빨간 네모 안에 CRUD가 아닌, 내가 커스텀한 api를 코드를 써서 response가 가능하게 만든다.

: renderer_classes은 response를 어떤 방식으로 랜더링 시킬 것인가를 정한다. 

: 예를 들어renderer.JSONRenderer라고 하면 데이터를 json방식으로 보내겠다는 말이 된다.

![CLASSLION%20ViewSet%20af35b5ecbc044947a33594edc5d3a4b0/_2019-08-22__1.52.43.png](CLASSLION%20ViewSet%20af35b5ecbc044947a33594edc5d3a4b0/_2019-08-22__1.52.43.png)

![CLASSLION%20ViewSet%20af35b5ecbc044947a33594edc5d3a4b0/_2019-08-22__1.56.23.png](CLASSLION%20ViewSet%20af35b5ecbc044947a33594edc5d3a4b0/_2019-08-22__1.56.23.png)

```python
|	@action(detail=True, renderer_classes=[renderers.StaticHTMLRenderer])
|	def highlight(self, request, *args, **kwargs):
|		snippet = self.get_object()
|		return Response(snippet.highlighted)
|	
|	def perform_create(self, serializer):
|		serializer.save(owner=self.request.user)
```

<br />

*그래서 말한대로 customAPI를 만들었는데, 얘는 어떤 method로 호출해야 처리되는 거지? get으로 해야함? 아님 post로 해야함??;;*

<br />

: Custom API의 default Method은 get방식이다.

 하지만 원한다면, action의 format인자로 다른 method지정이 가능하다.


<br/>

### (4) ReadOnlyViewSet방법으로 [views.py] 작성해보기

```python
|from post.models import Post
|from post.serializer import PostSerializer
|from rest_framework import viewsets
|
|#ReadOnlyModelViewSet은 말그대로 ListView, DetailView의 조회만 가능
|class PostViewSet(viewsets.ReadOnlyViewSet):
|  queryset = Post.objects.all()
|  serializer_class = PostSerializer
```

```python
#urls.py
|from rest_framework.routers import DefaultRouter
|from django.urls import include, path
|from post import views
|
|router = DefaultRouter()
|router.register('post', views.PostViewSet)
|
|urlpattern = [
|  path('', include(router.urls))
|]
```

: runserver를 해서 페이지에 들어가보면, 리스트 페이지와 디테일 페이지에서 get요청만 가능하고 수정, 삭제 요청은 불가하다는 걸 볼 수 있다.

![CLASSLION%20ViewSet%20af35b5ecbc044947a33594edc5d3a4b0/_2019-08-22__2.16.34.png](CLASSLION%20ViewSet%20af35b5ecbc044947a33594edc5d3a4b0/_2019-08-22__2.16.34.png)

![CLASSLION%20ViewSet%20af35b5ecbc044947a33594edc5d3a4b0/_2019-08-22__2.16.44.png](CLASSLION%20ViewSet%20af35b5ecbc044947a33594edc5d3a4b0/_2019-08-22__2.16.44.png)

<br/>

### (5) ModelViewSet방법으로 [views.py] 작성해보기

```python
|from post.models import Post
|from post.serializer import PostSerializer
|from rest_framework import viewsets
|
|#@action처리
|from rest_framework import renderers
|from rest_framework.decorators import action
|from django.http import HttpResponse
|
|class PostViewSet(viewsets.ModelViewSet): #crud가능
|  queryset = Post.objects.all()
|  serializer_class = PostSerializer
|
|  #@action(method=['post']) 이렇게 하면 default method 변경 가능
|  @action(detail=True, renderer_classes=[renderers.StaticHTMLRenderer])
|  def highlight(self, request,*args, **kwargs):
|    return HttpResponse("얍")
|    #그냥 얍!을 띄우는 커스텀 api
```

: 이제 runserver를 하고 디테일페이지로 가보면 [오른쪽 상단]에 [Extra Actions]→[Highlight]가 보인다.(아까 만든 커스텀 api)

![CLASSLION%20ViewSet%20af35b5ecbc044947a33594edc5d3a4b0/_2019-08-22__2.26.28.png](CLASSLION%20ViewSet%20af35b5ecbc044947a33594edc5d3a4b0/_2019-08-22__2.26.28.png)

: 그리고 버튼을 눌러보면 커스텀 액션인 "얍!"이 뜨는 걸 확인할 수 있다.

: url을 보면 `post/2/highlight/` → `post/pk/메소드명`으로 뜨는 걸 알 수 있다.

![CLASSLION%20ViewSet%20af35b5ecbc044947a33594edc5d3a4b0/_2019-08-22__2.27.48.png](CLASSLION%20ViewSet%20af35b5ecbc044947a33594edc5d3a4b0/_2019-08-22__2.27.48.png)

<br/><br/><br/>


### 장고 rest-api

[1]  <a href="https://github.com/KumJungMin/REACT-DJANGO/blob/master/CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df.md">JSON이란?</a>

[2] <a href="https://github.com/KumJungMin/REACT-DJANGO/blob/master/Export-3b5f2385-ed14-4878-9265-e1ab00dd6924/CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e.md">Rest, Serializers</a>

[3] <a href="https://github.com/KumJungMin/REACT-DJANGO/blob/master/Export-c4044b0d-ecb9-486f-9447-ddf8c5f65f64/CLASSLION%20View%20APIView%20f672defce2e34d84ada0b24cffba799c.md">View & APIView</a>

[4] <a href="https://github.com/KumJungMin/REACT-DJANGO/blob/master/Export-5c01d905-91b6-43ed-b457-d31b6252a625/CLASSLION%20Mixins%20generic%20CBV%20f8b2f7c2548b4f1997b56f9829777e45.md">Mixins & generic CBV</a>

[5] <a href="https://github.com/KumJungMin/REACT-DJANGO/blob/master/Export-ac5377dc-bf69-4da5-b2b0-5d187fc32697/CLASSLION%20ViewSet%20af35b5ecbc044947a33594edc5d3a4b0.md">ViewSet</a>

[6] <a href="https://github.com/KumJungMin/REACT-DJANGO/blob/master/Export-af386b95-5b40-4ba6-ab11-37653db27114/CLASSLION%20Router%20f7f68d9d466948d498872fca76ebc1da.md">Router</a>
