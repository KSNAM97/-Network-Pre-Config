# EIGRP Summary Pre-Config

## 설명
EIGRP 환경에서 Manual Route Summarization을 적용하는 템플릿입니다.

여러 개의 세부 경로를 하나의 요약 경로로 광고하여 라우팅 테이블을 단순화할 수 있습니다.

---

## 핵심 포인트

- 인터페이스 단위 요약
- 라우팅 테이블 최적화
- EIGRP Summary Route AD = 5

---

## Pre-Config

```cisco
configure terminal
interface <OUTBOUND_INTERFACE>
 ip summary-address eigrp <ASN> <SUMMARY_NETWORK> <SUMMARY_MASK>
end
write memory
```

---

## Example

```cisco
configure terminal
interface s1/1
 ip summary-address eigrp 100 131.116.0.0 255.255.248.0
end
write memory
```

---

## Verification

```cisco
show ip protocols
show ip route eigrp
show running-config interface <OUTBOUND_INTERFACE>
```

---

## Notes

- 요약은 광고가 나가는 인터페이스 기준으로 적용합니다.
- 잘못 요약하면 블랙홀 경로가 생길 수 있으므로 범위를 정확히 계산해야 합니다.
```
