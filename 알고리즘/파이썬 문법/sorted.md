**sorted(정렬할 데이터)**

**sorted(정렬할 데이터, reverse 파라미터)**

**sorted(정렬할 데이터, key 파라미터)**

**sorted(정렬할 데이터, key 파라미터, reverse 파라미터)**

sorted 함수는 **파이썬 내장 함수입니다.**  
첫 번째 **매개변수로 들어온 이터러블한 데이터**를 **새로운 정렬된 리스트로 만들어서 반환**해 주는 함수입니다.

- 첫 번째 매개변수로 들어올 **"정렬할 데이터"는 iterable 한 데이터** 이어야 합니다.

아래 옵션(파라미터)은 다 기본값으로 들어가 있기 때문에 **sorted(정렬 데이터)만** 넣어도 충분합니다.

**- key 옵션 (key 파라미터)**  
sorted 함수의 key 파라미터는 어떤 것을 기준으로 정렬할 것인가? 에 대한 기준입니다.  
즉, key 값을 기준으로 비교를 하여 정렬을 하겠다는 것인데, 이것을 정해 줄 수 있는 파라미터입니다.  
sorted( ~~ , key=뭐뭐)로 입력하게 되면 해당 키를 기준으로 정렬하여 반환합니다.

**- reverse 옵션 (reverse 파라미터)**  
해당 파라미터를 이용하면 오름차순으로 정렬할지 내림차순으로 정렬할지 정할 수 있습니다.  
디폴트로는 reverse=False로 오름차순으로 정렬이 됩니다.  
sorted( ~~ , reverse=True)로 입력하게 되면 내림차순으로 정렬하여 반환합니다.

** **리스트.sort()와 sorted(리스트)의 가장 큰 차이**는  
**리스트.sort()** 는 **본체의 리스트를 정렬해서 변환**하는 것이고,  
**sorted(리스트)** 는 **본체 리스트는 내버려두고, 정렬한 새로운 리스트를 반환하는** 것입니다.


## **2. 파이썬 정렬 sorted 함수 예제**

---

#### **2-1) sorted 함수 예제 : 리스트 정렬**

