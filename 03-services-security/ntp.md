# NTP Pre-Config

## 설명
장비 간 시간 동기화를 위한 NTP 기본 설정 템플릿입니다.

로그 분석, 인증, 운영 일관성 확보를 위해 사용합니다.

---

## 핵심 포인트

- NTP Server / Client 구분
- 신뢰 가능한 시간 소스 지정
- 필요 시 인증 적용 가능

---

## Pre-Config

```cisco
configure terminal
ntp server <NTP_SERVER_IP>
end
write memory
```

---

## Example

```cisco
configure terminal
ntp server 100.10.1.1
end
write memory
```

---

## Verification

```cisco
show ntp status
show ntp associations
show clock
```

---

## Notes

- 실습에서는 라우터를 NTP Server로 두고 다른 장비를 Client로 구성하는 경우가 많습니다.
- 인증까지 사용할 경우 `ntp authenticate`, `ntp authentication-key`, `ntp trusted-key` 설정을 추가합니다.
```
