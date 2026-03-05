# 🌐 학습 기록 정리

## 4) DNS ( Domain Name System )
- **정의**: 문자주소와 IP주소를 상호 변환해주는 시스템
- **Domain (도메인)**: 인터넷 상에서 쓰이는 문자 주소

---

## 💻 원격 접속
- **개요**: 네트워크를 통하여 원격지에 있는 컴퓨터와 연결해 시스템을 제어하고 관리하는 것
- **특징**: 물리적으로 서버에 접근하지 않아도 되므로 업무 효율성이 올라가고 문제 발생 시 신속한 조치를 취할 수 있음

### 1. Telnet (텔넷)
- **설명**: 현재는 많이 사용되지 않지만 오랫동안 사용되어왔던 원격 접속 프로토콜
- **보안**: 오래된 방식이기 때문에 현재는 텔넷 자체만으로 사용을 하지 않음. 텔넷 + 보안 기능을 더해서 사용하거나, 다른 프로토콜 사용이 권장됨

> **xinetd (슈퍼 데몬)**
> - 데몬을 관리하는 데몬
> - 관리하는 서비스가 호출되면 서비스를 실행하고, 호출이 끝나면 해당 서비스를 종료함

#### [관리 정보]
* **패키지 이름**: `xinetd`, `telnetd`
* **설정 파일**: `/etc/xinetd.d/telnet`
* **데몬 이름**: `xinetd`

---

### 🛠 systemctl 명령어
*서비스 및 데몬 관리 명령어*

| 명령어 | 설명 |
| :--- | :--- |
| **start** | 서비스 시작 |
| **stop** | 서비스 중지 |
| **restart** | 재시작 |
| **status** | 서비스 상태 확인 |
| **enable** | 서비스 활성화 |
| **disable** | 서비스 비활성화 |

---

### 🛡 ufw 명령어
*우분투 방화벽*

- `# ufw enable`: 방화벽 활성화
- `# ufw disable`: 방화벽 비활성화
- `# ufw status [numbered]`: 방화벽 상태 및 규칙 확인 (numbered 옵션 시 규칙 번호 출력)
- `# ufw allow [port/protocol]`: 허용 규칙 추가
- `# ufw deny [port/protocol]`: 거부 규칙 추가
- `# ufw delete [규칙 번호]`: 규칙 번호를 사용해 규칙 제거함

---

### 🔌 Port (포트)
- 네트워크 통신을 위해 사용되는 가상의 접속 지점
- 각각의 포트는 특정 프로세스 및 서비스가 예약되어 있음 (0 ~ 65535 존재)

**[비유 예시]**
- 1층: 짱구 / 2층: 철수 / 3층: 맹구
- ...
- 22층: **ssh**
- 23층: **telnet**
- 80층: **http**

---

### 🌐 TCP / UDP

| 구분 | TCP (연결 지향형) | UDP (비연결 지향형) |
| :--- | :--- | :--- |
| **연결 방식** | 송수신 대상 간 1:1 연결 통신 | 송수신 대상 간 연결하지 않음 |
| **신뢰성** | 높음 (데이터 손실/혼잡 방지 메커니즘) | 낮음 |
| **속도** | 느림 | 빠름 |

---

### 2. SSH ( Secure Shell )
- **정의**: 텔넷을 대체하고자 등장한 암호화된 통신을 제공하는 원격 접속 프로토콜
- **용도**: 서버 원격 접속 및 파일 전송

#### [SSH 접속 명령어]
1. `ssh [상대방 주소]`: 현재 사용자와 같은 계정명으로 원격 접속 시도
2. `ssh -l [계정명] [상대방 주소]`: 접속 시 사용할 계정을 명시하며 접속 시도
3. `ssh [계정명]@[상대방 주소]`: 접속 시 사용할 계정을 명시하며 접속 시도
4. `ssh [계정명]@[상대방 주소] -p [포트 번호]`: 특정 포트를 지정하며 접속 시도

#### [설정 파일 및 옵션]
- **설정 파일**: `/etc/ssh/sshd_config`

