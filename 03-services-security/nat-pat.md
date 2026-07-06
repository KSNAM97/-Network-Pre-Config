# NAT / PAT Pre-Config

## 설명
내부 사설망을 외부 공인망으로 변환하기 위한 NAT / PAT 기본 템플릿입니다.

인터넷 연결 또는 ISP 연결 실습에서 자주 사용합니다.

---

## 핵심 포인트

- Inside / Outside 인터페이스 구분
- Access-List로 내부망 지정
- PAT(Overload) 적용

---

## Pre-Config

```cisco
configure terminal
access-list 1 permit <INSIDE_NETWORK> <WILDCARD>

interface <INSIDE_INTERFACE>
 ip nat inside
!
interface <OUTSIDE_INTERFACE>
 ip nat outside
!
ip nat inside source list 1 interface <OUTSIDE_INTERFACE> overload
end
write memory
```

---

## Example

```cisco
configure terminal
access-list 1 permit 192.168.10.0 0.0.0.255

interface fa0/0
 ip nat inside
!
interface s1/0
 ip nat outside
!
ip nat inside source list 1 interface s1/0 overload
end
write memory
```

---

## Verification

```cisco
show ip nat translations
show ip nat statistics
show running-config | include ip nat
```

---

## Notes

- PAT는 여러 내부 호스트가 하나의 공인 IP를 공유할 수 있게 해줍니다.
- 인터페이스 방향(`inside/outside`)을 반대로 넣지 않도록 주의합니다.
```
