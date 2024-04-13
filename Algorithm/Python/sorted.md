- sorted()는 파이썬 내장 [[메서드(Method)]]이다.  

- sort()와 sorted()의 가장 큰 차이는 sort()는 본체의 리스트를 정렬해서 변환하는 것이고,  sorted(리스트)는 본체 리스트는 내버려두고, 정렬한 새로운 리스트를 반환하는 것이다.


## 문법

- 첫 번째 매개변수로 들어온 이터러블한 데이터를 새로운 정렬된 리스트로 만들어서 반환해 주는 함수이다.
- 첫 번째 매개변수로 들어올 "정렬할 데이터"는 iterable 한 데이터 이어야한다.

- 아래 옵션(매개변수)은 다 기본값으로 들어가 있기 때문에 sorted(정렬 데이터)만 넣어도 충분하다.

### key(key 파라미터) 옵션

sorted 함수의 key 파라미터는 어떤 것을 기준으로 정렬할 것인가? 에 대한 기준입니다.  
즉, key 값을 기준으로 비교를 하여 정렬을 하겠다는 것인데, 이것을 정해 줄 수 있는 파라미터입니다.  
sorted( ~~ , key=뭐뭐)로 입력하게 되면 해당 키를 기준으로 정렬하여 반환합니다.

### reverse (reverse 파라미터) 옵션

- 해당 파라미터를 이용하면 오름차순으로 정렬할지 내림차순으로 정렬할지 정할 수 있다.
- 디폴트로는 reverse=False로 오름차순으로 정렬이 된다.

- sorted( ~~ , reverse=True)로 입력하게 되면 내림차순으로 정렬하여 반환한다.


## 파이썬 정렬 sorted 함수 예시

### 리스트 정렬

```python
a = [2, 4, 1, 9, 100, 29, 40, 10]
b = sorted(a)
c = sorted(a, reverse=True)

print(f'origin: {a}')
print(f'sorted(a: {b}')
print(f'sorted(a, reverse=True) : {c}')
```

