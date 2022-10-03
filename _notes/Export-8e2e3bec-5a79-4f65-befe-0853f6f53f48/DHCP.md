---
title: "DHCP"
---

# DHCP

Dynamic Host Configuration Protocol (BOOTP 프로토콜 기반)

↔ 수동으로 IP와 네트워크 정보를 직접 설정하는 것을 ‘정적 할당’

KT는 왜 DHCP 데이터를 가지고 있을까?) 핸드폰이나 PC를 사용할 때, IP를 별도로 설정하지 않아도 네트워크에 쉽게 접속되므로 특별한 경우를 제외하고 동적 할당 방식을 기본으로 사용.

![Untitled](DHCP%20e4065d730f4441098975dc74f9995e59/Untitled.png)

**1.** **DHCP** **Discover**

DHCP 클라이언트는 DHCP 서버를 찾기 위해 DHCP Discover 메시지 브로드캐스트

(브로드캐스트 : 같은 서브넷에 있는 모든 클라이언트들이 메시지를 받게 전송한다는 뜻)

**2.** **DHCP** **Offer**

DHCP 서버는 클라이언트에 할당할 IP 주소와 서브넷, 게이트웨이, DNS 정보, Lease Time 등의 정보를 포함한 DHCP 메시지를 클라이언트로 전송

**3.** **DHCP** **Request**

DHCP 서버로부터 제안받은 IP 주소(Requested IP)와 DHCP 서버 정보(DHCP Server Identifier)를 포함한 DHCP 요청 메시지를 브로드캐스트 (2개 이상의 DHCP 서버로부터 Offer가 오면 특별한 알고리즘은 없고 특정 설정을 적용하여 배정 요청함)

유니 캐스트로 보내도 되는데 브로드캐스트로 보내는 이유: IP를 설정하고 유니캐스트로 패킷을 전달해도 되지만 DHCP 서버 여러 대가 동작하는 환경을 위해 브로드캐스트를 사용

**4.** **DHCP** **Acknowledgement**

DHCP 클라이언트로부터 IP 주소를 사용하겠다는 요청을 받으면 DHCP 서버에 해당 IP를 어떤 클라이언트가 언제부터 사용하기 시작했는지 정보를 기록하고 DHCP Request 메시지를 정상적으로 수신했다는 응답을 전송

![Untitled](DHCP%20e4065d730f4441098975dc74f9995e59/Untitled%201.png)

IP 임대시간이 있다. 임대시간 지나면 재할당 받아야 하지만, 갱신을 통해 임대받은 IP를 계속 사용할 수 있다. 이 때 유니캐스트로 진행되어 불필요한 브로드캐스트 발생 X

- 공격
    - DHCP Starvation
        - 공격자가 DHCP서버의 IP할당대역을 모두 할당받아오면 일반 사용자가 DHCP서버에게 할당요청할때 할당을 해줄 수 없음 → request가 너무 많으면 의심할 필요가 있다
        - Port-security로 방어
    - DHCP Spoofing → ManInTheMiddle 공격 가능
        
        ![Untitled](DHCP%20e4065d730f4441098975dc74f9995e59/Untitled%202.png)
        
        - 공격자가 DHCP를 위장하여 조작된 정보를 전송하는것. → request에 비해 offer가 너무 적으면 의심할 필요가 있다
        - DHCP snooping으로 방어(port security와 비슷한 개념)

[^1]: [https://thebook.io/007046/ch07/04/02-01/](https://thebook.io/007046/ch07/04/02-01/)

[^2]: [https://datatracker.ietf.org/doc/html/rfc2131](https://datatracker.ietf.org/doc/html/rfc2131)