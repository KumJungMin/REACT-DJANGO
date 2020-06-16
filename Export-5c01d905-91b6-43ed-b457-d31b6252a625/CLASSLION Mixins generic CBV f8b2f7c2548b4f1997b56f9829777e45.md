# CLASSLION:Mixins & generic CBV

우리는 앞서 APIView를 이용하여 뷰를 설정했다.

그런데 만약 우리가 여러 개의 모델객체를 생성했고, 이 모델 객체에 대해 list를 보여주고자할 때, 비슷한 논리의 코드가 모델객체 수 만큼 반복된다. 비효율적!

그렇다면 이러한 낭비는 어떻게 막을 수 있을까? 상속의 성질을 이용하면 된다.

<br/>

# 3. Mixins

---

: 믹스인은 중복되는 코드를 상속의 성질을 이용하여 줄인 케이스이다.

: 오른쪽 코드는 우리가 import한 generic에 내장되어 있던 함수나 변수.  우리는 이 변수들을 재정의하는 방식으로 사용한다.(none → 우리가 지정)

![CLASSLION%20Mixins%20generic%20CBV%20f8b2f7c2548b4f1997b56f9829777e45/_2019-08-22__10.35.57.png](CLASSLION%20Mixins%20generic%20CBV%20f8b2f7c2548b4f1997b56f9829777e45/_2019-08-22__10.35.57.png)

<br/>

## 1) SnippetList클래스

### (1) 역할

: 스니펫 목록의 리스트 목록을 보여준다.

: `mixins`에서 `ListModel, CreateModel`을 가져와서 상속한다.

<br/>

### (2) 코드 핵심

```python 
queryset = Snippet.objects.all()     
serializer_class = SnippetSerializer 
```

: 쿼리셋, 직렬화변수는 generics의 GenericAPIView에 있는 변수들.

: 어떤 모델을 기반으로 쿼리셋을 설정할지. 어떤 직렬화 클래스를 가지고 직렬화를 할 건지 설정.    

```python
def get(self, request, *args, **kwargs):
	return self.list(request, *args, **kwargs)
```

: 인수는 가변인수로 하여, 리스트에서 요청하고 싶은 `http메소드`를 정의한다.

: 이 메소드들은 리턴으로 메소드(`list, create`)를 리턴하여 이 인수를 이용해 데이터를 처리한다.

: get메소드 리턴시 실행되는 `list메소드`는 `mixins.ListModelMixins`에 있다.

: post메소드 리턴시 실행되는 `create메소드`는 `mixins.CreateModelMixins`에 내장되어 있다.

<br/>

### (3) 전체 코드

```python
from snippets.models import Snippet
from snippets.serializers import SnippetSerializer
from rest_framework import mixins    # 
from rest_framework import generics  #

class SnippetList(mixins.ListModelMixin, mixins.CreateModelMixin, generics.GenericAPIView):
#쿼리셋, 직렬화변수는 generics의 GenericAPIView에 있는 변수들.
	queryset = Snippet.objects.all()      #어떤 모델을 기반으로 쿼리셋을 설정할지.
	serializer_class = SnippetSerializer  #어떤 직렬화 클래스를 가지고 직렬화를 할 건지 설정.    

#get메소드 리턴시 실행되는 list메소드는 mixins.ListModelMixins에 있다.
#post메소드 리턴시 실행되는 create메소드는 mixins.CreateModelMixins에 내장되어 있다.
	def get(self, request, *args, **kwargs):
		return self.list(request, *args, **kwargs)
	
	def post(self, request, *args, **kwargs):
		return self.create(request, *args, **kwargs)
```

: mixin파일에 있는 `CreateModelMixin`클래스를 보면, 우리가 앞서 `apiview`를 상속할 때 쓰던 긴 코드가 `create메소드`에 내장되어 있음을 알 수 있다. 

  결론적으로 우리는 다른 곳에서 이미 정의된 코드를 import를 이용하여 사용하며, 이 방법으로 코드의 길이를 줄일 수 있다.

![CLASSLION%20Mixins%20generic%20CBV%20f8b2f7c2548b4f1997b56f9829777e45/_2019-08-22__10.47.56.png](CLASSLION%20Mixins%20generic%20CBV%20f8b2f7c2548b4f1997b56f9829777e45/_2019-08-22__10.47.56.png)

<br/>
<br/>

## 2) SnippetDetail클래스

### (1) 역할

: 스니펫에 있는 각각의 데이터를 다루는 용도로 이용된다. 

: `mixins`에서 `RetrieveModel, UpdateModel, DestoryModel`을 가져와서 상속한다.

<br/>

### (2) 전체 코드

: `SnippetDetail`클래스는 `get, put, delete`메소드를 생성하여 이에 대한 리턴으로 `mixins`에 있는 메소드를 실행시킨다.

```python
class SnippetDetail(mixins.RetrieveModelMixin, mixins.UpdateModelMixin, mixins.DestroyModelMixin, generic.GenericAPIView):
	queryset = Snippet.object.all()
	serializer_class = SnippetSerializer

	def get(self, request, *args, **kwargs):    #내용보기
		return self.retrieve(request, *args, **kwargs)

	def put(self, request, *args, **kwargs):    #수정하기
		return self.update(request, *args, **kwargs)

	def delete(self, request, *args, **kwargs): #삭제하기
		return self.destory(request, *args, **kwargs)
```

<br/><br/>

# 4. generic CBV

---

: 우리는 앞서 `APIVIew`의 코드를 상속개념을 이용하여 `mixins`으로 줄였는데, 이번에는 믹스인 조차도 줄여보자!

## 1) 전체 코드

: 이번에는 `generics`에 있던 `ListCreateAPIVIew`를 상속받아, 변수를 재정의한다.

: 실질적인 코드들은 상속한 것에 다 들어있어 코드가 좀 더 간단해진다.

```python
from snippets.models import Snippet
from snippets.serializers import SnippetSerializer
from rest_framework import generics

#어떤 모델을 어떤 직렬화로 등록해줄지 generics의 변수 재정의를 해준다.
class SnippetList(generics.ListCreateAPIView):
	queryset = Snippet.object.all()
	serializer_class = SnippetSerializer

class SnippetDetail(generics.RetrieveUpdateDestroyAPIView):
	queryset = Snippet.object.all()
	serializer_class = SnippetSerializer

```
