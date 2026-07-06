# DMVPN Phase 3 Pre-Config

## 설명
DMVPN Phase 3 기본 구성 템플릿입니다.

Phase 2와 비교해 Hub가 `ip nhrp redirect`,  
Spoke가 `ip nhrp shortcut`을 사용하며,  
Spoke-to-Spoke 최적 경로를 더 유연하게 형성할 수 있습니다.

---

## 핵심 포인트

- Hub: `ip nhrp redirect`
- Spoke: `ip nhrp shortcut`
- 듀얼 허브 가능
- mGRE + NHRP + IPsec + Routing
- MTU / MSS 조정
- Overlay 라우팅

---

## Hub Pre-Config

```cisco
configure terminal
!
interface Tunnel1
 ip address <HUB_TUNNEL_IP> <MASK>
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source <WAN_INTERFACE>
 tunnel mode gre multipoint
 tunnel key <TUNNEL_KEY>
 ip nhrp network-id <NHRP_ID>
 ip nhrp holdtime 600
 ip nhrp authentication <NHRP_KEY>
 ip nhrp map multicast dynamic
 ip nhrp redirect
 no ip split-horizon eigrp <ASN>
 no ip next-hop-self eigrp <ASN>
 tunnel protection ipsec profile <IPSEC_PROFILE>
!
router eigrp <ASN>
 no auto-summary
 passive-interface default
 no passive-interface Tunnel1
 network <TUNNEL_NETWORK> <WILDCARD>
end
write memory
```

---

## Hub Example

```cisco
configure terminal
!
interface Tunnel1
 ip address 172.16.10.1 255.255.255.0
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source GigabitEthernet0/0
 tunnel mode gre multipoint
 tunnel key 200
 ip nhrp network-id 200
 ip nhrp holdtime 600
 ip nhrp authentication NHRPKEY2
 ip nhrp map multicast dynamic
 ip nhrp redirect
 no ip split-horizon eigrp 100
 no ip next-hop-self eigrp 100
 tunnel protection ipsec profile DMVPN-PROFILE
!
router eigrp 100
 no auto-summary
 passive-interface default
 no passive-interface Tunnel1
 network 172.16.10.0 0.0.0.255
end
write memory
```

---

## Spoke Pre-Config

```cisco
configure terminal
!
interface Tunnel1
 ip address <SPOKE_TUNNEL_IP> <MASK>
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source <WAN_INTERFACE>
 tunnel mode gre multipoint
 tunnel key <TUNNEL_KEY>
 ip nhrp network-id <NHRP_ID>
 ip nhrp holdtime 600
 ip nhrp authentication <NHRP_KEY>
 ip nhrp nhs <HUB1_TUNNEL_IP>
 ip nhrp nhs <HUB2_TUNNEL_IP>
 ip nhrp map <HUB1_TUNNEL_IP> <HUB1_PUBLIC_IP>
 ip nhrp map <HUB2_TUNNEL_IP> <HUB2_PUBLIC_IP>
 ip nhrp map multicast <HUB1_PUBLIC_IP>
 ip nhrp map multicast <HUB2_PUBLIC_IP>
 ip nhrp shortcut
 tunnel protection ipsec profile <IPSEC_PROFILE>
!
router eigrp <ASN>
 no auto-summary
 passive-interface default
 no passive-interface Tunnel1
 network <TUNNEL_NETWORK> <WILDCARD>
 network <LAN_NETWORK> <LAN_WILDCARD>
end
write memory
```

---

## Spoke Example

```cisco
configure terminal
!
interface Tunnel1
 ip address 172.16.10.33 255.255.255.0
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source GigabitEthernet0/0
 tunnel mode gre multipoint
 tunnel key 200
 ip nhrp network-id 200
 ip nhrp holdtime 600
 ip nhrp authentication NHRPKEY2
 ip nhrp nhs 172.16.10.1
 ip nhrp nhs 172.16.10.2
 ip nhrp map 172.16.10.1 203.0.113.6
 ip nhrp map 172.16.10.2 203.0.113.10
 ip nhrp map multicast 203.0.113.6
 ip nhrp map multicast 203.0.113.10
 ip nhrp shortcut
 tunnel protection ipsec profile DMVPN-PROFILE
!
router eigrp 100
 no auto-summary
 passive-interface default
 no passive-interface Tunnel1
 network 172.16.10.0 0.0.0.255
 network 172.10.30.0 0.0.0.255
end
write memory
```

---

## Verification

```cisco
show dmvpn
show ip nhrp
show ip eigrp neighbors
show ip route eigrp
show crypto isakmp sa
show crypto ipsec sa
show crypto session
```

---

## Phase 2 대비 변경점

- Hub:
  - `ip nhrp redirect` 추가
  - `no ip next-hop-self eigrp <ASN>` 적용 가능
- Spoke:
  - `ip nhrp shortcut` 추가
- 목적:
  - Hub가 최적 경로 정보를 유도하고, Spoke가 더 효율적인 직접 경로를 형성

---

## Notes

- 네 설정 기준으로 `Branch3 / Tunnel1`는 이 문서 예시로 사용하기 좋습니다.
- 듀얼 허브 구조에서는 `ip nhrp nhs`, `map`, `map multicast`를 허브별로 모두 명시해야 합니다.
- Phase 3는 규모가 커질수록 운용상 장점이 큽니다.
- NAT/PAT 내용은 별도 문서에서 관리하고, VPN 보호 대역은 NAT 예외 처리되어야 합니다.
```
