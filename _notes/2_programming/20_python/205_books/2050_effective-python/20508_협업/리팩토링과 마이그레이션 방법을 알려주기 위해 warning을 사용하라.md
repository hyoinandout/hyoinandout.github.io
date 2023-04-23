API 변경 -> 사람들에게 나의 코드를 리팩터링하고 나의 API를 사용하는 부분을 최신 API에 맞춰 변경할 수 있도록 협력을 요청하는 방법 -> warnings 모듈 사용

warnings module:
1. stack level이라는 parameter를 통해 어디서 경고를 발생시켰는지 알 수 있다. /ex4
2. 경고가 발생할 때 해야할 작업도 설정을 할 수 있다. -> xfail test /ex5
3. -W error flag를 통해, 경고를 예외로 간주 시킬 수 있음. /ex6
4. 프로덕션 배포 후 : 경고로 오류를 유발하는 대신, logging 내장 모듈에 복제하기 /ex8



