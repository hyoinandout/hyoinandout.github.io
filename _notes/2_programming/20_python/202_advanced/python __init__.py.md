---
---

패키지임을 알 수 있게 해줌. (패키지: 특정 기능을 하는 파일을 모아 놓은것)
파이썬 버전에 따라 없어도 되지만 있는게 좋은데, 가끔 ModuleNotFoundError 같은게 뜨기 때문. 예를 들어 pytest 돌릴 때 이건 python3 -m pytest랑 같은데 이 때 PYTHONPATH에 따라 tests 폴더를 인식 못하는 경우가 있음. 이런 경우를 대비해서 tests 폴더에 init.py를 생성해주면 해결된다.