---
---

# 따배쿠

- 컨테이너, 도커
- 가상머신 vs 컨테이너
    - 가상머신: 하드웨어를 효율적으로 쓸 수 있음, 수직적 확장
    - 컨테이너: 더 가벼움. 확장도 빠름 (배포용)
- 멀티 호스트 도커 플랫폼 : 일일이 관리 어려움
→ 오케스트레이션
- 선언적 API
- 설치 툴
    - kubeadm
    - kubespray
- CNI
    - container network interface
    - container간 통신을 지원하는 VxLAN. Pod Network
    - 다양한 종류의 플러그 인’
- control plane, worker node