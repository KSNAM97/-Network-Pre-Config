# GRE over IPsec Pre-Config

## 설명
GRE 터널을 IPsec으로 보호하는 기본 템플릿입니다.

동적 라우팅(EIGRP / OSPF)이나 멀티캐스트 트래픽을  
암호화된 상태로 전달해야 할 때 사용합니다.

---

## 핵심 포인트

- GRE 터널 생성
- IPsec Profile 적용
- Underlay / Overlay 구분
- MTU / MSS 조정
- Tunnel 보호

---

## Pre-Config

```cisco
configure terminal
!
interface Tunnel0
 ip address <TUNNEL_IP> <MASK>
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source <SOURCE_INTERFACE>
 tunnel destination <PEER_PUBLIC_IP>
 keepalive 10 3
 tunnel protection ipsec profile <IPSEC_PROFILE>
!
end
write memory
```

---

## Example

```cisco
configure terminal
!
interface Tunnel0
 ip address 172.16.0.11 255.255.255.0
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source FastEthernet0/0
 tunnel destination 203.0.113.6
 keepalive 10 3
 tunnel protection ipsec profile DMVPN-PROFILE
!
end
write memory
```

---

## Verification

```cisco
show interface tunnel0
show crypto isakmp sa
show crypto ipsec sa
show crypto session
ping <PEER_TUNNEL_IP>
```

---

## Notes

- 이 문서는 `ipsec-ikev1.md`의 공통 보안 설정이 이미 적용되었다는 전제입니다.
- GRE가 라우팅/멀티캐스트를 담당하고, IPsec이 암호화를 담당합니다.
- GRE over IPsec 환경에서는 보통 `mode transport`를 우선 검토합니다.
```
