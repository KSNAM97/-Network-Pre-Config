# DMVPN Phase 2 Pre-Config

## 설명
DMVPN Phase 2 기본 구성 템플릿입니다.

Hub는 mGRE + NHRP Server(NHS) 역할을 수행하고,  
Spoke는 Hub를 통해 등록한 뒤 Spoke-to-Spoke 직접 통신이 가능하도록 구성합니다.

---

## 핵심 포인트

- Hub: `tunnel mode gre multipoint`
- Hub: `ip nhrp map multicast dynamic`
- Hub: `no ip split-horizon eigrp`
- Spoke: `ip nhrp nhs`
- Spoke: Hub 오버레이/공인 IP 정적 매핑
- Overlay 라우팅(EIGRP/OSPF)
- IPsec Profile 적용

---

## Hub Pre-Config

```cisco
configure terminal
!
interface Tunnel0
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
 no ip split-horizon eigrp <ASN>
 tunnel protection ipsec profile <IPSEC_PROFILE>
!
router eigrp <ASN>
 no auto-summary
 passive-interface default
 no passive-interface Tunnel0
 network <TUNNEL_NETWORK> <WILDCARD>
end
write memory
```

---

## Hub Example

```cisco
configure terminal
!
interface Tunnel0
 ip address 172.16.0.1 255.255.255.0
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source GigabitEthernet0/0
 tunnel mode gre multipoint
 tunnel key 100
 ip nhrp network-id 100
 ip nhrp holdtime 600
 ip nhrp authentication NHRPKEY1
 ip nhrp map multicast dynamic
 no ip split-horizon eigrp 100
 tunnel protection ipsec profile DMVPN-PROFILE
!
router eigrp 100
 no auto-summary
 passive-interface default
 no passive-interface Tunnel0
 network 172.16.0.0 0.0.0.255
end
write memory
```

---

## Spoke Pre-Config

```cisco
configure terminal
!
interface Tunnel0
 ip address <SPOKE_TUNNEL_IP> <MASK>
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source <WAN_INTERFACE>
 tunnel mode gre multipoint
 tunnel key <TUNNEL_KEY>
 ip nhrp network-id <NHRP_ID>
 ip nhrp holdtime 600
 ip nhrp authentication <NHRP_KEY>
 ip nhrp nhs <HUB_TUNNEL_IP>
 ip nhrp map <HUB_TUNNEL_IP> <HUB_PUBLIC_IP>
 ip nhrp map multicast <HUB_PUBLIC_IP>
 tunnel protection ipsec profile <IPSEC_PROFILE>
!
router eigrp <ASN>
 no auto-summary
 passive-interface default
 no passive-interface Tunnel0
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
interface Tunnel0
 ip address 172.16.0.11 255.255.255.0
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source FastEthernet0/0
 tunnel mode gre multipoint
 tunnel key 100
 ip nhrp network-id 100
 ip nhrp holdtime 600
 ip nhrp authentication NHRPKEY1
 ip nhrp nhs 172.16.0.1
 ip nhrp map 172.16.0.1 203.0.113.6
 ip nhrp map multicast 203.0.113.6
 tunnel protection ipsec profile DMVPN-PROFILE
!
router eigrp 100
 no auto-summary
 passive-interface default
 no passive-interface Tunnel0
 network 172.16.0.0 0.0.0.255
 network 172.10.20.0 0.0.0.255
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

## Notes

- 네 설정 기준으로 `Branch1 / Tunnel0`는 이 문서 예시로 사용하기 좋습니다.
- Hub에서는 `no ip split-horizon eigrp`가 매우 중요합니다.
- Phase 2는 Spoke-to-Spoke 직접 통신이 가능하지만, 라우팅 next-hop 처리 정책을 같이 봐야 합니다.
- NAT 설정은 별도 문서에서 관리하고, VPN 보호 대역은 NAT 예외 처리되어야 합니다.
```
