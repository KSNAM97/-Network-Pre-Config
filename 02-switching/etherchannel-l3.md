# EtherChannel L3 Pre-Config

## 설명
L3 환경에서 Routed Port 기반으로 EtherChannel을 구성할 때 사용하는 템플릿입니다.

스위치 간 L3 연결 또는 L3 백본 구간에서 사용할 수 있습니다.

---

## 핵심 포인트

- `no switchport` 사용
- L3 Port-Channel에 IP 직접 할당
- Routed Port 기반 Aggregation

---

## Pre-Config

```cisco
configure terminal
interface range <PORT_RANGE>
 no switchport
 channel-group <GROUP_ID> mode <MODE>
!
interface port-channel <GROUP_ID>
 no switchport
 ip address <PORT_CHANNEL_IP> <MASK>
 no shutdown
end
write memory
```

---

## Example

```cisco
configure terminal
interface range e0/0 - 1
 no switchport
 channel-group 10 mode active
!
interface port-channel 10
 no switchport
 ip address 121.160.10.2 255.255.255.0
 no shutdown
end
write memory
```

---

## Verification

```cisco
show etherchannel summary
show ip interface brief
show running-config interface port-channel <GROUP_ID>
```

---

## Notes

- L3 EtherChannel은 `switchport`가 아닌 Routed Port 기반입니다.
- Port-Channel 인터페이스에 직접 IP를 설정합니다.
```
