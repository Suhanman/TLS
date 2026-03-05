# 🌐 네트워크 서비스 설정 가이드

## 1. DHCP (Dynamic Host Configuration Protocol)
IP 주소를 자동으로 할당하기 위한 서비스 설정 정보입니다.

* **패키지 이름:** `isc-dhcp-server`
* **설정 파일:** `/etc/dhcp/dhcpd.conf`
* **데몬 이름:** `isc-dhcp-server`
* **방화벽 포트:** `67/udp`

---

## 2. FTP (File Transfer Protocol)
파일 전송 프로토콜로, 서버와 클라이언트 간의 파일을 안전하게 전송합니다.

### 📂 FTP 서버 종류
1.  **로컬(Local) FTP 서버**
    * 시스템의 일반 사용자들이 접속하여 사용하는 설정
    * 일반 사용자들의 보안 인증을 사용
2.  **익명(Anonymous) FTP 서버**
    * 익명의 사용자가 파일을 공유하기 위해 사용하는 설정
    * `anonymous` 계정으로 특별한 인증 절차 없이 접속 가능

### 🔌 FTP 서버 포트

| 포트/프로토콜 | 역할 | 설명 |
| :--- | :--- | :--- |
| **20/tcp** | Data Port | 실질적인 파일을 전송하는 데 사용 |
| **21/tcp** | Command Port | 클라이언트-서버 간 제어 명령 전달 |

---

## 3. vsftpd 설정 (Very Secure FTP Daemon)

### 기본 정보
* **패키지 이름:** `vsftpd`
* **데몬 이름:** `vsftpd`

### 주요 환경 설정 (`/etc/vsftpd.conf`)
| 행 번호 | 설정 옵션 | 설명 |
| :--- | :--- | :--- |
| **25** | `anonymous_enable=NO` | 익명 계정 활성화 여부 |
| **28** | `local_enable=YES` | 일반 계정 활성화 여부 |

---

## 4. FTP 클라이언트 명령어
FTP 접속 후 사용하는 주요 명령어 모음입니다.

| 명령어 | 기능 | 설명 |
| :--- | :--- | :--- |
| `dir` | 파일 목록 확인 | 현재 디렉토리의 파일 리스트 표시 |
| `pwd` | 위치 확인 | FTP 서버 상의 현재 경로 표시 |
| `get` | 다운로드 | 서버의 파일을 로컬로 가져오기 |
| `put` | 업로드 | 로컬의 파일을 서버로 전송하기 |
| `cd` | 위치 이동 | FTP 서버 내 디렉토리 이동 |
| `lcd` | 로컬 이동 | 접속한 PC(로컬)의 디렉토리 이동 |
| `quit` | 종료 | FTP 연결 끊기 및 프로그램 종료 |

## 5. FTP 클라이언트 서버 설정
### vsftpd 서버 주요 설정 (`vsftpd.conf`)
서버의 보안과 사용자 권한을 제어하는 핵심 설정값입니다.

* **계정 활성화**
    * `25` **anonymous_enable=NO**: 익명 계정 접속 차단
    * `28` **local_enable=YES**: 서버 로컬 계정 사용자 접속 허용
* **권한 및 배너**
    * `31` **#write_enable=YES**: 일반 계정의 쓰기(업로드 등) 권한 허용 여부
    * `35` **#local_umask=022**: 파일 업로드 시 적용될 기본 umask 값
    * `103` **#ftpd_banner=...**: 접속 시 출력될 환영 메시지 설정
    * **banner_file=/etc/vsftpd_banner**: 배너 내용을 별도 파일로 관리할 경우 설정

---

## 3. chroot (사용자 격리 설정)
접속한 사용자의 **홈 디렉터리를 최상위 디렉터리(`/`)로 인식**시켜, 상위 디렉터리로의 접근을 제한하는 보안 설정입니다.

### 관련 설정 (Line 124~)
* `124` **chroot_list_enable=YES**: 특정 사용자를 지정할 리스트 기능 활성화
* `126` **chroot_list_file=/etc/vsftpd.chroot_list**: 리스트 파일 경로 지정
* `127` **allow_writeable_chroot=YES**: chroot가 적용된 디렉터리에 쓰기 권한 허용 (보안상 기본값은 거부)

