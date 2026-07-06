# Loopback Pre-Config

## 설명
라우터 식별, Router-ID 고정, 테스트용 네트워크 광고를 위해 사용하는 Loopback 인터페이스 설정입니다.

EIGRP, OSPF, Multi-ISP, DMVPN 실습에서 반복적으로 사용됩니다.

---

## 핵심 포인트

- Router-ID 고정용
- 테스트용 목적지 네트워크 생성
- 라우팅 광고 대상 네트워크로 활용

---

## Pre-Config

```cisco
configure terminal
interface loopback0
 ip address <LOOPBACK0_IP> <MASK>
 no shutdown
!
interface loopback1
 ip address <LOOPBACK1_IP> <MASK>
 no shutdown
end
write memory
```

---

## Example

```cisco
configure terminal
interface loopback0
 ip address 11.11.11.11 255.255.255.0
 no shutdown
!
interface loopback1
 ip address 131.116.1.1 255.255.255.0
 no shutdown
end
write memory
```

---

## Verification

```cisco
show ip interface brief
show running-config | section interface Loopback
```

---

## Notes

- OSPF에서는 Router-ID 고정용으로 Loopback0를 자주 사용합니다.
- EIGRP에서는 테스트망 광고용으로 Loopback1, Loopback2를 함께 사용하는 경우가 많습니다.
```
