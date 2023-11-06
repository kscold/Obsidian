삭제 연산은 탐색, 삽입보다는 약간 복잡하다. 삭제 결과로 자칫 이진탐색트리의 속성이 깨질 수 있기 때문이다. 따라서 가능한 세 가지 경우의 수를 모두 따져봐야한다. 먼저 삭제할 노드에 자식노드가 없는 경우(리프노드라면)(case 1)이다. 이 케이스라면 아래 이미지와 같이 해당 노드를 그냥 없애기만 하면 된다.

[![](https://i.imgur.com/d8sOy3z.png "source: imgur.com")]

이번엔 삭제할 노드에 자식노드가 하나 있는 경우(case 2)이다. 이 케이스라면 해당 노드를 지우고, 해당 노드의 자식노드와 부모노드를 연결해주면 된다. 아래 이미지는 트리에서 20을 삭제한다고 가정했을 때의 예시이다.

[![](https://i.imgur.com/RqCRxO9.png "source: imgur.com")](https://imgur.com/RqCRxO9)

20을 루트노드로 하는 서브트리의 모든 값은 20의 부모노드인 30보다 작거나 같다. 이진탐색트리의 속성 때문이다. 따라서 20을 지우고, 20의 하나뿐인 자식노드(25)와 부모노드(30)를 연결해도 이진탐색트리의 속성이 깨지지 않는 걸 확인할 수 있다.

마지막으로 삭제할 노드에 자식노드가 두 개 있는 경우(case 3)이다. 아래 트리에서 16을 삭제해야 한다고 가정했을 때, 기존처럼 16을 무작정 지우게 되면 13의 위치가 애매해진다. 계산복잡성을 줄이기 위해서는 트리의 요소값들을 크게 바꾸지 않고 원하는 값(16)만 삭제할 수록 좋다.

[![](https://i.imgur.com/yKzDYZu.png "source: imgur.com")](https://imgur.com/yKzDYZu)

16을 삭제하기 전 위 트리 각 요소를 중위순회 방식(왼쪽 서브트리-노드-오른쪽 서브트리 순으로 순회)으로 읽어본다. 다음과 같이 정렬된 순으로 나타나 이진탐색트리 속성을 만족하고 있음을 확인할 수 있다.

중위 순회로 읽었을때 이진탐색트리는 오름차순 정렬이 된다.
> 4, 10, 13, 16, 20, 22, 25, 28, 30, 42

이 리스트와 예시 그림을 보면 16의 왼쪽 서브트리에 속한 모든 값은 16보다 작고, 오른쪽 서브트리에 속한 모든 값은 16보다 큰 것을 확인할 수 있습니다. 특히 13을 predecessor(삭제 대상 노드의 왼쪽 서브트리 가운데 최대값), 20을 successor(삭제 대상 노드의 오른쪽 서브트리 가운데 최소값)라고 합니다. 트리를 중위순회 방식으로 늘여뜨려 표시하면 16 바로 앞에 13이 있고, 바로 뒤에 20이 있기 때문에 각각 이런 이름이 붙은 것 같다.

따라서 아래와 같이 삭제할 노드인 16 위치에 20을 복사해 놓고, 기존 20 위치에 있던 노드를 삭제하게 되면 정렬된 순서를 유지(=이진탐색트리 속성을 만족)하면서도 원하는 결과를 얻을 수 있게 된다. 이는 위 그림에서도 확인할 수 있다. (물론 16 위치에 predecessor인 13을 놓고, 기존 13 위치에 있던 노드를 삭제해도 원하는 결과를 얻을 수 있습니다)

> 4, 10, 13, ~~16~~ **20**, ~~20~~, 22, 25, 28, 30, 42

이진탐색트리 구조상 successor(삭제 대상 노드의 오른쪽 서브트리의 최소값)는 자식노드가 하나이거나, 하나도 존재하지 않습니다. 각각 살펴보면 다음과 같습니다.

- successor의 자식노드가 하나인 케이스 : 위 예시 그림과 같습니다. 삭제 대상 노드의 오른쪽 서브트리가 30을 루트노드로 하는 트리일 때, 이 트리의 맨 왼쪽 노드인 20은 하나의 자식노드(25)를 갖습니다.
- successor의 자식노드가 존재하지 않는 케이스 : 삭제 대상 노드의 오른쪽 서브트리가 아래 그림과 같을 때에는 successor는 자식노드를 가지지 않습니다.

[![](https://i.imgur.com/po0R4GB.png "source: imgur.com")](https://imgur.com/po0R4GB)

마찬가지로 왼쪽 서브트리의 맨 오른쪽 노드인 predecessor 또한 자식노드가 하나이거나, 하나도 존재하지 않습니다. 따라서 자식노드가 두 개인 경우(case 3)에는 다음과 같이 삽입 연산을 수행하면 됩니다(successor 기준).

1. 삭제 대상 노드의 오른쪽 서브트리를 찾는다.
2. successor(1에서 찾은 서브트리의 최소값) 노드를 찾는다.
3. 2에서 찾은 successor의 값을 삭제 대상 노드에 복사한다.
4. successor 노드를 삭제한다.

4번 successor 노드를 삭제하는 과정은 case 1나 case2에 해당합니다. 이미 언급했듯이 successor는 자식노드가 하나이거나, 하나도 존재하지 않기 때문입니다.

이번엔 삽입연산의 계산복잡성을 따져 보겠습니다. Big-O notation으로는 최악의 케이스를 고려해야 하므로 가장 연산량이 많은 case 3(삭제 대상 노드의 자식노드가 두 개인 경우)이 분석 대상입니다.

트리의 높이가 ℎℎ이고 삭제대상 노드의 레벨이 𝑑�라고 가정해 보겠습니다. 1번 과정에서는 𝑑�번의 비교 연산이 필요합니다. 2번 successor 노드를 찾기 위해서는 삭제 대상 노드의 서브트리 높이(ℎ−𝑑ℎ−�)에 해당하는 비교 연산을 수행해야 합니다. 3번 4번은 복사하고 삭제하는 과정으로 상수시간이 걸려 무시할 만 합니다. 종합적으로 따지면 𝑂(𝑑+ℎ−𝑑)�(�+ℎ−�), 즉 𝑂(ℎ)�(ℎ)가 됩니다.


```python
class BinarySearchTree(object):
    ...
    def delete(self, key):
        self.root, deleted = self._delete_value(self.root, key)
        return deleted
 
    def _delete_value(self, node, key):
        if node is None:
            return node, False
 
        deleted = False # deleted 초기화
        if key == node.data: # 삭제 하고 싶은 값과 노드의 값이 같으면
            deleted = True # deletd True로 설정
            if node.left and node.right: # 왼쪽노드와 오른쪽 노드 둘 다 존재
                # replace the node to the leftmost of node.right
                parent, child = node, node.right
                while child.left is not None: # child의 왼쪽노드가 존재하면
                    parent, child = child, child.left
                child.left = node.left # child.left노드에 node.left를 대입
                # 한쪽으로 모는 것으로 볼 수 있음
                
                if parent != node:
                    parent.left = child.right
                    child.right = node.right
                node = child
            elif node.left or node.right:
                node = node.left or node.right
            else:
                node = None
        elif key < node.data:
            node.left, deleted = self._delete_value(node.left, key)
        else:
            node.right, deleted = self._delete_value(node.right, key)
        return node, deleted
```

이진 트리의 좌우 균형이 맞는다면 탐색, 삽입, 삭제의 시간복잡도가 `O(logn)`이다. 그러나 이진 탐색 트리는 정렬된 데이터에는 취약하다. 오름차순이든 내림차순이든 정렬된 데이터가 입력되면 한 쪽으로 치우친(`skewed`) 트리가 만들어진다. 이 때, 최악의 경우 모든 데이터를 살펴야 할 수도 있어 시간복잡도가 `O(n)`이 된다.