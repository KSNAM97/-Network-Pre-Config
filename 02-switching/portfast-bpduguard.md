# PortFast / BPDU Guard Pre-Config

## 설명
PC가 연결되는 Access Port에 PortFast를 적용하고, BPDU Guard로 스위치 연결 오동작을 방지하는 템플릿입니다.

Access Layer 보안 및 빠른 포트 활성화에 유용합니다.

---

## 핵심 포인트

- PortFast 활성화
- BPDU Guard 적용
- End-Host 포트 보호

---

## Pre-Config

```cisco
configure terminal
interface <ACCESS_PORT>
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
end
write memory
```

---

## Example

```cisco
configure terminal
interface e0/3
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
end
write memory
```

---

## Verification

```cisco
show spanning-tree interface <ACCESS_PORT> detail
show running-config interface <ACCESS_PORT>
```

---

## Notes

- PortFast는 End Device가 연결된 포트에만 적용합니다.
- BPDU Guard는 해당 포트에서 BPDU 수신 시 포트를 Err-Disable 상태로 만들 수 있습니다.
```
