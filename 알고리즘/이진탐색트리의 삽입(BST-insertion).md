![[Pasted image 20230919171513.png]]

위 이미지 트리에서 7과 3사이에 4를 추가해도 이진탐색트리의 속성이 깨지지 않음을 확인할 수 있다. 하지만 이진탐색트리가 커질 경우 이렇게 트리의 중간에 새 데이터를 삽입하게 되면 서브트리의 속성이 깨질 수 있기 때문에 삽입 연산은 반드시 리프노드에서 이뤄지게 된다.

#### 이진탐색트리 삽입의 특징
- 이진탐색트리의 가장 왼쪽 리프노드는 해당 트리의 최소값, 제일 오른쪽 리프노드는 최대값이 된다. 만약 위 트리에서 100을 추가하려고 한다면 제일 오른쪽 잎새노드의 오른쪽 자식노드를 만들고 여기에 붙이면 된다.


###  삽입 / Insert Method

BinarySearchTree에 `insert()` Method를 구현하여 트리에 새 원소를 추가한다. 재귀를 아용해서 구현하면 간단하다. 새로 추가할 원소의 값을 현재 노드의 값과 비교하여 왼쪽/오른쪽 중 알맞은 위치로 노드를 옮겨가면서 삽입 위치를 확인한다.

```python
class Node(object):
    def __init__(self, data):
        self.data = data
        self.left = self.right = None


class BinarySearchTree(object): 
	def __init__(self): # 생성자 초기화
		self.root = None

	def insert(self, data):
		self.root = self._insert_value(self.root, data)     
		return self.root is not None # 루트노드가 None이 아니면 반환
		
	def _insert_value(self, node, data):        
		if node is None:            
			node = Node(data)        
		else:            
			if data <= node.data:     
			    node.left = self._insert_value(node.left, data)
			else:                
				node.right = self._insert_value(node.right, data)
			
		return node
```