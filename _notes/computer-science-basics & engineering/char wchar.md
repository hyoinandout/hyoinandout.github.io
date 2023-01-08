---
---

python에서 ctype을 쓰던 중 ctype의 변수 타입 중 제목의 두가지가 있었다. 차이점을 알아보자.

wchar = wide character

우선 ascii code (1바이트, 0-127), unicode (2바이트)가 있다.
char <-> ascii, wchar <-> uni

t = translation의 t라고 생각하시는 분도 있는 듯 하다.

아무튼 wchar_t는 명백히 utf-16 인코딩 되는 문자열 다룰 때 사용

유니코드에서:
그렇다면 UTF-8/16 의 차이는? 문자 하나를 표현할 때 사용할 최소 바이트수 차이이다.(8/16 bit) 
![Untitled](/assets/utf.png)
출처 나무위키