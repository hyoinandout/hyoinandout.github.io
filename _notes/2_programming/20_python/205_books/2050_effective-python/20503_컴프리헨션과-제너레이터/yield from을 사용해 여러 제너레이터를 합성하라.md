
```python
def move(period, speed):
	for _ in range(period):
		yield speed

def pause(delay):
	for _ in range(delay):
		yield 0
```

AS-IS
```python
#
def animate():
	for delta in move(4, 5.0):
		yield delta
	for delta in pause(3):
		yield delta
	for delta in move(2, 3.0):
		yield delta

def render(delta):
	print(f'Delta: {delta:.1f}')
# 화면에서 이미지를 이동시킨다  

def run(func):
	for delta in func():
		render(delta)

run(animate)
```

move(yield, generator) ->  animate(iterator, yield, generator) -> run(iterator)

TO-BE
```python
def animate_composed():
	yield from move(4, 5.0)
	yield from pause(3)
	yield from move(2, 3.0)

run(animate_composed)
```
move(yield, generator) ->  animate_composed(~~iterator~~, yield from) -> run(iterator)


yield from 은 for 루프 내포시키고 yield 식을 처리하도록 하지만, python-native하므로 조금 더 빠르다.

