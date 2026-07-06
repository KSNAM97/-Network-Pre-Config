# SVI / Inter-VLAN Routing Pre-Config

## 설명
L3 Switch에서 SVI를 이용해 VLAN 간 라우팅을 구성할 때 사용하는 템플릿입니다.

각 VLAN에 게이트웨이 역할을 하는 SVI를 만들고, `ip routing`으로 L3 기능을 활성화합니다.

---

## 핵심 포인트

- `ip routing` 활성화
- VLAN별 SVI 생성
- VLAN Gateway IP 설정
- Inter-VLAN Routing 구성

---

## Pre-Config

```cisco
configure terminal
ip routing
!
interface vlan <VLAN_ID_1>
 ip address <VLAN1_GATEWAY> <MASK>
 no shutdown
!
interface vlan <VLAN_ID_2>
 ip address <VLAN2_GATEWAY> <MASK>
 no shutdown
end
write memory
```

---

## Example

```cisco
configure terminal
ip routing
!
interface vlan 100
 ip address 13.13.100.1 255.255.255.0
 no shutdown
!
interface vlan 200
 ip address 13.13.200.1 255.255.255.0
 no shutdown
!
interface vlan 130
 ip address 13.13.30.1 255.255.255.0
 no shutdown
end
write memory
```

---

## Verification

```cisco
show ip interface brief
show ip route
show running-config | section interface Vlan
```

---

## Notes

- SVI가 up/up 되려면 해당 VLAN이 존재하고, 활성 포트가 있어야 합니다.
- L3 Switch에서만 `ip routing`을 사용합니다.
```
