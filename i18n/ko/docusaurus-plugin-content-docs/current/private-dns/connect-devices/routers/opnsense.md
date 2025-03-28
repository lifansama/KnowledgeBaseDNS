---
title: OPNSense
sidebar_position: 8
---

OPNSense 펌웨어는 종종 무선 액세스 포인트, DHCP 서버, DNS 서버를 구성하는 데 사용되며, AdGuard DNS를 장치에서 직접 구성할 수 있도록 허용합니다.

## 라우터 관리 패널 사용

Keenetic 라우터가 DNS-over-HTTPS 또는 DNS-over-TLS 구성을 지원하지 않는 경우, 이 지침을 따르세요.

1. 라우터 관리자 패널을 엽니다. 192.168.1.1`또는`192.168.0.1\`에서 접속할 수 있습니다.
2. 관리자 사용자 이름(일반적으로 admin)과 라우터 비밀번호를 입력합니다.
3. 상단 메뉴에서 **서비스**를 클릭한 다음 드롭다운 메뉴에서 **DHCP 서버**를 선택합니다.
4. **DHCP 서버** 페이지에서 DNS 설정을 구성할 인터페이스(예: LAN, WLAN)를 선택합니다.
5. **DNS 서버**까지 아래로 스크롤합니다.
6. **수동 DNS**를 선택합니다. **이 DNS 서버 사용** 또는 **수동으로 DNS 서버 지정**을 선택하고 다음 DNS 서버 주소를 입력합니다:
    - IPv4: `94.140.14.49` 및 `94.140.14.59`
    - IPv6: `2a10:50c0:0:0:0:0:ded:ff` 및 `2a10:50c0:0:0:0:0:dad:ff`
7. 설정을 저장합니다.
8. 필요에 따라 보안 강화를 위해 DNSSEC를 활성화할 수 있습니다.
9. IP(또는 팀 구독이 있는 경우 전용 IP)를 연결합니다.

- [전용 IP](/private-dns/connect-devices/other-options/dedicated-ip.md)
- [연결된 IPs](/private-dns/connect-devices/other-options/linked-ip.md)
