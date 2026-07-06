# Redistribution Pre-Config

## 설명
서로 다른 라우팅 프로토콜 간 경로를 교환하기 위한 Redistribution 설정 템플릿입니다.

EIGRP ↔ OSPF, Static ↔ Dynamic Routing 환경에서 사용합니다.

---

## 핵심 포인트

- 프로토콜 간 경로 재분배
- Seed Metric 설정 필요
- 재분배 후 라우팅 테이블 변화 확인

---

## Pre-Config

```cisco
configure terminal
router ospf <OSPF_PROCESS_ID>
 redistribute eigrp <EIGRP_ASN> subnets
!
router eigrp <EIGRP_ASN>
 redistribute ospf <OSPF_PROCESS_ID> metric <BW> <DELAY> <RELIABILITY> <LOAD> <MTU>
end
write memory
```

---

## Example

```cisco
configure terminal
router ospf 100
 redistribute eigrp 100 subnets
!
router eigrp 100
 redistribute ospf 100 metric 100000 100 255 1 1500
end
write memory
```

---

## Verification

```cisco
show ip route
show ip protocols
show running-config | section router
```

---

## Notes

- EIGRP는 Redistribution 시 Metric 값을 직접 지정해야 합니다.
- OSPF는 `subnets` 키워드를 빠뜨리면 세부 경로가 광고되지 않을 수 있습니다.
- 재분배 루프를 방지하기 위한 Route-Map 설계가 필요할 수 있습니다.
```
