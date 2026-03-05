# Docker (도커)

> 컨테이너 기반의 가상화 플랫폼
> 컨테이너라는 단위로 데이터, 자원, 소프트웨어를 격리시켜주는 소프트웨어

---

## 목차

1. [컨테이너란?](#1-컨테이너란)
2. [컨테이너 vs 가상 머신](#2-컨테이너-vs-가상-머신)
3. [도커 설치 패키지](#3-도커-설치-패키지)
4. [도커 주요 명령어](#4-도커-주요-명령어)
5. [도커 이미지](#5-도커-이미지)
6. [포트 포워딩](#6-포트-포워딩)
7. [컨테이너 내부 접속](#7-컨테이너-내부-접속)
8. [컨테이너 파일 복사](#8-컨테이너-파일-복사)

---

## 1. 컨테이너란?

- 소프트웨어를 실행시키기 위해 생성된 **격리 공간**
- 특정 소프트웨어를 실행하기 위한 **모든 환경이 포함**되어 있다
- 도커(Docker) → 컨테이너를 관리하는 플랫폼

---

## 2. 컨테이너 vs 가상 머신

| 구분 | 컨테이너 | 가상 머신 (VM) |
|------|----------|----------------|
| 가상화 수준 | 운영체제 수준 | 하드웨어 수준 |
| 커널 공유 | 호스트의 커널 공유 | 독립된 커널 사용 |
| 무게(크기) | 매우 가볍다 | 상대적으로 무겁다 |
| 생성/삭제 속도 | 매우 빠르다 | 상대적으로 느리다 |
| 격리 수준 | VM에 비해 낮다 | 컨테이너에 비해 높다 |
| 자원 소비 | 적음 | 많음 |
| 자원 할당량 설정 | — | 설정 가능 |

---

## 3. 도커 설치 패키지

```bash
apt install -y docker.io
```

---

## 4. 도커 주요 명령어

| 명령어 | 동작 |
|--------|------|
| `run` | 컨테이너 생성 + 실행 |
| `start` | 컨테이너 실행 |
| `stop` | 컨테이너 중지 |
| `rm` | 컨테이너 삭제 |
| `ps` | 실행 중인 컨테이너 확인 |
| `ps -a` | 모든 컨테이너 확인 |

### 사용 예시

```bash
# 컨테이너 생성 및 실행
docker run --name itbank -d ubuntu/apache2
#                                └── (이미지)

# 컨테이너 실행
docker start itbank

# 컨테이너 목록 확인
docker ps
docker ps -a
```

---

## 5. 도커 이미지

- 도커 컨테이너를 생성하는 데 사용되는 이미지
- 해당 이미지에 관련된 소프트웨어를 위해 필요한 **모든 것이 포함**되어 있다
- 주로 **Docker Hub** 와 같은 리포지토리에 저장되고 공유된다

---

## 6. 포트 포워딩

> 호스트 포트로 접근 시 컨테이너 포트로 연결해주는 옵션

```bash
# 문법
-p [호스트 포트]:[컨테이너 포트]

# 예시
-p 8081:80
```

### 적용 예시

```bash
docker run --name itbank -d -p 8081:80 ubuntu/apache2
```

---

## 7. 컨테이너 내부 접속

```bash
# 접속
docker exec -i -t [컨테이너 이름] /bin/bash

# 탈출
exit
```

---

## 8. 컨테이너 파일 복사

```bash
# 문법
docker cp [원본] [복사 경로]
```

> **컨테이너 경로 표현:** `[컨테이너 이름]:[컨테이너 상의 경로]`

### 예시

```bash
# 호스트 → 컨테이너
docker cp ./index.html itbank:/var/www/html/

# 컨테이너 → 호스트
docker cp itbank:/var/www/html/index.html ./
```
# Docker Compose 및 WordPress 설정

---

## 1. Docker Compose (도커 컴포즈)

### 개요

도커 컨테이너 기반의 애플리케이션을 정의하고 실행하기 위한 도구입니다.

**특징:**
- 컨테이너에 관한 설정을 하나의 파일(정의 파일)로 작성
- 한 번에 여러 개의 컨테이너를 실행하고 종료 가능
- 복잡한 다중 컨테이너 환경을 간편하게 관리

---

### 1.1 설치 및 기본 정보

**패키지 이름:**
```
docker-compose
```

**정의 파일 이름:**
```
docker-compose.yml
```

---

### 1.2 주요 명령어

| 명령어 | 설명 |
|--------|------|
| `docker-compose up` | 도커 컴포즈 실행 |
| `docker-compose down` | 도커 컴포즈 종료 (생성된 모든 컨테이너 제거) |

**사용 예:**
```bash
# docker-compose up
# docker-compose down
```

---

## 2. WordPress (워드프레스)

### 개요

사용자가 전문적인 웹 개발 지식 없이도 웹 사이트를 만들 수 있게 해주는 오픈소스 소프트웨어입니다.

---

### 2.1 필수 패키지 목록

| 번호 | 패키지명 |
|------|---------|
| 1 | apache2 |
| 2 | ghostscript |
| 3 | libapache2-mod-php |
| 4 | php |
| 5 | php-bcmath |
| 6 | php-curl |
| 7 | php-imagick |
| 8 | php-intl |
| 9 | php-json |
| 10 | php-mbstring |
| 11 | php-mysql |
| 12 | php-xml |
| 13 | php-zip |

**설치 확인:**
```bash
# echo "<?php phpinfo(); ?>" > /var/www/html/info.php
```

---

## 3. WordPress 설치 및 설정

### 3.1 데이터베이스 설정

#### Step 1: MariaDB 설치
```bash
# apt install -y mariadb-server
```

#### Step 2: DBMS 접속
```bash
# mysql -u root -p
```

#### Step 3: 데이터베이스 및 사용자 생성
```sql
> create database wp_db;
> grant all privileges on wp_db.* to 'wp_user'@'192.168.108.%' identified by 'It1';
> flush privileges;
```

**설명:**
- `wp_db`: WordPress용 데이터베이스명
- `wp_user`: 데이터베이스 사용자명
- `192.168.108.%`: 192.168.108 대역의 모든 호스트에서 접속 허용
- `It1`: 사용자 비밀번호

#### Step 4: MariaDB 원격 접속 활성화
```bash
# vi /etc/mysql/mariadb.conf.d/50-server.cnf
```

**수정 내용:**
```ini
27 #bind-address            = 127.0.0.1
```

주석 처리하여 모든 인터페이스에서 접속 가능하도록 설정

---

### 3.2 WordPress 설치

#### Step 1: WordPress 다운로드
```bash
# wget https://wordpress.org/wordpress-6.2.zip
```

#### Step 2: 압축 해제
```bash
# unzip wordpress-6.2.zip
```

---

### 3.3 WordPress 설정 파일 구성

#### Step 1: 설정 파일 생성
```bash
# cp wordpress/wp-config-sample.php wordpress/wp-config.php
```

#### Step 2: 설정 파일 수정
```bash
# vi wordpress/wp-config.php
```

**설정 내용:**
```php
/** The name of the database for WordPress */
define( 'DB_NAME', 'wp_db' );

/** Database username */
define( 'DB_USER', 'wp_user' );

/** Database password */
define( 'DB_PASSWORD', 'It1' );

/** Database hostname */
define( 'DB_HOST', '192.168.108.10' );
```

| 라인 | 설정값 | 설명 |
|------|--------|------|
| 22 | `DB_NAME` | WordPress 데이터베이스명 |
| 25 | `DB_USER` | 데이터베이스 사용자명 |
| 28 | `DB_PASSWORD` | 데이터베이스 사용자 비밀번호 |
| 31 | `DB_HOST` | 데이터베이스 서버 호스트 |

---

### 3.4 WordPress 파일 복사

#### Step 1: 웹 서버 디렉토리로 복사
```bash
# cp -r wordpress/*  /var/www/html/
```

---

### 3.5 권한 설정

#### Step 1: 디렉토리 소유권 설정
```bash
# chown www-data: /var/www/
```

#### Step 2: 디렉토리 권한 설정
```bash
# find /var/www/ -type d -exec chmod 2755 {} \;
```

**설명:**
- 디렉토리에 대한 읽기, 쓰기, 실행 권한 설정

#### Step 3: 파일 권한 설정
```bash
# find /var/www/ -type f -exec chmod 664 {} \;
```

**설명:**
- 파일에 대한 읽기, 쓰기 권한 설정

---

### 3.6 Apache 재시작 및 테스트

#### Step 1: 기본 index.html 제거
```bash
# rm /var/www/html/index.html
```

#### Step 2: Apache 데몬 재시작
```bash
# systemctl restart apache2
```

#### Step 3: 접속 테스트
웹 브라우저에서 서버의 IP 또는 도메인으로 접속하여 WordPress 설치 화면이 표시되는지 확인

---

## 설치 프로세스 요약

```
1. MariaDB 설치 및 DB 설정
   ├─ MariaDB 설치
   ├─ 데이터베이스 생성 (wp_db)
   ├─ 사용자 생성 (wp_user)
   └─ 원격 접속 설정

2. WordPress 설치
   ├─ 다운로드
   ├─ 압축 해제
   └─ 파일 복사

3. WordPress 설정
   ├─ wp-config.php 생성
   ├─ 데이터베이스 정보 입력
   └─ 권한 설정

4. Apache 구성 및 재시작
   ├─ 기본 파일 제거
   ├─ Apache 재시작
   └─ 접속 테스트
```

---

## 참고 사항

### 권한 설정 상세 설명

| 명령어 | 대상 | 권한 | 설명 |
|--------|------|------|------|
| `chown www-data:` | `/var/www/` | 소유권 | Apache 웹 서버 사용자(www-data)에게 소유권 부여 |
| `chmod 2755` | 디렉토리 | rwxr-sr-x | 읽기, 쓰기, 실행 권한 + 그룹 ID 상속 |
| `chmod 664` | 파일 | rw-rw-r-- | 읽기, 쓰기 권한 (소유자 및 그룹) |

### 데이터베이스 사용자 권한

```sql
grant all privileges on wp_db.* to 'wp_user'@'192.168.108.%' identified by 'It1';
```

- `all privileges`: 모든 권한 부여
- `wp_db.*`: wp_db 데이터베이스의 모든 테이블
- `'wp_user'@'192.168.108.%'`: 192.168.108.0 ~ 192.168.108.255 범위의 호스트에서만 접속 가능

### 주요 파일 위치

| 파일/디렉토리 | 설명 |
|-------------|------|
| `/var/www/html/` | Apache 웹 루트 디렉토리 |
| `/etc/mysql/mariadb.conf.d/50-server.cnf` | MariaDB 설정 파일 |
| `wordpress/wp-config.php` | WordPress 설정 파일 |