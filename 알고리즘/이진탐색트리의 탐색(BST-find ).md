
### 탐색 / Find Method

원하는 값의 존재유무를 확인할 수 있도록, `find()` Method를 구현한다. 재귀와 값의 대소관계 비교를 통해 구현할 수 있다.

```python

# 노드 클래스 선언이 되어 있다고 가정
class BinarySearchTree(object):
    ...
    # 생성자 초기화와 트리가 생성되어 있다고 가정
    
    def find(self, key):
        return self._find_value(self.root, key)
 
    def _find_value(self, root, key):
        if root is None or root.data == key:
            return root is not None
        elif key < root.data:
            return self._find_value(root.left, key)
        else:
            return self._find_value(root.right, key)
```