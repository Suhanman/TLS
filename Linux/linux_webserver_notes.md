# 종합 실습 & 웹 서버 설정 노트

---

## 목차

1. [종합 실습](#1-종합-실습)
   - 초기 셋팅
   - 리눅스 업데이트
   - 하드 작업
   - 사용자 계정 설정
   - 용량 저장
   - 웹 사이트 변경
2. [웹 서버 (Web Server)](#2-웹-서버-web-server)
   - 개요
   - 패키지 및 설정 파일
   - apache2.conf 주요 설정
   - LogLevel 단계
   - 디렉터리 설정
   - 모듈 관리

---

## 1. 종합 실습

> ⚠️ **기존 설정 제거 후, 새로 압축 풀기부터 진행**

---

### Step 0. 초기 셋팅

- 네트워크 IP : **수동 설정**
- 초기 비밀번호 설정

---

### Step 1. 리눅스 최신 업데이트

```bash
apt update
apt upgrade
```

---

### Step 2. 하드 작업

| 항목 | 설정값 |
|------|--------|
| HDD | 1GB (새로 추가) |
| Partition 1 | 150M |
| Partition 2 | 나머지 용량 할당 |
| M.P 1 | `/mount/mp1/` |
| M.P 2 | `/mount/mp2/` |
| 소유 계정 | `user1` |
| 마운트 상태 | **활성화 (mount 동작 중)** |

---

### Step 3. 사용자 계정 설정

| 계정 | 쉘 | 홈 디렉터리 | 비고 |
|------|----|-------------|------|
| `user1` | `/bin/bash` | 기본 (`/home/user1/`) | 홈 디렉터리 안에 `compress/` 자동 생성 |
| `user2` | `/bin/ksh93` | `/var/www/html/` 경로 안 | — |

---

### Step 4. 용량 저장

- `/bin/*` 경로의 내용물을 복사하여 저장

```
저장 위치 : user1 홈디렉터리/compress/ 안
```

> ⚠️ **저장 작업은 `user1` 계정으로 진행할 것**

---

### Step 5. 웹 사이트 변경

```
1. 웹 파일 다운로드
2. user2 계정으로 업로드
3. /var/www/html/ 로 이동 후 unzip 실행
4. 웹 사이트 화면 출력 확인
```

> ⚠️ `/var/www/html/index.html` 은 반드시 **백업** 후 진행  
> ⚠️ **모든 작업은 `user2` 계정으로 진행할 것**

---

## 2. 웹 서버 (Web Server)

> 클라이언트의 요청을 받아 웹 페이지나 콘텐츠를 제공해주는 서버

### 웹 페이지 유형

| 유형 | 설명 |
|------|------|
| **정적 웹 페이지** | 클라이언트 요청에 따라 미리 만들어진 웹 페이지를 제공 |
| **동적 웹 페이지** | 클라이언트 요청에 따라 서버에서 동적으로 콘텐츠를 생성하여 제공 |

---

### 패키지 이름

```bash
apt install -y apache2
apt install -y nginx
```

---

### 설정 파일 목록

| 파일 경로 | 설명 |
|-----------|------|
| `/etc/apache2/apache2.conf` | Apache 메인 설정 파일 |
| `/etc/apache2/sites-enabled/000-default.conf` | 기본 가상 호스트 설정 |
| `/etc/apache2/mods-enabled/dir.conf` | 디렉터리 인덱스 설정 |
| `/etc/apache2/ports.conf` | 포트 설정 파일 |

---

### apache2.conf 주요 설정

#### LogLevel (143번 줄)

```apache
LogLevel warn
```

> 로그 파일의 중요도를 조절하는 설정.  
> 설정한 수준 미만의 오류는 로그를 남기지 않는다.

**LogLevel 단계 (심각도 순)**

| 레벨 | 설명 |
|------|------|
| `emerg` | 시스템이 작동 불가할 정도의 심각한 오류 |
| `alert` | 즉시 조치가 필요한 오류 |
| `crit` | 치명적 오류 |
| `error` | 일반적 오류 |
| `warn` | 경고, 잠재적인 오류 |
| `notice` | 알림, 주목할 만한 정보 |
| `info` | 일반적인 정보 |
| `debug` | 문제 해결을 위한 매우 상세한 정보 |

---

#### 모듈 포함 설정 (146~147번 줄)

```apache
IncludeOptional mods-enabled/*.load
IncludeOptional mods-enabled/*.conf
```

> 모듈 파일을 설정 파일에 포함하는 설정

---

#### 디렉터리 접근 권한 블록

```apache
<Directory /> ... </Directory>
```

> 디렉터리에 대한 접근 권한 및 설정을 정의하는 블록

| 키워드 | 의미 |
|--------|------|
| `denied` | 거부 |
| `granted` | 허용 |

---

### sites-enabled/000-default.conf 주요 설정

#### DocumentRoot (12번 줄)

```apache
DocumentRoot /var/www/html
```

> `index.html` 파일이 위치할 디렉터리 설정  
> ⚠️ 경로 끝에 `/` 를 붙이지 않는다

---

### mods-enabled/dir.conf 주요 설정

#### DirectoryIndex

```apache
DirectoryIndex index.html
```

> 웹 사이트 페이지로 사용할 index 파일의 이름을 정의하는 설정

---

### 포트 설정

```apache
# /etc/apache2/ports.conf
```

> IP 주소 뒤에 포트 번호를 `:port` 형식으로 지정

```
http://[IP 주소]:[포트번호]
```

---

### 모듈 관리

| 경로 | 설명 |
|------|------|
| `/etc/apache2/mods-available/` | 사용 가능한 모듈이 위치한 디렉터리 |
| `/etc/apache2/mods-enabled/` | 현재 사용 중인 모듈이 위치한 디렉터리 |

```bash
# 모듈 활성화
a2enmod [모듈명]

# 모듈 비활성화
a2dismod [모듈명]
```
