---
---

간단한 것은 내장 클래스 상속 받으면 된다
ex)
```python
class FrequencyList(list):
	def __init__(self, members):
		super().__init__(members)  
	
	def frequency(self):
		counts = {}
		for item in self:
			counts[item] = counts.get(item, 0) + 1
		return counts
```

근데 사용자 수요에 따라 이걸로 해결 안되는 경우도 있다
ex)B-Tree를 list로 표현하고 싶은데 이걸 단순히 트리/각 노드가 list를 상속받는다고 해서 해결될 것 같지 않다
->
특별한 인스턴스 메소드를 사용해 컨테이너의 동작을 구현하는데, 여기서는 \_\_getitem\_\_()을 구현해주자.

B-Tree ex)
![](/assets/b-tree.jpg)

```python
class IndexableNode(BinaryNode):
	def _traverse(self):
		if self.left is not None:
			yield from self.left._traverse()
		yield self
		if self.right is not None:
			yield from self.right._traverse()
	
	def __getitem__(self, index):
		for i, item in enumerate(self._traverse()):
			if i == index:
				return item.value
		raise IndexError(f'인덱스 범위 초과: {index}')
```

```python
print('LRR:', tree.left.right.right.value)  # 7
print('인덱스 0:', tree[0])  # 2
print('인덱스 1:', tree[1])  # 5
print('11이 트리 안에 있나?', 11 in tree)  # True
print('17이 트리 안에 있나?', 17 in tree)  # False
print('트리:', list(tree))  # [2,5,6,7,10,11,15]

print('길이:', len(tree))  # X
```

len이 구현 안되어 있어서 에러가 나오는데, 그렇다면 매번 다 특별한 메소드 구현?
X -> collections.abc를 상속하라!
인터페이스만 구현하면 그 뒤로는 자유롭게 사용 가능.

