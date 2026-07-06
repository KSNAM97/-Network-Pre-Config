# mGRE Pre-Config

## 설명
mGRE(multipoint GRE) 기본 설정 템플릿입니다.

하나의 Tunnel 인터페이스에서 여러 Peer를 수용할 수 있도록 구성하며,  
DMVPN의 기반 터널 기술로 사용됩니다.

---

## 핵심 포인트

- `tunnel mode gre multipoint`
- `tunnel key`
- 다중 Peer 수용
- NHRP와 함께 사용할 수 있음

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
 tunnel mode gre multipoint
 tunnel key <TUNNEL_KEY>
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
 ip address 172.16.0.1 255.255.255.0
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source GigabitEthernet0/0
 tunnel mode gre multipoint
 tunnel key 100
!
end
write memory
```

---

## Verification

```cisco
show interface tunnel0
show running-config interface tunnel0
```

---

## Notes

- mGRE 자체만으로는 DMVPN이 아닙니다.
- 실제 DMVPN은 mGRE + NHRP + IPsec + Routing 구조입니다.
- 하나의 터널에 여러 Spoke를 수용하는 허브 구조에 적합합니다.
```
