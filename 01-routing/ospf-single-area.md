# OSPF Single-Area Pre-Config

## 설명
단일 Area(일반적으로 Area 0) 기반 OSPF 설정 템플릿입니다.

작은 규모의 OSPF 실습이나 기본 Neighbor 형성 실습에 적합합니다.

---

## 핵심 포인트

- `router-id` 설정
- `passive-interface default`
- 필요한 인터페이스만 활성화
- Area 0 광고

---

## Pre-Config

```cisco
configure terminal
router ospf <PROCESS_ID>
 router-id <ROUTER_ID>
 passive-interface default
 no passive-interface <INTERFACE_1>
 no passive-interface <INTERFACE_2>
 network <NET_1> <WILDCARD_1> area 0
 network <NET_2> <WILDCARD_2> area 0
 network <LOOPBACK_IP> 0.0.0.0 area 0
end
write memory
```

---

## Example

```cisco
configure terminal
router ospf 100
 router-id 1.1.1.1
 passive-interface default
 no passive-interface s1/0
 no passive-interface fa0/0
 network 210.116.41.32 0.0.0.3 area 0
 network 168.126.63.0 0.0.0.255 area 0
 network 1.1.1.1 0.0.0.0 area 0
end
write memory
```

---

## Verification

```cisco
show ip ospf neighbor
show ip ospf interface brief
show ip route ospf
show ip protocols
```

---

## Notes

- Router-ID는 Loopback과 함께 사용하는 것이 일반적입니다.
- Loopback 광고 시 `0.0.0.0` wildcard를 자주 사용합니다.
```