| 라인 | 설정 옵션 | 설명 |
| :--- | :--- | :--- |
| 14 | **#Port 22** | SSH 통신에 사용할 포트 번호 지정 |
| 15 | **#AddressFamily any** | IP 주소 버전 지정 (`inet`: IPv4, `inet6`: IPv6, `any`: 모두) |
| 32 | **#LoginGraceTime 2m** | 로그인 인증 최대 시간 |
| 33 | **#PermitRootLogin** | root 사용자 로그인 허용 여부 (`yes` 시 로그인 가능) |
| 35 | **#MaxAuthTries 6** | 패스워드 입력 시도 횟수 |
| 36 | **#MaxSessions 10** | 하나의 IP 당 열 수 있는 최대 세션 개수 |

- **데몬 이름**: `sshd`



---

## 8. DNS 서버 구성

### 8.1 DNS 설정 정보

| 항목 | 설정값 |
|------|--------|
| DNS 서버 IP | 192.168.108.10 |
| test1.it.com | 192.168.108.10 |
| test2.it.com | 192.168.108.20 |

**조건:**
- 방화벽 enable

---

## 9. DNS 서버(서버1) 구성

### 9.1 방화벽 설정

```bash
# ufw enable
# ufw allow 22/tcp
# ufw allow 53/udp
```

**설명:**
- ufw 방화벽 활성화
- SSH 포트(22/tcp) 허용
- DNS 포트(53/udp) 허용

---

### 9.2 BIND9 패키지 설치

```bash
# apt install -y bind9
```

---

### 9.3 DNS 영역 설정

```bash
# vi /etc/bind/named.conf.default-zones
```

**설정 파일 내용:**
```zone
zone "it.com" {
        type master;
        file "/etc/bind/it.com.zone";
};
```

**설정 설명:**

| 항목 | 값 | 설명 |
|------|-----|------|
| zone | it.com | 영역명 |
| type | master | 주 DNS 서버 |
| file | /etc/bind/it.com.zone | 영역 파일 위치 |

---

### 9.4 DNS 레코드 파일 생성

```bash
# vi /etc/bind/it.com.zone
```

**설정 파일 내용:**
```zone
$TTL    604800
@       IN      SOA     it.com. mail.it.com. (
                        0       ; Serial
                        604800  ; Refresh
                        86400   ; Retry
                        2419200 ; Expire
                        604800 ) ; Negative Cache TTL
        IN      NS      it.com.
        IN      A       192.168.108.10
test1   IN      A       192.168.108.10
test2   IN      A       192.168.108.20
```

**DNS 레코드 설명:**

| 레코드 | 타입 | 값 | 설명 |
|--------|------|-----|------|
| it.com. | SOA | mail.it.com. | 권한의 시작 레코드 |
| it.com. | NS | it.com. | 네임서버 |
| it.com. | A | 192.168.108.10 | 루트 도메인 → 192.168.108.10 |
| test1 | A | 192.168.108.10 | test1.it.com → 192.168.108.10 |
| test2 | A | 192.168.108.20 | test2.it.com → 192.168.108.20 |

**SOA 레코드 필드:**

| 필드 | 값 | 설명 |
|------|-----|------|
| Serial | 0 | 영역 버전 번호 |
| Refresh | 604800 | 보조 DNS 새로고침 간격 (초) |
| Retry | 86400 | 재시도 간격 (초) |
| Expire | 2419200 | 만료 시간 (초) |
| Negative Cache TTL | 604800 | 부정 캐시 TTL (초) |

---

### 9.5 BIND9 데몬 재시작 및 확인

```bash
# systemctl restart bind9
# systemctl status bind9
```

---

## 설정 완료 체크리스트

### MariaDB 설정
- [ ] 방화벽 활성화 및 포트 허용 (22/tcp, 3306/tcp)
- [ ] MariaDB 설치
- [ ] itbank 사용자 권한 부여
- [ ] bind-address 주석 처리
- [ ] MariaDB 재시작
- [ ] 클라이언트 연결 확인
- [ ] 데이터베이스 및 테이블 생성
- [ ] 데이터 입력 완료

### DHCP 설정
- [ ] 방화벽 활성화 및 포트 허용 (22/tcp, 67/udp)
- [ ] isc-dhcp-server 설치
- [ ] dhcpd.conf 설정 완료
- [ ] DHCP 서버 재시작
- [ ] 클라이언트 IP 할당 확인

### DNS 설정
- [ ] 방화벽 활성화 및 포트 허용 (22/tcp, 53/udp)
- [ ] BIND9 설치
- [ ] named.conf.default-zones 설정 완료
- [ ] it.com.zone 파일 생성
- [ ] BIND9 재시작
- [ ] DNS 이름 해석 확인