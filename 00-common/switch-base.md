# Switch Base Pre-Config

## 설명
Cisco Switch 실습 전에 공통으로 적용하는 기본 초기 설정입니다.

L2/L3 스위치 실습에서 반복되는 관리 설정을 먼저 적용하고,  
이후 VLAN, Trunk, SVI, EtherChannel, VTP 실습에 바로 사용할 수 있도록 구성합니다.

---

## 핵심 포인트

- `hostname` 설정
- `no ip domain-lookup` 적용
- 콘솔/VTY 접속 환경 정리
- 로컬 관리자 계정 생성
- STP 기본 모드 설정

---

## Pre-Config

```cisco
enable
configure terminal
hostname <SWITCH_NAME>
no ip domain-lookup

service password-encryption
enable secret <ENABLE_SECRET>

banner motd ^CUnauthorized access prohibited.^C

username <ADMIN_USER> privilege 15 secret <ADMIN_PASSWORD>

line console 0
 logging synchronous
 exec-timeout 0 0
line vty 0 4
 logging synchronous
 exec-timeout 10 0
 transport input telnet ssh
 login local

spanning-tree mode pvst

end
write memory
```

---

## Example

```cisco
enable
configure terminal
hostname SW1
no ip domain-lookup

service password-encryption
enable secret cisco1234

banner motd ^CUnauthorized access prohibited.^C

username admin privilege 15 secret admin1234

line console 0
 logging synchronous
 exec-timeout 0 0
line vty 0 4
 logging synchronous
 exec-timeout 10 0
 transport input telnet ssh
 login local

spanning-tree mode pvst

end
write memory
```

---

## Verification

```cisco
show running-config
show users
show spanning-tree summary
show interfaces status
```

---

## Notes

- L3 Switch 실습 시에는 이후 별도로 `ip routing`을 활성화해야 합니다.
- STP 모드는 실습 환경에 따라 `pvst`, `rapid-pvst` 등으로 변경할 수 있습니다.
- SSH 접속을 위해서는 `ssh-management.md` 설정을 추가로 적용합니다.
```
/switch-base.md
