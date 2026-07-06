# ACL Standard / Extended Pre-Config

## 설명
표준 ACL과 확장 ACL을 이용한 기본 트래픽 제어 템플릿입니다.

출발지/목적지/프로토콜/포트 기반 정책 제어 실습에 사용합니다.

---

## 핵심 포인트

- Standard ACL: Source IP 기준
- Extended ACL: SA/DA/Protocol/Port 기준
- Top-down 처리
- Implicit Deny 존재

---

## Pre-Config

```cisco
configure terminal
access-list 10 permit <SOURCE_NETWORK> <WILDCARD>
access-list 10 deny any
!
access-list 101 permit ospf any any
access-list 101 permit eigrp any any
access-list 101 deny tcp host <SRC_IP> host <DST_IP> eq telnet
access-list 101 deny tcp host <SRC_IP> host <DST_IP> eq www
access-list 101 permit ip any any
end
write memory
```

---

## Example

```cisco
configure terminal
access-list 10 permit 192.168.10.0 0.0.0.255
access-list 10 deny any
!
access-list 101 permit ospf any any
access-list 101 deny tcp host 198.210.10.1 host 110.11.2.2 eq telnet
access-list 101 deny tcp host 198.210.10.1 host 100.10.1.1 eq www
access-list 101 permit ip any any
end
write memory
```

---

## Verification

```cisco
show access-lists
show ip interface <INTERFACE>
show running-config | section access-list
```

---

## Notes

- OSPF/EIGRP 환경에서는 프로토콜 허용 규칙 누락에 주의합니다.
- Extended ACL은 일반적으로 출발지 가까이에 적용합니다.
- 마지막에 명시하지 않아도 `implicit deny`가 존재합니다.
```
