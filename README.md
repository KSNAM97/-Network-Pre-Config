# Network Pre-Config

네트워크 실습에서 반복해서 사용하는 Cisco IOS 프리컨피그(Pre-Config) 템플릿 저장소입니다.

프로토콜/기능별로 설정을 분리했으며, 각 문서는 아래 형식으로 정리합니다.

- 설명 (Description)
- 프리컨피그 (Pre-Config)
- 예시 (Example)
- 검증 명령어 (Verification)
- 참고사항 (Notes)

---

## 폴더 구조

```text
network-preconfig/
├─ README.md
├─ 00-common/
├─ 01-routing/
├─ 02-switching/
├─ 03-services-security/
└─ 04-vpn/
```

---

## 폴더 안내

- `00-common/`  
  공통 기본 설정  
  (Router / Switch / SSH)

- `01-routing/`  
  라우팅 설정  
  (EIGRP / OSPF / Default Route / Redistribution)

- `02-switching/`  
  스위칭 설정  
  (VLAN / Trunk / SVI / EtherChannel / VTP)

- `03-services-security/`  
  서비스 및 보안 설정  
  (DHCP / NAT / NTP / ACL / Port Security / CBAC)

- `04-vpn/`  
  VPN 및 터널링 설정  
  (GRE / IPsec / GRE over IPsec / DMVPN)

---

## 사용 방법

1. `00-common/`에서 기본 설정 적용
2. 필요한 프로토콜 문서 선택
3. `Pre-Config` 템플릿 확인
4. 실습 환경에 맞게 값 수정
5. `Example`과 비교
6. `Verification` 명령어로 검증

---

## 관련 실습 저장소

- Campus-VLAN-OSPF-NTP-Security
- EIGRP-Multi-ISP-Network
- OSPF-ACL-Multi-ISP-Network
- GRE-GRE-Over-IPsec-Basic-Study
- IPsec-Network-Security-Lab
- DMVPN-Over-IPsec-Lab

---

## 메모

- GitHub: 실습 결과물 관리
- Notion: 개념 정리 관리
- 본 저장소: 재사용 가능한 Pre-Config 템플릿 관리

---

## License

MIT License
