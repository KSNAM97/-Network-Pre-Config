# IPsec IKEv1 Pre-Config

## 설명
IPsec VPN에서 공통으로 사용하는 IKEv1 기반 보안 설정 템플릿입니다.

GRE, GRE over IPsec, mGRE, DMVPN 실습에서 반복되는  
ISAKMP / Transform-Set / IPsec Profile 구성을 공통 블록으로 분리한 문서입니다.

---

## 핵심 포인트

- IKEv1 Phase 1: ISAKMP Policy
- Pre-Shared Key(PSK)
- IKE Keepalive / DPD
- IPsec Phase 2: Transform-Set
- PFS(Perfect Forward Secrecy)
- SA Lifetime
- IPsec Profile

---

## 구성 요소 설명

### 1. ISAKMP Policy
IKE Phase 1에서 사용할 암호화, 해시, 인증, DH Group, Lifetime을 정의합니다.

### 2. Pre-Shared Key
Peer 간 사전 공유 키입니다.  
양쪽 장비에서 값이 일치해야 합니다.

### 3. ISAKMP Keepalive
Dead Peer Detection(DPD) 역할로,  
상대 장비 상태를 확인하는 데 사용합니다.

### 4. Transform-Set
IPsec Phase 2에서 사용할 ESP 암호화/무결성 알고리즘을 정의합니다.

### 5. IPsec Profile
Tunnel 인터페이스에 적용할 IPsec 보호 정책입니다.

---

## Pre-Config

```cisco
configure terminal
!
crypto isakmp policy 10
 authentication pre-share
 encryption aes 256
 hash sha
 group 5
 lifetime 86400
!
crypto isakmp key <PSK> address <PEER_PUBLIC_IP>
!
crypto isakmp keepalive 10 3
!
crypto ipsec transform-set TS-AES256 esp-aes 256 esp-sha-hmac
 mode transport
!
crypto ipsec profile VPN-PROFILE
 set transform-set TS-AES256
 set pfs group5
 set security-association lifetime seconds 3600
!
end
write memory
```

---

## Example

```cisco
configure terminal
!
crypto isakmp policy 10
 authentication pre-share
 encryption aes 256
 hash sha
 group 5
 lifetime 86400
!
crypto isakmp key DMVPN-HUB address 203.0.113.6
!
crypto isakmp keepalive 10 3
!
crypto ipsec transform-set TS-AES256 esp-aes 256 esp-sha-hmac
 mode transport
!
crypto ipsec profile DMVPN-PROFILE
 set transform-set TS-AES256
 set pfs group5
 set security-association lifetime seconds 3600
!
end
write memory
```

---

## Wildcard PSK Example

허브 주소를 고정하지 않고, 여러 피어를 수용해야 하는 경우 사용할 수 있는 예시입니다.

```cisco
configure terminal
!
crypto isakmp policy 10
 authentication pre-share
 encryption aes 256
 hash sha
 group 5
 lifetime 86400
!
crypto isakmp key DMVPN-HUB address 0.0.0.0 0.0.0.0
!
crypto isakmp keepalive 10 3
!
crypto ipsec transform-set TS-AES256 esp-aes 256 esp-sha-hmac
 mode transport
!
crypto ipsec profile DMVPN-PROFILE
 set transform-set TS-AES256
 set pfs group5
 set security-association lifetime seconds 3600
!
end
write memory
```

---

## Verification

```cisco
show crypto isakmp policy
show crypto isakmp sa
show crypto ipsec transform-set
show crypto ipsec sa
show crypto ipsec profile
show crypto session
show crypto engine connections active
```

---

## Notes

- GRE / mGRE / DMVPN과 함께 사용할 때는 `mode transport`를 우선 고려합니다.
- 직접 Site-to-Site IPsec만 구성할 경우에는 `mode tunnel`을 사용할 수 있습니다.
- 양쪽 장비의 아래 항목은 반드시 일치해야 합니다.
  - encryption
  - hash
  - authentication
  - group
  - PFS group
  - PSK
- Lifetime 값이 달라도 협상은 가능할 수 있지만, 실습에서는 맞춰두는 것이 좋습니다.
- DPD(`crypto isakmp keepalive`)는 상대 장비 장애 감지에 도움이 됩니다.

---

## Common Troubleshooting Points

- `show crypto isakmp sa` 상태가 올라오지 않으면:
  - PSK 불일치
  - Peer IP 오타
  - ISAKMP Policy 불일치
  - Underlay 통신 불가
- `show crypto isakmp sa`는 올라오는데 IPsec SA가 안 생기면:
  - Transform-Set 불일치
  - Tunnel 보호 적용 누락
  - Interesting Traffic 미발생
- NAT 환경에서는 VPN 대상 대역이 NAT 예외 처리되어야 합니다.

---

## Related Documents

- `gre.md`
- `gre-over-ipsec.md`
- `mgre.md`
- `dmvpn-phase2.md`
- `dmvpn-phase3.md`
```
