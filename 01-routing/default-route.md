# Default Route Pre-Config

## 설명
외부망 또는 ISP 방향으로 기본 경로(Default Route)를 설정하는 템플릿입니다.

라우터가 목적지를 알 수 없는 경우, 지정한 Next-Hop 또는 출구 인터페이스로 트래픽을 전달합니다.

---

## 핵심 포인트

- 정적 기본 경로 설정
- ISP 또는 상위 라우터 방향 지정
- Stub / Branch 환경에서 자주 사용

---

## Pre-Config

```cisco
configure terminal
ip route 0.0.0.0 0.0.0.0 <NEXT_HOP_IP>
end
write memory
```

---

## Example

```cisco
configure terminal
ip route 0.0.0.0 0.0.0.0 121.160.11.65
end
write memory
```

---

## Verification

```cisco
show ip route static
show ip route
```

---

## Notes

- Next-Hop IP 방식이 일반적으로 더 명확합니다.
- 환경에 따라 출구 인터페이스 방식으로도 설정할 수 있습니다.
```
