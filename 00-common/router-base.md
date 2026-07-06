# Router Base Pre-Config

## 설명
Cisco Router 실습 전에 공통으로 적용하는 기본 초기 설정입니다.

반복적으로 사용하는 관리 설정을 먼저 적용해서  
이후 라우팅, 보안, VPN 실습을 바로 진행할 수 있도록 구성합니다.

---

## 핵심 포인트

- `hostname` 설정
- `no ip domain-lookup` 적용
- 콘솔/VTY 접속 환경 정리
- 로컬 관리자 계정 생성
- 기본 관리 편의성 확보

---

## Pre-Config

```cisco
enable
configure terminal
hostname <HOSTNAME>
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

end
write memory
```

---

## Example

```cisco
enable
configure terminal
hostname R1
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

end
write memory
```

---

## Verification

```cisco
show running-config
show users
show ip interface brief
show line
```

---

## Notes

- `no ip domain-lookup`은 오입력 시 DNS 질의를 막아 실습 진행 속도를 높여줍니다.
- `logging synchronous`는 콘솔 입력 중 시스템 메시지로 줄이 깨지는 현상을 줄여줍니다.
- SSH만 사용할 경우 `transport input ssh`로 변경할 수 있습니다.
- SSH 접속을 위해서는 별도의 `ip domain-name`, RSA Key 생성 설정이 필요합니다.
```
