
각 노드가 최대 두개의 값을 가질 수 있다.

[[이진탐색트리(Binary Search Tree)]]와 크게 다른 점은 노드의 삽입이 있다는 점이다.


기본 노드 선언
```python

class Node: # 루트노드가 정해져 있고 코드 내부에 정보가 입력되어 있을 때
    def __init__(self, root):  
        self.root = root  
        self.left = None
        self.right = None


class Node: # 루트노드가 정해지지 않고 사용자가 모든 정보를 입력할 때
    def __init__(self, root, left=None, right=None):  
        self.root = root  
        self.left = left  
        self.right = right

```

