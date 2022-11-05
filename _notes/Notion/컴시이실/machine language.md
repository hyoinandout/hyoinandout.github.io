---
---

# machine language

syntax를 알고 해석할 줄 알아야함

예시 문제 C코드로 generate

conditional branches-

condition code (Test, compare) → condition code register

우리가 다루는 4가지 condition code

loop

switch - jump table

first six arguments - rdi rsi rdx …

ja : check greater than 6 or less than 0

100부터 시작하면? shift

sparse : binary tree(may use decision trees)

```cpp
void decode(long *xp, long *yp, long * zp)
{
	long a = *xp;
	long b = *yp;
	long c = *zp;
	*yp = a;
	*zp = b;
	*xp = c;
}
```