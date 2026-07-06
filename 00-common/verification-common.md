# Common Verification Commands

## 설명
라우터/스위치 공통으로 자주 사용하는 기본 검증 명령어 모음입니다.

실습 초기에 장비 상태, 인터페이스 상태, 설정 적용 여부를 빠르게 확인할 때 사용합니다.

---

## Router Verification

```cisco
show running-config
show startup-config
show ip interface brief
show interfaces description
show users
show line
show version
show clock
```

---

## Switch Verification

```cisco
show running-config
show startup-config
show interfaces status
show vlan brief
show spanning-tree summary
show mac address-table
show users
show version
```

---

## Management Verification

```cisco
show ip ssh
show crypto key mypubkey rsa
show running-config | section line vty
show running-config | include username
```

---

## Notes

- 실습 시작 직후에는 `show ip interface brief` 또는 `show interfaces status`를 가장 먼저 확인하는 것이 좋습니다.
- 원격 접속 문제가 있으면 `line vty`, `login local`, `transport input` 설정을 우선 점검합니다.
- 설정 저장 여부는 `show startup-config` 또는 `copy run start`로 확인합니다.
```
