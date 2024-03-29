
- [[병합 정렬(Merge Sort)]]과 마찬가지로 퀵 정렬도 분할 정복(Devide and Conquer)기법과 재귀 알고리즘을 이용한 정렬 알고리즘이다.
- 정렬 라이브러리의 근간이 되는 알고리즘으로 기준 데이터를 설정하고 그 기준보다 큰 데이터와 작은 데이터의 위치를 바꾸는 동작이 핵심이다.

- 퀵 정렬은 기준을 설정한 다음 큰 수와 작은 수를 교환한 후 리스트를 반으로 나누는 방식으로 동작한다.
- 원리를 이해하면 [[병합 정렬(Merge Sort)]]과 [[힙 정렬(Heap Sort)]] 등 다른 고급 정렬 기번에 비해 쉽게 소스코드를 작성할 수 있다.

## 퀵정렬의 알고리즘

- 쉬운 이해를 위해서 다음과 같이 1부터 7까지 총 7개의 숫자가 들어있는 배열을 기준으로 설명한다.

```python
[6, 5, 1, 4, 7, 2, 3]
```

- 항상 정 가운데를 기준으로 분할을 하는 병합 정렬과 달리, 퀵 정렬은 흔히 피벗(pivot)이라고 불리는 임의의 기준값을 사용한다.
- pivot 값을 선택하는데는 여러 가지 방법이 있지만 여기서는 정 중앙에 위치한 4을 pivot으로 정한다.

- 그리고 이 pivot 값을 기준으로 pivot보다 작은 값의 그룹과 pivot보다 큰 값의 그룹으로 나눈다.

```python
            p
[3, 2, 1] < 4 < [7, 5, 6]
```

- 위와 같이 피벗 값보다 작은 값들은 모두 왼편으로 몰고, 큰 값들은 모두 오른편으로 몰면 기준값은 정확히 정렬된 위치에 놓이게 된다. 
- 또한 이런 방식으로 분할을 해놓으면 앞으로 더 이상 왼편에 있는 값들과 오른편에 있는 값들 간에는 비교를 할 필요가 없다.

- 따라서 반대편은 전혀 신경쓰지 않고 왼편이든 오른편이든 같은편 내의 값들 끼리만 비교 후 정렬을 할 수 있게 된다.

- 먼저 왼편을 동일한 방식으로 정렬해보도록 한다.
- 왼편의 정 가운데에 위치한 pivot 값인 2보다 작은 값인 1인 왼쪽에 큰 값인 3은 오른쪽에 위치시켰다.
- 이제 양쪽 모두 값이 하나씩 밖에 없기 때문에 이로써 왼편의 정렬 작업은 완료되었다.

```python
      p
[1] < 2 < [3]
```

- 오른편도 동일한 방식으로 정렬해보자. 
- 오른편의 pivot 값인 5 보다 작은 값은 없으므로 7과 6을 모두 오른편에 위치시켯다.

```python
     p
[] < 5 < [7, 6]
```

- 오른편에는 값이 2개가 있기 때문에 추가 정렬이 필요하다.
- 왼편에는 값이 없지만 오른편에는 여전히 두 개의 값이 있기 때문에, 동일한 방식의 정렬을 적용한다.

```python
      p
[6] < 7 < []
```

- 마지막으로 지금까지 좌우로 분할했던 값들을 모두 합치보면 다음과 같이 정렬된 배열을 얻을 수 있다.

```python
[1, 2, 3, 4, 5, 6, 7]
```

- 지금까지 살펴본 것과 같이 퀵 정렬은 배열을 피벗값 기준으로 더 작은 값과 큰 값으로 반복적으로 분할하여 정렬해나가는 방식을 취하고 있다.

## 특징

