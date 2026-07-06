# OSPF Multi-Area Pre-Config

## 설명
Multi-Area OSPF 환경에서 사용하는 기본 설정 템플릿입니다.

Area 0(Backbone)과 비-백본 Area를 연결하는 ABR 환경에서 자주 사용됩니다.

---

## 핵심 포인트

- `router-id` 설정
- `passive-interface default`
- Area별 네트워크 분리
- ABR 구조 실습에 적합

---

## Pre-Config

```cisco
configure terminal
router ospf <PROCESS_ID>
 router-id <ROUTER_ID>
 passive-interface default
 no passive-interface <AREA0_IF>
 no passive-interface <AREA100_IF>
 no passive-interface <AREA210_IF>
 network <AREA0_NET> <WILDCARD> area 0
 network <AREA100_NET> <WILDCARD> area 100
 network <AREA210_NET> <WILDCARD> area 210
 network <LOOPBACK_IP> 0.0.0.0 area <AREA_ID>
end
write memory
```

---

## Example

```cisco
configure terminal
router ospf 100
 router-id 11.11.11.11
 passive-interface default
 no passive-interface s1/0
 no passive-interface s1/1
 network 210.116.41.32 0.0.0.3 area 100
 network 210.116.41.36 0.0.0.3 area 0
 network 11.11.11.11 0.0.0.0 area 100
end
write memory
```

---

## Verification

```cisco
show ip ospf neighbor
show ip ospf interface brief
show ip ospf database
show ip route ospf
show ip protocols
```

---

## Notes

- Area 0는 Backbone Area로 유지해야 합니다.
- Multi-Access 환경에서는 DR/BDR 선출도 함께 확인하는 것이 좋습니다.
```
