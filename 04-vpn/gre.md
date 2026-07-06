# GRE Pre-Config

## 설명
GRE(Generic Routing Encapsulation) 터널 기본 설정 템플릿입니다.

두 라우터 사이에 1:1 논리 터널을 구성하고,  
라우팅 프로토콜 또는 사설망 트래픽을 전달할 때 사용합니다.

---

## 핵심 포인트

- `tunnel source`
- `tunnel destination`
- Tunnel IP 할당
- Keepalive
- MTU / MSS 조정

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
!
end
write memory
```

---

## Verification

```cisco
show ip interface brief
show interface tunnel0
show running-config interface tunnel0
ping <PEER_TUNNEL_IP>
```

---

## Notes

- GRE는 암호화를 제공하지 않습니다.
- 인터넷 구간 보호가 필요하면 IPsec과 함께 사용합니다.
- MTU/MSS 조정을 하지 않으면 단편화가 발생할 수 있습니다.
```
