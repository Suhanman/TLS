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

## 4. FTP 클라이언트 서버 설정
  25 anonymous_enable=NO
- 익명 계정 활성화 여부

28 local_enable=YES
- 일반 계정 활성화 여부

31 #write_enable=YES
- 일반 계정 업로드 허용 여부

35 #local_umask=022
- 일반 사용자 업로드 시 umask 값

103 #ftpd_banner=~~~
banner_file=/etc/vsftpd_banner
- ftp 접속 시 출력되는 배너 설정



* chroot
- 접속한 사용자의 홈 디렉터리를
  최상위 디렉터리로 인식 시키는 설정

- 사용자가 이용할 수 있는
  디렉터리를 제한한다

124 chroot_list_enable=YES
- chroot 설정이 적용될 사용자를
  지정할 chroot list 설정 활성화

126 chroot_list_file=/etc/vsftpd.chroot_list
- list 파일 위치 설정

1) chroot_local_user=NO
- list 파일에 명시된 사용자들 chroot 적용

2) chroot_local_user=YES
- list 파일에 명시된 사용자들 chroot 미적용

127 allow_writeable_chroot=YES
- chroot 적용된 사용자들의 쓰기 권한 허용
- chroot 설정은 기본적으로 쓰기 권한을 허용하지 않는다





















* umask
- 파일, 디렉터리의 초기 허가권 값을
  제어하는 값

  r w x r - x r - x   =   7 5 5

r = 4
w= 2
x = 1
- = 0


0 2 2 = - - -   - w -   - w -

디렉터리 777
파일 666
umask 022


777 = r w x r w x r w x
0 2 2 = - - -  - w -  - w -
==============
r w x  r - x  r - x





