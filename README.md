# Network Pre-Config

네트워크 실습에서 반복적으로 사용하는 **Cisco IOS Pre-Config 템플릿**을 정리한 저장소입니다.

프로토콜 및 기능별로 설정을 분리했으며,  
각 문서는 아래 형식으로 통일하여 관리합니다.

- **설명 (Description)**
- **프리컨피그 (Pre-Config)**
- **예시 (Example)**
- **검증 명령어 (Verification)**
- **참고사항 (Notes)**

---

## 폴더 구조

    network-preconfig/
    ├─ README.md
    ├─ 00-common/
    ├─ 01-routing/
    ├─ 02-switching/
    ├─ 03-services-security/
    └─ 04-vpn/

---

## 폴더 안내

### `00-common/`
공통 기본 설정 문서를 정리한 폴더입니다.

- Router Base
- Switch Base
- SSH Management

### `01-routing/`
라우팅 관련 프리컨피그 문서를 정리한 폴더입니다.

- EIGRP
- OSPF
- Default Route
- Redistribution

### `02-switching/`
스위칭 관련 프리컨피그 문서를 정리한 폴더입니다.

- VLAN
- Trunk
- SVI
- EtherChannel
- VTP

### `03-services-security/`
서비스 및 보안 관련 프리컨피그 문서를 정리한 폴더입니다.

- DHCP
- NAT
- NTP
- ACL
- Port Security
- CBAC

### `04-vpn/`
VPN 및 터널링 관련 프리컨피그 문서를 정리한 폴더입니다.

- GRE
- IPsec
- GRE over IPsec
- DMVPN

---

## 사용 방법

1. `00-common/`에서 기본 설정을 먼저 적용합니다.
2. 필요한 프로토콜 또는 기능 문서를 선택합니다.
3. `Pre-Config` 템플릿을 확인합니다.
4. 실습 환경에 맞게 IP, 인터페이스, 프로세스 번호 등 값을 수정합니다.
5. `Example`과 비교하여 설정 방향을 점검합니다.
6. `Verification` 명령어로 정상 동작 여부를 확인합니다.

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

- **GitHub**: 실습 결과물 및 설정 템플릿 관리
- **Notion**: 개념 정리 및 학습 내용 관리
- 본 저장소는 **재사용 가능한 Pre-Config 템플릿 저장소**를 목적으로 합니다.

---

## License

This project is licensed under the MIT License.