- 파이썬의 `list.sort()` 함수나 자바의 `Arrays.sort()`처럼 프로그래밍 언어 차원에서 기본적으로 지원되는 내장 정렬 함수는 대부분은 퀵 정렬을 기본으로 합니다.
- 일반적으로 원소의 개수가 적어질수록 나쁜 중간값이 선택될 확률이 높아지기 때문에, 원소의 개수에 따라 퀵 정렬에 다른 정렬을 혼합해서 쓰는 경우가 많습니다.
- 병합 정렬과 퀵 정렬은 분할 정복과 재귀 알고리즘을 사용한다는 측면에서는 유사해보이지만, 내부적으로 정렬을 하는 방식에서는 큰 차이가 있습니다.
- 병합 정렬은 항상 정 중앙을 기준으로 단순 분할 후 병합 시점에서 값의 비교 연산이 발생하는 반면, 퀵 정렬은 분할 시점부터 비교 연산이 일어나기 때문에 그 이후 병합에 들어가는 비용이 매우 적거나 구현 방법에 따라서 아예 병합을 하지 않을 수도 있습니다.

복잡도
- 쿽 정렬의 성능은 pivot 값을 어떻게 선택하느냐에 크게 달라질 수 있습니다. 이상적인 경우에는 pivot 값을 기준으로 동일한 개수의 작은 값들과 큰 값들이 분할되어 병합 정렬과 마찬가지로 `O(nlog(n))`의 시간 복잡도를 가지게 됩니다.
- 하지만 pivot 값을 기준으로 분할했을 때 값들이 한 편으로 크게 치우치게 되면, 퀵 정렬은 성능은 저하되게 되며, 최악의 경우 한 편으로만 모든 값이 몰리게 되어 `O(n^2)`의 시간 복잡도를 보이게 됩니다.
- 따라서 상용 코드에서는 중앙값(median)에 가까운 pivot 값을 선택할 수 있는 섬세한 전략이 요구되며, 배열의 첫값과 중앙값 그리고 마지막값 중에 크기가 중간인 값을 사용하는 방법이 많이 사용됩니다.
- 퀵 정렬은 공간 복잡도는 구현 방법에 따라 달라질 수 있는데, 입력 배열이 차지하는 메모리만을 사용하는 in-place sorting 방식으로 구현을 사용할 경우, `O(log(n))`의 공간 복잡도를 가진 코드의 구현이 가능합니다.


### 알고리즘

코드가 이해가 어려울 수도 있기 때문에 간단히 알고리즘을 설명하겠습니다. 메인 함수인 `quick_sort()`는 크게 `sort()`와 `partition()` 2개의 내부 함수로 나눠집니다. `sort()` 함수는 재귀 함수이며 정렬 범위를 시작 인덱스와 끝 인덱스로 인자로 받습니다. (둘다 [[inclusive]]) `partition()` 함수는 정렬 범위를 인자로 받으며 다음 로직을 따라서 좌우측의 값들을 정렬하고 분할 기준점의 인덱스를 리턴합니다. 이 분할 기준점(mid)는 `sort()`를 재귀적으로 호출할 때 우측 리스트의 시작 인덱스로 사용됩니다.

- 리스트의 정 가운데 있는 값을 pivot 값을 선택합니다.
- 시작 인덱스(low)는 계속 증가 시키고, 끝 인덱스(high)는 계속 감소 시키기위한 while 루프를 두 인덱스가 서로 교차해서 지나칠 때까지 반복시킵니다.
    - 시작 인덱스(low)가 가리키는 값과 pivot 값을 비교해서 더 작은 경우 반복해서 시작 인덱스 값을 증가시킵니다. (pivot 값보다 큰데 좌측에 있는 값을 찾기 위해)
    - 끝 인덱스(high)가 가리키는 값과 pivot 값을 비교해서 더 작은 경우 반복해서 끝 인덱스 값을 감소시킵니다. (pivot 값보다 작은데 우측에 있는 값을 찾기 위해)
    - 두 인덱스가 아직 서로 교차해서 지나치치 않았다면 시작 인덱스(low)가 가리키는 값과 끝 인덱스(high)가 가리키는 값을 상호 교대(swap) 시킵니다. (잘못된 위치에 있는 두 값의 위치를 바꾸기 위해)
    - 상호 교대 후, 다음 값을 가리키기 위해 두 인덱스를 각자 진행 방향으로 한 칸씩 이동 시킵니다.
- 두 인덱스가 서로 교차해서 지나치게 되어 while 루프를 빠져나왔다면 다음 재귀 호출의 분할 기준점이될 시작 인덱스를 리턴합니다.