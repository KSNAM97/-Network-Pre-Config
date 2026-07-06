# EtherChannel L2 Pre-Config

## 설명
L2 환경에서 여러 개의 스위치 포트를 하나의 논리 링크로 묶는 EtherChannel 템플릿입니다.

Trunk 기반 L2 Aggregation 실습에서 사용합니다.

---

## 핵심 포인트

- PAgP / LACP 사용 가능
- 묶기 전 포트 설정 일치 필요
- Trunk 기반 Port-Channel 구성

---

## Pre-Config

```cisco
configure terminal
interface range <PORT_RANGE>
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group <GROUP_ID> mode <MODE>
!
interface port-channel <GROUP_ID>
 switchport trunk encapsulation dot1q
 switchport mode trunk
end
write memory
```

---

## Example

```cisco
configure terminal
interface range e0/0 - 1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 1 mode desirable
!
interface port-channel 1
 switchport trunk encapsulation dot1q
 switchport mode trunk
end
write memory
```

---

## Verification

```cisco
show etherchannel summary
show interfaces trunk
show running-config interface port-channel <GROUP_ID>
```

---

## Notes

- PAgP: `desirable / auto`
- LACP: `active / passive`
- 양쪽 장비의 속도, 듀플렉스, trunk/access 모드가 일치해야 합니다.
```
