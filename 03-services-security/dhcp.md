# DHCP Pre-Config

## 설명
라우터 또는 L3 장비에서 DHCP 서비스를 구성할 때 사용하는 기본 템플릿입니다.

내부 호스트에 자동으로 IP, Gateway, DNS 정보를 할당할 때 사용합니다.

---

## 핵심 포인트

- 제외 주소 설정
- DHCP Pool 생성
- Gateway / DNS / Lease 설정

---

## Pre-Config

```cisco
configure terminal
ip dhcp excluded-address <EXCLUDED_START> <EXCLUDED_END>

ip dhcp pool <POOL_NAME>
 network <NETWORK> <MASK>
 default-router <DEFAULT_GATEWAY>
 dns-server <DNS_SERVER>
 lease <DAYS>
end
write memory
```

---

## Example

```cisco
configure terminal
ip dhcp excluded-address 13.13.100.1 13.13.100.20

ip dhcp pool VLAN100
 network 13.13.100.0 255.255.255.0
 default-router 13.13.100.1
 dns-server 168.126.63.1
 lease 5
end
write memory
```

---

## Verification

```cisco
show ip dhcp pool
show ip dhcp binding
show running-config | section dhcp
```

---

## Notes

- Gateway 주소는 보통 SVI 또는 라우터 인터페이스 IP를 사용합니다.
- 서버/프린터 등 고정 IP 장비는 `excluded-address`로 제외하는 것이 좋습니다.
```
