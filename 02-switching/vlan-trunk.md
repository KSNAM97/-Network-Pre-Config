# VLAN / Trunk Pre-Config

## 설명
스위치에서 VLAN 생성, Access Port 설정, Trunk 구성 시 사용하는 기본 템플릿입니다.

VLAN 기반 네트워크 구성과 스위치 간 VLAN 전달을 위한 가장 기본적인 설정입니다.

---

## 핵심 포인트

- VLAN 생성
- Access Port 할당
- 802.1Q Trunk 구성
- Allowed VLAN 제한 가능

---

## Pre-Config

```cisco
configure terminal
vlan <VLAN_ID_1>
 name <VLAN_NAME_1>
vlan <VLAN_ID_2>
 name <VLAN_NAME_2>
!
interface <ACCESS_PORT_1>
 switchport mode access
 switchport access vlan <VLAN_ID_1>
!
interface <ACCESS_PORT_2>
 switchport mode access
 switchport access vlan <VLAN_ID_2>
!
interface <TRUNK_PORT>
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan <VLAN_LIST>
end
write memory
```

---

## Example

```cisco
configure terminal
vlan 100
 name CCNA
vlan 200
 name CCNP
!
interface e0/0
 switchport mode access
 switchport access vlan 100
!
interface e0/1
 switchport mode access
 switchport access vlan 200
!
interface e0/2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 100,200
end
write memory
```

---

## Verification

```cisco
show vlan brief
show interfaces trunk
show running-config interface <TRUNK_PORT>
```

---

## Notes

- 환경에 따라 `switchport trunk encapsulation dot1q` 명령이 없는 IOS도 있습니다.
- Trunk 포트는 허용 VLAN을 제한해서 불필요한 VLAN 전파를 줄일 수 있습니다.
```
