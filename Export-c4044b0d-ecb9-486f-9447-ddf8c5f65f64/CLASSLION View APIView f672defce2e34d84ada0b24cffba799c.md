# CLASSLION:View & APIView

# 1. View를 배우는 과정

---

이전 시간에 배운 Viewset는 view(CRUD)를 설계하는 쉽고 간단한 방법이다. 

오늘은 바로 Viewset을 하는 게 아닌 APIView → Mixins → Generic CBV → ViewSet 순서로 진행해보자.

아니 쉽고 간단한 ViewSet이 있는데 더 코드가 긴 다른 것들은 왜 배움?

![CLASSLION%20View%20APIView%20f672defce2e34d84ada0b24cffba799c/_2019-08-20__10.56.49.png](CLASSLION%20View%20APIView%20f672defce2e34d84ada0b24cffba799c/_2019-08-20__10.56.49.png)

: 사실 이 Viewset은 이전단계를 상속하여 만들어진다.

: APIView를 상속하여 → MIXins → MIXins을 상속하여 → Generic CBV → Generic CBV를 상속하여 → viewset이다. 

 그러니까 간단한 viewset 쓰고 싶어도 이전 단계 것들을 이해해야 가능.
 
 <br/>

*그냥 이 viewset만 계속 쓰면 되지 않음?*

: ㅇㅇ안됨. 나중에 커스터마이징을 해서 코드를 작성하거나 버그가 발생했을 때 구조를 이해 못하면 해결하는데 겁나 힘듬;

<br/>

# 2. APIView

---

## 1) 코드 분석

: 리스트를 보여주는 SnippetList, 상세내용을 보여주는 SnippetDetail가 있다.

: 이 클래스뷰들은 APIView를 상속하여 get, post 등등의 요청에 대해 데이터를 가공하여 처리한다.

: 이 클래스에서 메소드들을 정의할 때, 내가 쓰고자하는 Http이름으로 정의한다.

: SnippetDetail클래스는 APIView를 상속받아, 데이터에 대해 get, put, delete요청을 하고 싶으므로 이것들을 메소드로 정의하면 된다.

![CLASSLION%20View%20APIView%20f672defce2e34d84ada0b24cffba799c/_2019-08-21__11.02.01.png](CLASSLION%20View%20APIView%20f672defce2e34d84ada0b24cffba799c/_2019-08-21__11.02.01.png)

<br/>

### (1) import 부분

| APIView | 장고의 APIView를 상속받는 클래스를 사용하므로 import 한다. |
|:--------:|:--------:|
| Serializers | 직렬화파일에서 특정 직렬화클래스를 import한다. |
| models | 모델파일에서 모델객체를 import한다. |
| Http404 | 404에러를 띄우기 위해 Http404를 import한다. |
| Status, Response | APIView를 상속하여 view를 설계할 때, status, response를 import해서, 직접 응답 과정을 만든다. <br /> (어떤 status인지에 따라 response를 함)  |

<br/>

### (2) SnippetList

| 역할 | list 뷰를 당담한다. |
|:--------:|:--------:|
| 형태 | APIView를 상속한 클래스 형태이다. |
| http 요청 | 리스트에서 필요한 요청은 get, post이다. |

<br/>

### (3) SnippetDetail

| 역할 | 상세내용을 보여주는 역할을 담당한다. |
|:--------:|:--------:|
| 형태 | APIView를 상속한 클래스 형태이다. |
| get_404 함수 | 404를 구현한 코드이다. |
| http 요청 | get, put, delete이다. |

<br/>

## 2) Views.py 코드 작성

: APIView형식으로 view클래스를 작성해보자!

<br/>

### (1) 데이터 처리 대상

: PostDetail 클래스의 get_object 메소드 대신 아래 코드를 써도 된다.

: `from django.shortcuts import get_object_or_404`

```python
|from post.models import Post
|from post.serializer import PostSerializer
|#statusdㅔ 따라 직접 response를 처리
|from django.http import Http404   #get object or 404 직접 구현
|from rest_framework.response import Response
|from rest_framework import status 
|#APIView를 상속받은 CBV
|from rest_framework.views import APIView
```
<br/>

### (2) class 부분 - PostList

: 객체들의 리스트를 보여주거나 새 글 객체를 등록한다.

: response가 인자로 받는 것은 순서대로 (데이터, status, template_name, header ...)가 있다.

