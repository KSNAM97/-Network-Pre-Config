# VTP Pre-Config

## 설명
VTP(VLAN Trunking Protocol)를 사용하여 VLAN 정보를 스위치 간 동기화하는 템플릿입니다.

Server / Client / Transparent 모드 기반 VLAN 관리 실습에 사용합니다.

---

## 핵심 포인트

- VTP Domain 설정
- Password 설정 가능
- Server / Client / Transparent 모드 구분

---

## Pre-Config

```cisco
configure terminal
vtp domain <VTP_DOMAIN>
vtp mode <VTP_MODE>
vtp password <VTP_PASSWORD>
end
write memory
```

---

## Example

```cisco
configure terminal
vtp domain ccnp
vtp mode server
vtp password cisco1234
end
write memory
```

---

## Verification

```cisco
show vtp status
show running-config | include vtp
```

---

## Notes

- VTP는 같은 Domain, Password, Version이어야 정상 동작합니다.
- 실무에서는 VTP 사용을 최소화하거나 Transparent를 선호하는 경우도 많습니다.
```
