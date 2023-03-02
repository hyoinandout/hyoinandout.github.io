예제: A * _sin_(2pi/steps)

기존 제너레이터에 아쉬운 점:
제너레이터가 데이터를 내보내면서 다른 데이터를 받아들일 때 직접 쓸 수 있는 방법이 없는 것 처럼 보인다.

ex)
```python
def wave(amplitude, steps):
	step_size = 2 * math.pi / steps # 2라디안/단계 수
	for step in range(steps):
		radians = step * step_size
		fraction = math.sin(radians)
		output = amplitude * fraction
		yield output
```

amplitude를 바꾸고 싶은데, 호출부에서 바꿀 방법이 없을까? -> send

```python
def my_generator():
	received = yield 1
	print(f'받은 값 = {received}')

it = iter(my_generator())
output = next(it) # 첫 번째 제너레이터 출력을 얻는다
print(f'출력값 = {output}')

try:
	next(it) # 종료될 때까지 제너레이터를 실행한다
except StopIteration:
	pass
```

결과 : 출력 1, 받은 값 None

일반적으로 제너레이터를 이터레이션할때 yield는 None을 반환
try-except 문에서 my_generator function에서 yield 다음 문장이 실행

```python
#
it = iter(my_generator())
output = it.send(None) # 첫 번째 제너레이터 출력을 얻는다
print(f'출력값 = {output}')

try:
	it.send('안녕!') # 값을 제너레이터에 넣는다
except StopIteration:
	pass
```


결과 : 출력 1, 받은 값 안녕!
처음에 None을 보내주는 이유는 제너레이터가 아직 yield에 도착하지 않은 상태기 때문.
send None -> yield 1 -> send(안녕) -> 받은 값 안녕

그러나 이해하기 어렵다(# 54)
yield from도 이 문제를 해결해주진 않는다.
yield와 send를 쓸 때 문제점이 있는데,
넘어갈 때 내포된 제너레이터들이 send로부터 값을 받기 위해 None을 리턴하는 단순한 yield로 시작한다는 점이다.(# 84)

해결책: 이터레이터 전달(리스트, 등)
제너레이터를 사용해 이터레이터를 만든 경우에도 작동하지만, thread-safe한지는 미지수이다.

-> generator object 자체는 GIL에 의해서 thread-safe 하지만,
동시다발적인 next가 각각 다른 스레드에서 호출된다면 상태가 보전되겠는가?

But the thread trying to get the next element from the generator which is already in execution state in other thread (executing the generator code between the `yield`'s) would get ValueError:
```python
ValueError: generator already executing
```

ref)https://stackoverflow.com/questions/1131430/are-generators-threadsafe/1133605#1133605
