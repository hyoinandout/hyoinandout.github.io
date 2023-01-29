---
---

#linux

Screen is a full-screen window manager that multiplexes a physical terminal between several processes (typically interactive shells). 

여러 개의 터미널로 분화시켜주는 역할

보통 vm에서 계속 돌려야 하는 것이 있을 때 사용 ex) db에 web UI로 접근해서 무언가를 하면 컴퓨터가 꺼질 때 connection이 끊어지면서 rollback이 될 수 있는데, screen 통해서 하면 그럴 염려가 없음

- screen -S ${name}: ${name}으로 된 screen 생성
- screen -ls, screen -list: 현재 screen 뭐있는지
- screen -r/-x ${name}: detach 된 screen에 다시 들어가기 

들어가서
- ctrl + a + c: 터미널 하나 더 만들기
- ctrl + a 0,1,2,3,4: 0번, 1번, 2번, 3번, 4번 터미널로 들어가기
- ctrl + a + a: 바로 전 창으로
- ctrl + a + d: detach