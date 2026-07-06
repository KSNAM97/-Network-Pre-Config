# SSH Management Pre-Config

## 설명
Telnet 대신 SSH로 안전하게 원격 접속하기 위한 기본 설정입니다.

라우터/스위치 공통으로 사용할 수 있으며,  
로컬 계정 기반 관리 접속 환경을 구성합니다.

---

## 핵심 포인트

- `ip domain-name` 설정
- RSA Key 생성
- SSH Version 2 사용
- VTY 접속을 SSH로 제한

---

## Pre-Config

```cisco
configure terminal
ip domain-name <DOMAIN_NAME>
crypto key generate rsa modulus 2048
ip ssh version 2

line vty 0 4
 transport input ssh
 login local

end
write memory
```

---

## Example

```cisco
configure terminal
ip domain-name lab.local
crypto key generate rsa modulus 2048
ip ssh version 2

line vty 0 4
 transport input ssh
 login local

end
write memory
```

---

## Verification

```cisco
show ip ssh
show crypto key mypubkey rsa
show running-config | section line vty
```

---

## Notes

- 이 설정 전에 반드시 로컬 사용자 계정이 있어야 합니다.
- `router-base.md` 또는 `switch-base.md` 적용 후 사용하는 것을 권장합니다.
- 일부 IOS에서는 RSA Key 생성 시 yes/no 확인이 추가로 나올 수 있습니다.
```