```python
|class PostList(APIView):
|  def get(self, request):  # 리스트 보여줭!
|    posts = Post.objects.all() #모든 객체들의 쿼리셋 가져오기
|    serializer = PostSerializer(posts, many = True)
|    #다수의 객체(posts)를 직렬화할 때는 many = true로 함.
|    return Response(serializer.data)
|    #직접 직렬화한 데이터를 응답으로 리턴해준다.
|
|  def post(self, request): # 새 글을 등록하게 해줘!
|    serializer = PostSerializer(data=request.data) #내가 request한 데이터가 직렬화가 되어야 함.
|    if serializer.is_valid():  #유효성 검사
|      serializer.save()        #검사시 문제 없으면 저장
|      return Response(serializer.data, status=status.HTTP_201_CREATED)
|      #문제없이 잘 됐으면, 직렬화한 데이터와 객체 생성하는 http요청으로 응답해준다.
|     return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
|     #유효성 검사시 문제가 있다면, bad_request를 응답으로 보낸다.
|
```

<br/>

### (3) class 부분 - PostDetail

: 각각의 데이터 객체에 대해 보기, 수정, 삭제를 한다.

```python
|class PostDetail(APIView):
|  #get_obejct_or_404를 구현해주는 helper function
|  def get_object(self, pk):
|    try:
|      return Post.objects.get(pk=pk)
|    except Post.DoesNotExist:
|      raise Http404
| 
|  def get(self, request, pk, format=None):
|    post = self.get_object(pk)
|    #post = get_object_or_404(Post, pk)
|    serializer = PostSerializer(post)
|    return Response(serializer.data)
|
|  def put(self, request, pk, format=None): #PK값을 이용한다. 
|    post = self.get_object(pk)     #요청한 pk에 해당하는 객체를 가져온다.
|    serializer = PostSerializer(post, data=request.data)
|    #요청한 데이터를 post에 씌우고 -> 직렬화한다.
|    if serializer.is_vaild():      #만약 이 직렬화한 데이터가 유효하다면
|      serializer.save()            # 저장
|      return Response(serializer.data)
|    return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)  
|
|  def delete(self, request, pk, format=None):
|    post = self.get_object(pk)
|    post.delete()
|    return Response(status=status.HTTP_204_NO_CONTECT)
```

<br/>

### (4) Urls.py 코드 작성

:  url을 이용하여 특정주소를 요청하면 그에 해당하는 view클래스가 발동하여 데이터를 가져오게 한다.

```python
from django.urls import path
from rest_framework.urlpattens import format_suffix_pattens
from post import views

#default router은 사용 x => api root 없음

urlpatterns = [
  #127.0.0.1:8000/   post == ListView
	path('post/', views.PostList.as_view()),
  #127.0.0.1:8000/post/<pk> == DetailView
	path('post/<int:pk>/', views.PostDetail.as_view()),
]

urlpattens = format_suffix_patterns(urlpattens)
```

: 이렇게 url까지 작성을 하면 해당 페이지에 접속하여 api가 제대로 작동하는 지 확인할 수 있다.

<br/><br/><br/>


### 장고 rest-api

[1]  <a href="https://github.com/KumJungMin/REACT-DJANGO/blob/master/CLASSLION%20JSON%2073dffc642c0b4666b96c9605dae106df.md">JSON이란?</a>

[2] <a href="https://github.com/KumJungMin/REACT-DJANGO/blob/master/Export-3b5f2385-ed14-4878-9265-e1ab00dd6924/CLASSLION%20Rest%20Serializers%20001660ba1dc140a7ab99f15d82a7766e.md">Rest, Serializers</a>

[3] <a href="https://github.com/KumJungMin/REACT-DJANGO/blob/master/Export-c4044b0d-ecb9-486f-9447-ddf8c5f65f64/CLASSLION%20View%20APIView%20f672defce2e34d84ada0b24cffba799c.md">View & APIView</a>

[4] <a href="https://github.com/KumJungMin/REACT-DJANGO/blob/master/Export-5c01d905-91b6-43ed-b457-d31b6252a625/CLASSLION%20Mixins%20generic%20CBV%20f8b2f7c2548b4f1997b56f9829777e45.md">Mixins & generic CBV</a>

[5] <a href="https://github.com/KumJungMin/REACT-DJANGO/blob/master/Export-ac5377dc-bf69-4da5-b2b0-5d187fc32697/CLASSLION%20ViewSet%20af35b5ecbc044947a33594edc5d3a4b0.md">ViewSet</a>

[6] <a href="https://github.com/KumJungMin/REACT-DJANGO/blob/master/Export-af386b95-5b40-4ba6-ab11-37653db27114/CLASSLION%20Router%20f7f68d9d466948d498872fca76ebc1da.md">Router</a>
