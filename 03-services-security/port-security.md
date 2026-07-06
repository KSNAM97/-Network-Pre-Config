# Port Security Pre-Config

## 설명
스위치 Access Port에 허용된 MAC 주소만 통신하도록 제한하는 Port Security 기본 템플릿입니다.

사내 단말 접속 통제 및 무단 장비 연결 방지에 사용합니다.

---

## 핵심 포인트

- Access Port 전용
- 최대 MAC 개수 제한
- Violation Mode 설정
- Sticky MAC 사용 가능

---

## Pre-Config

```cisco
configure terminal
interface <ACCESS_PORT>
 switchport mode access
 switchport port-security
 switchport port-security maximum <MAX_COUNT>
 switchport port-security violation <VIOLATION_MODE>
 switchport port-security mac-address sticky
end
write memory
```

---

## Example

```cisco
configure terminal
interface e0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation restrict
 switchport port-security mac-address sticky
end
write memory
```

---

## Verification

```cisco
show port-security
show port-security interface <ACCESS_PORT>
show running-config interface <ACCESS_PORT>
```

---

## Notes

- `protect`, `restrict`, `shutdown` 모드를 상황에 맞게 선택합니다.
- Port Security는 Trunk Port가 아니라 Access Port에 적용하는 것이 일반적입니다.
```