|   |   |   |
|---|---|---|
1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9|
a = [2, 4, 1, 9, 100, 29, 40, 10]<br><br>b = sorted(a)<br><br>c = sorted(a, reverse=True)<br><br>print(f'origin                  : {a}')<br><br>print(f'sorted(a)               : {b}')<br><br>print(f'sorted(a, reverse=True) : {c}')<br><br>[Colored by Color Scripter](http://colorscripter.com/info#e)|
[cs](http://colorscripter.com/info#e)|

![](https://blog.kakaocdn.net/dn/UrByg/btqQbFGLgGW/BEDfpXksE9xqfLdqVxahOK/img.png)

파이썬 리스트 sorted 예제

sorted 함수를 이용해서 리스트를 정렬해 보았습니다.  
리스트 자체 함수인 sort는 해당 리스트를 정렬하는 것과 달리  
이 sorted 함수는 정렬한 새로운 리스트를 반환하는 것을 볼 수 있습니다.

**정렬된 리스트 = sorted(기존 리스트)**

또한, **sorted 함수의 reverse 파라미터(옵션)를 True**로 변경하면 **내림차순**으로 정렬하는 것을 볼 수 있습니다.

#### **2-2) sorted 함수 예제 :  딕셔너리 key 정렬**

|   |   |   |
|---|---|---|
1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10<br><br>11<br><br>12<br><br>13<br><br>14<br><br>15<br><br>16<br><br>17<br><br>18<br><br>19<br><br>20<br><br>21<br><br>22<br><br>23<br><br>24<br><br>25<br><br>26<br><br>27<br><br>28<br><br>29<br><br>30<br><br>31<br><br>32<br><br>33<br><br>34<br><br>35<br><br>36|
# 딕셔너리<br><br>d = dict()<br><br>d['a'] = 66<br><br>d['i'] = 20<br><br>d['e'] = 30<br><br>d['d'] = 33<br><br>d['f'] = 50<br><br>d['g'] = 60<br><br>d['c'] = 22<br><br>d['h'] = 80<br><br>d['b'] = 11<br><br># 1. 딕셔너리 출력<br><br>print("\n1. 기본 딕셔너리")<br><br>print(d)<br><br># 딕셔너리 키 정렬 오름차순(디폴트)<br><br>print("\n2. 키 정렬 sorted(d.items())")<br><br>f = sorted(d.items())<br><br>print(f)<br><br># 딕셔너리 키 정렬 내림차순<br><br>print("\n3. 키 정렬 sorted(d.items(), reverse=True)")<br><br>g = sorted(d.items(), reverse=True)<br><br>print(g)<br><br># 딕셔너리 키만 정렬 및 출력1<br><br>print("\n4. 키만 정렬 sorted(d.keys())")<br><br>h = sorted(d.keys())<br><br>print(h)<br><br># 딕셔너리 키만 정렬 및 출력2<br><br>print("\n5. 키만 정렬 sorted(d)")<br><br>i = sorted(d)<br><br>print(i)<br><br>[Colored by Color Scripter](http://colorscripter.com/info#e)|
[cs](http://colorscripter.com/info#e)|

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

#### **2-3) sorted 함수 예제 :  딕셔너리 value 정렬 (operator 모듈 이용)**

|   |   |   |
|---|---|---|
1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10<br><br>11<br><br>12<br><br>13<br><br>14<br><br>15<br><br>16<br><br>17<br><br>18<br><br>19<br><br>20<br><br>21<br><br>22|
import operator<br><br>d = {'b': 400, 'f': 300, 'a': 200, 'd': 100, 'c': 500}<br><br>print('1. 원본 딕셔너리')<br><br>print(d.items())<br><br>print('\n2. 딕셔너리 정렬 : sorted(d.items())')<br><br>f = sorted(d.items())<br><br>print(f)<br><br>print('\n3. 딕셔너리 정렬 : sorted(d.items(), key=operator.itemgetter(1))')<br><br>g = sorted(d.items(), key=operator.itemgetter(1))<br><br>print(g)<br><br>print('\n4. 딕셔너리 정렬 : sorted(d.items(), key=operator.itemgetter(1), reverse=True)')<br><br>h = sorted(d.items(), key=operator.itemgetter(1), reverse=True)<br><br>print(h)<br><br>[Colored by Color Scripter](http://colorscripter.com/info#e)|
[cs](http://colorscripter.com/info#e)|

![](https://blog.kakaocdn.net/dn/CvcNx/btqQlKfqlNX/UKinqQWaOdopQXP40v1G5k/img.png)

파이썬 딕셔너리 value 정렬 sorted 함수 이용

딕셔너리의 value를 기준으로 sorted함수를 이용하여 정렬을 하기 위한 첫 번째 방법입니다.  
operator 모듈의 itemgetter 함수를 이용하여서 딕셔너리의 value 값에 접근을 하여 그 값을 기준으로 정렬해달라 할 수 있습니다.   
아래와 같이 말이죠.  
**sorted(딕셔너리.items(), key=operator.itemgetter(1))**

#### **2-4) sorted 함수 예제 : 딕셔너리 value 정렬 (lambda 이용)**

|   |   |   |
|---|---|---|
1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10<br><br>11<br><br>12<br><br>13<br><br>14<br><br>15<br><br>16<br><br>17<br><br>18<br><br>19<br><br>20<br><br>21<br><br>22|
import operator<br><br>d = {'blockdmask': 400, 'equal': 300, 'apple': 200, 'dish': 100, 'cook': 500}<br><br>print('1. 원본 딕셔너리')<br><br>print(d.items())<br><br>print('\n2. 딕셔너리 정렬 : sorted(d.items())')<br><br>f = sorted(d.items())<br><br>print(f)<br><br>print('\n3. 딕셔너리 정렬 : sorted(d.items(), key=lambda x: x[1])')<br><br>g = sorted(d.items(), key=lambda x: x[1])<br><br>print(g)<br><br>print('\n4. 딕셔너리 정렬 : sorted(d.items(), key=lambda x: x[1], reverse=True)')<br><br>h = sorted(d.items(), key=lambda x: x[1], reverse=True)<br><br>print(h)<br><br>[Colored by Color Scripter](http://colorscripter.com/info#e)|
[cs](http://colorscripter.com/info#e)|

![](https://blog.kakaocdn.net/dn/bBgCZR/btqQjnY5ow5/FUVW8jpGUjWzLrKcKZ92pk/img.png)

sorted 함수 파이썬 딕셔너리 value 정렬 lambda 이용

sorted 함수에서 딕셔너리 value를 기준으로 정렬을 하기 위한 두 번째 방법은 람다를 이용하는 것입니다.

**sorted(딕셔너리.items(), key=lambda x: x[1])** 이를 이용해서 **key=를 x로 세팅해**주어서 **정렬하**도록 할 수 있습니다.

출처: [https://blockdmask.tistory.com/466](https://blockdmask.tistory.com/466) [개발자 지망생:티스토리]