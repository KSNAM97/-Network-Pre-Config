# CBAC Pre-Config

## 설명
CBAC(Context-Based Access Control)를 사용한 상태 기반(Stateful) 트래픽 제어 기본 템플릿입니다.

Inside에서 시작된 세션을 추적하고, 반환 트래픽을 동적으로 허용할 때 사용합니다.

---

## 핵심 포인트

- Inspect Rule 생성
- Inside / Outside 방향 적용
- Stateful Inspection 기반 동작

---

## Pre-Config

```cisco
configure terminal
ip inspect name <INSPECT_NAME> tcp
ip inspect name <INSPECT_NAME> udp
ip inspect name <INSPECT_NAME> icmp
!
interface <INSIDE_INTERFACE>
 ip inspect <INSPECT_NAME> out
end
write memory
```

---

## Example

```cisco
configure terminal
ip inspect name CBAC-FW tcp
ip inspect name CBAC-FW udp
ip inspect name CBAC-FW icmp
!
interface fa0/0
 ip inspect CBAC-FW out
end
write memory
```

---

## Verification

```cisco
show ip inspect config
show ip inspect interfaces
show ip inspect sessions
```

---

## Notes

- CBAC는 내부에서 시작된 세션을 기준으로 응답 트래픽을 허용합니다.
- ACL과 함께 구성할 경우 인터페이스 방향과 정책 순서를 함께 점검해야 합니다.
```
