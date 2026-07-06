# EIGRP Pre-Config

## 설명
EIGRP 기본 라우팅 설정 템플릿입니다.

EIGRP Neighbor를 형성하고, 내부 네트워크와 Loopback 네트워크를 광고할 때 사용합니다.

---

## 핵심 포인트

- `no auto-summary`
- `passive-interface default`
- 필요한 인터페이스만 활성화
- 내부망 + Loopback 광고

---

## Pre-Config

```cisco
configure terminal
router eigrp <ASN>
 no auto-summary
 passive-interface default
 no passive-interface <INTERFACE_1>
 no passive-interface <INTERFACE_2>
 network <NET_1> <WILDCARD_1>
 network <NET_2> <WILDCARD_2>
 network <LOOPBACK_NET> <WILDCARD_3>
end
write memory
```

---

## Example

```cisco
configure terminal
router eigrp 100
 no auto-summary
 passive-interface default
 no passive-interface s1/0
 no passive-interface fa0/0
 network 121.160.11.64 0.0.0.3
 network 168.110.21.0 0.0.0.63
 network 123.123.123.0 0.0.0.255
end
write memory
```

---

## Verification

```cisco
show ip eigrp neighbors
show ip eigrp topology
show ip route eigrp
show ip protocols
```

---

## Notes

- VLSM 환경에서는 반드시 `no auto-summary`를 적용합니다.
- 불필요한 Hello 전송을 막기 위해 `passive-interface default` 후 필요한 인터페이스만 해제합니다.
```