![](https://blog.kakaocdn.net/dn/UrByg/btqQbFGLgGW/BEDfpXksE9xqfLdqVxahOK/img.png)

- 리스트 자체 [[메서드(Method)]]인 sort()는 해당 리스트를 정렬하는 것과 달리 sorted() [[메서드(Method)]]는 정렬한 새로운 리스트를 반환하는 것을 볼 수 있다.

- 또한, sorted 함수의 reverse 파라미터(옵션)를 True로 변경하면 내림차순으로 정렬하는 것을 볼 수 있다.


### 딕셔너리 key 정렬

```python
d = dict()<br><br>d['a'] = 66<br><br>d['i'] = 20<br><br>d['e'] = 30<br><br>d['d'] = 33<br><br>d['f'] = 50<br><br>d['g'] = 60<br><br>d['c'] = 22<br><br>d['h'] = 80<br><br>d['b'] = 11<br><br># 1. 딕셔너리 출력<br><br>print("\n1. 기본 딕셔너리")<br><br>print(d)<br><br># 딕셔너리 키 정렬 오름차순(디폴트)<br><br>print("\n2. 키 정렬 sorted(d.items())")<br><br>f = sorted(d.items())<br><br>print(f)<br><br># 딕셔너리 키 정렬 내림차순<br><br>print("\n3. 키 정렬 sorted(d.items(), reverse=True)")<br><br>g = sorted(d.items(), reverse=True)<br><br>print(g)<br><br># 딕셔너리 키만 정렬 및 출력1<br><br>print("\n4. 키만 정렬 sorted(d.keys())")<br><br>h = sorted(d.keys())<br><br>print(h)<br><br># 딕셔너리 키만 정렬 및 출력2<br><br>print("\n5. 키만 정렬 sorted(d)")<br><br>i = sorted(d)<br><br>print(i)
```


![](https://blog.kakaocdn.net/dn/Q6xj4/btqQpuCSdHU/0kL5wsAAKYxh6Nv0dk5JTk/img.png)

파이썬 딕셔너리 key 정렬 sorted 함수 이용

이렇게 딕셔너리의 키 값을 기준으로 정렬을 할 수 있습니다.  
sorted(딕셔너리.items())을 이용해서 키값을 기준으로 정렬을 하고, value도 그게 맞게 따라가서 출력되도록 할 수 있고,

그게 아니라 딕셔너리의 key 만 필요하다면  
sorted(딕셔너리.keys()) 혹은 sorted(딕셔너리)을 이용하면 잘 정렬되는 것을 볼 수 있습니다.

역시 보면 sorted 함수의 반환형은 딕셔너리라 하더라도 **[] 대괄호**로 묶여있는 걸 확인할 수 있습니다.  
즉, **sorted 함수의 반환형은 리스트 타입**입니다. **sorted 함수의 인자로 들어온 딕셔너리를 정렬**해서 **리스트 타입으로 반환**해주는 것을 확인할 수 있습니다.

예제 2, 3번을 보면 **딕셔너리의 key, value쌍들이 리스트 안에 각 튜플 타입으로 변환**되어 들어가 있는 것을 볼 수 있습니다.  
[('i', 20), ('h', 80), ('g', 60), ('f', 50), ('e', 30), ('d', 33), ('c', 22), ('b', 11), ('a', 66)] 전체적으로는 리스트로 감싸여 있고 리스트 안의 값으로 튜플 타입으로 되어있는 것을 볼 수 있습니다.  
**정리 : sorted 함수로 딕셔너리 정렬을 하게 되면, key, value가 각각 튜플로 묶이고 그것들의 리스트로 만들어서 반환**해 주는 것을 알 수 있습니다.

### 딕셔너리 value 정렬 (operator 모듈 이용)

```python
import operator

d = {'b': 400, 'f': 300, 'a': 200, 'd': 100, 'c': 500}

print('1. 원본 딕셔너리')

print(d.items())

print('\n2. 딕셔너리 정렬 : sorted(d.items())')

f = sorted(d.items())
print(f)

print('\n3. 딕셔너리 정렬 : sorted(d.items(), key=operator.itemgetter(1))')

g = sorted(d.items(), key=operator.itemgetter(1))
print(g)
print('\n4. 딕셔너리 정렬 : sorted(d.items(), key=operator.itemgetter(1), reverse=True)')

h = sorted(d.items(), key=operator.itemgetter(1), reverse=True)
print(h)
```

![](https://blog.kakaocdn.net/dn/CvcNx/btqQlKfqlNX/UKinqQWaOdopQXP40v1G5k/img.png)

파이썬 딕셔너리 value 정렬 sorted 함수 이용

딕셔너리의 value를 기준으로 sorted함수를 이용하여 정렬을 하기 위한 첫 번째 방법입니다.  
operator 모듈의 itemgetter 함수를 이용하여서 딕셔너리의 value 값에 접근을 하여 그 값을 기준으로 정렬해달라 할 수 있습니다.   
아래와 같이 말이죠.  
**sorted(딕셔너리.items(), key=operator.itemgetter(1))**

### 딕셔너리 value 정렬 (lambda 이용)

```python
import operator

d = {'blockdmask': 400, 'equal': 300, 'apple': 200, 'dish': 100, 'cook': 500}

print('1. 원본 딕셔너리')
print(d.items())

print('\n2. 딕셔너리 정렬 : sorted(d.items())')
f = sorted(d.items())
print(f)

print('\n3. 딕셔너리 정렬 : sorted(d.items(), key=lambda x: x[1])')
g = sorted(d.items(), key=lambda x: x[1])

print(g)print('\n4. 딕셔너리 정렬 : sorted(d.items(), key=lambda x: x[1], reverse=True)')

h = sorted(d.items(), key=lambda x: x[1], reverse=True)
print(h)
```

![](https://blog.kakaocdn.net/dn/bBgCZR/btqQjnY5ow5/FUVW8jpGUjWzLrKcKZ92pk/img.png)

sorted 함수 파이썬 딕셔너리 value 정렬 lambda 이용

sorted 함수에서 딕셔너리 value를 기준으로 정렬을 하기 위한 두 번째 방법은 람다를 이용하는 것입니다.

**sorted(딕셔너리.items(), key=lambda x: x[1])** 이를 이용해서 **key=를 x로 세팅해**주어서 **정렬하**도록 할 수 있습니다.

출처: [https://blockdmask.tistory.com/466](https://blockdmask.tistory.com/466) [개발자 지망생:티스토리]