### 설정 조합에 따른 동작
| 설정 항목 | `chroot_local_user=NO` | `chroot_local_user=YES` |
| :--- | :--- | :--- |
| **동작 방식** | 리스트에 **명시된** 사용자만 격리 | 리스트에 **명시되지 않은** 사용자만 격리 |
| **특징** | 화이트리스트 방식 | 블랙리스트 방식 |

---

## 4. umask (기본 허가권 제어)
파일이나 디렉터리 생성 시 부여되는 **초기 허가권(Permission)** 값을 제어합니다.

### 권한 점수 계산
* **r (read):** 4
* **w (write):** 2
* **x (execute):** 1

> **예시:** `rwxr-xr-x` → (4+2+1) (4+0+1) (4+0+1) = **755**

### umask 적용 원리 (022 기준)
기본 최대 권한에서 umask 값을 **뺀** 결과가 실제 권한이 됩니다.

| 구분 | 최대 권한 | umask (022) | 최종 권한 | 권한 표기 |
| :--- | :---: | :---: | :---: | :--- |
| **디렉터리** | 777 | 022 | **755** | `rwxr-xr-x` |
| **파일** | 666 | 022 | **644** | `rw-r--r--` |

---


---

## 5. DHCP 서버 구성

### 5.1 작업 조건

| 조건 | 내용 |
|------|------|
| 조건 1 | 클라이언트 2개 사용 (필요시 클론 생성) |
| 조건 2 | 방화벽 활성화 상태로 작업 |

### 5.2 DHCP 설정 정보

| 항목 | 설정값 |
|------|--------|
| DHCP 서버 IP | 192.168.108.10 |
| DHCP 할당 범위 | 192.168.108.100 ~ 192.168.108.109 |
| 게이트웨이 | 192.168.108.2 |
| 서브넷 마스크 | 255.255.255.0 |
| DNS 서버 | 168.126.63.1 |

### 5.3 클라이언트 구성

| 클라이언트 | IP 설정 | 설정값 |
|----------|--------|--------|
| W10-1 | 자동 부여 | DHCP 자동할당 |
| W10-2 | 고정 부여 | 192.168.108.109 |

---

## 6. DHCP 서버(서버1) 구성

### 6.1 방화벽 설정

```bash
# ufw enable
# ufw allow 22/tcp
# ufw allow 67/udp
```

**설명:**
- ufw 방화벽 활성화
- SSH 포트(22/tcp) 허용
- DHCP 포트(67/udp) 허용

---

### 6.2 DHCP 서버 패키지 설치

```bash
# apt install -y isc-dhcp-server
```

---

### 6.3 DHCP 설정 파일 작성

```bash
# vi /etc/dhcp/dhcpd.conf
```

**설정 파일 내용:**
```dhcp
subnet 192.168.108.0 netmask 255.255.255.0 {
        range 192.168.108.100 192.168.108.109;
        option routers 192.168.108.2;
        option subnet-mask 255.255.255.0;
        option domain-name-servers 168.126.63.1;
        host Win10-2 {
        hardware ethernet 00:0C:29:DE:28:4F;
        fixed-address 192.168.108.109;
        }
}
```

**설정 설명:**

| 항목 | 값 | 설명 |
|------|-----|------|
| subnet | 192.168.108.0 | 서브넷 주소 |
| netmask | 255.255.255.0 | 서브넷 마스크 |
| range | 192.168.108.100 ~ 192.168.108.109 | DHCP 할당 범위 |
| routers | 192.168.108.2 | 기본 게이트웨이 |
| subnet-mask | 255.255.255.0 | 서브넷 마스크 옵션 |
| domain-name-servers | 168.126.63.1 | DNS 서버 |
| hardware ethernet | 00:0C:29:DE:28:4F | Win10-2 MAC 주소 |
| fixed-address | 192.168.108.109 | Win10-2 고정 IP |

---

### 6.4 DHCP 데몬 재시작 및 확인

```bash
# systemctl restart isc-dhcp-server
# systemctl status isc-dhcp-server
```

---

## 7. DHCP 클라이언트(Windows) 설정 확인

### 7.1 IP 주소 갱신 명령어

```bash
> ipconfig /release
```

**설명:** 현재 할당받은 IP 주소 해제

```bash
> ipconfig /renew
```

**설명:** DHCP 서버에서 새로운 IP 주소 할당받기

```bash
> ipconfig /all
```

**설명:** 현재 네트워크 설정 정보 확인

---