# 웹 서버 및 데이터베이스 설정

---

## 1. Virtual Host (가상 호스트)

### 개요
하나의 서버에서 여러 웹 사이트를 운영할 수 있게 해주는 기능.

### Apache Virtual Host 설정

**설정 파일 위치:**
```
/etc/apache2/sites-enabled/000-default.conf
```

**설정 예시:**
```apache
<VirtualHost *:65000>
    DocumentRoot /home/page
</VirtualHost>
```

**주의사항:**
- alias 접속에는 `/` 까지 붙여줘야 함

---

## 2. Nginx

### 설정 파일

**파일 위치:**
```
/etc/nginx/sites-enabled/default
```

**주요 설정 라인:**

| 라인 | 설정 | 설명 |
|------|------|------|
| 41 | `root /var/www/html;` | Document Root 설정 |
| 44 | `index.html;` | 웹사이트 HTML 설정 |

---

## 3. 데이터베이스(DB) 설정

### 3.1 기본 개념

#### 데이터 (Data)
정보의 단편적인 조각

#### 정보 (Information)
데이터를 수집하고 가공해 의미를 부여한 결과물

#### 데이터베이스 (Database)
데이터를 효율적으로 저장하고 활용할 수 있도록 체계적으로 조직화한 데이터들의 집합체

#### DBMS (Database Management System)
- **정의**: 데이터베이스 관리 시스템
- **역할**: 데이터베이스를 효율적으로 관리하고 다루기 위한 소프트웨어
- **기능**: 데이터베이스의 생성, 삭제, 수정 등의 관리 작업 수행
- **종류**: Oracle, MySQL, MariaDB 등

#### SQL (Structured Query Language)
- DBMS 내부에서 데이터를 관리하기 위해 사용되는 표준화된 언어
- 대부분의 DBMS에서 일관되게 사용 가능

---

### 3.2 데이터베이스의 구성 요소

```
DBMS
 └─ 데이터베이스
     └─ 테이블
         └─ 데이터(레코드/필드)
```

| 구성 요소 | 설명 |
|---------|------|
| **데이터베이스** | 데이터들을 체계적으로 저장하고 관리하기 위해 사용하는 시스템 |
| **테이블** | 실제 데이터들이 저장되는 구성 요소 (데이터들을 구조화해 저장한 실제 집합) |
| **레코드 / 로우 (Record, Row)** | 테이블의 행 |
| **필드 / 컬럼 (Field, Column)** | 테이블의 열 |

---

## 4. MariaDB 설정 및 명령어

### 4.1 기본 정보

**패키지 이름:**
```
mariadb-server
```

**데몬 이름:**
```
mariadb
```

**포트 번호:**
```
3306/tcp
```

**설정 파일:**
```
/etc/mysql/mariadb.conf.d/50-server.cnf
```

**원격 접속 설정:**
```ini
27 # bind-address = 127.0.0.1  (주석 처리)
```

---

### 4.2 기본 명령어

#### 화면 정리
```sql
system clear;
```
또는
```
[Ctrl + L]
```

#### 데이터베이스 관련

| 명령어 | 설명 |
|--------|------|
| `show databases;` | 전체 DB 확인 |
| `create database [DB명];` | DB 생성 |
| `drop database [DB명];` | DB 삭제 |
| `use [DB명];` | DB 선택 |

#### 테이블 관련

| 명령어 | 설명 |
|--------|------|
| `show tables;` | 테이블 확인 |
| `desc [테이블명];` | 테이블 구조 출력 |
| `drop table [테이블명];` | 테이블 삭제 |

---

### 4.3 테이블 생성

**문법:**
```sql
create table [테이블명] ([필드 정의]);
```

**예시:**
```sql
MariaDB [itbank_db]> create table itbank_tb(num int(4), name char(20));
```

---

### 4.4 MariaDB 자료형

**개요:**
테이블을 생성할 때 각 필드에 들어갈 데이터들의 자료형을 정해야 하며, 지정된 자료형에 해당하는 데이터만 입력 가능합니다.

| 자료형 | 설명 |
|--------|------|
| `int` | 정수 |
| `float` | 실수 |
| `char` | 고정 길이 문자열 (어떤 데이터를 넣든 데이터의 길이가 고정) |
| `varchar` | 가변 길이 문자열 (문자열의 길이에 맞춰 데이터의 길이가 변함) |

---

### 4.5 레코드(데이터) 관련 명령어

#### 레코드 입력
```sql
insert into [테이블명] values ([값1], [값2], ...);
```

#### 레코드 확인
```sql
select [필드명] from [테이블명];
```

#### 레코드 삭제
```sql
delete from [테이블명] where [필드] = [값];
```

**예시:**
```sql
delete from itbank_tb where num = 1;
```

#### 레코드 수정
```sql
update [테이블명] set [필드]=[값] where [필드]=[값];
```

---

### 4.6 사용자 권한 설정

#### 권한 부여
```sql
grant all privileges on *.* to 'test'@'%' identified by 'It1';
```

**설명:**
- `all privileges`: 모든 권한
- `*.*`: 모든 데이터베이스와 테이블
- `'test'@'%'`: 사용자 test, 모든 호스트에서 접속 가능

#### 특정 DB에만 권한 부여
```sql
grant all privileges on itbank_DB.* to 'test'@'%' identified by 'It1';
```

#### 권한 적용
```sql
flush privileges;
```

---

### 4.7 원격 접속

**클라이언트에서 원격 데이터베이스 접속:**
```bash
mysql -h 192.168.108.10 -u test -p
```

**옵션 설명:**
- `-h`: 호스트 IP 주소
- `-u`: 사용자명
- `-p`: 비밀번호 (입력 후 마스킹 처리)

---

## 참고 사항

### 계층 구조
```
DBMS
 ├─ 데이터베이스 (Database)
 │   ├─ 테이블 (Table)
 │   │   ├─ 레코드/로우 (Row)
 │   │   └─ 필드/컬럼 (Column)
```

### 주요 DBMS 종류
- Oracle
- MySQL
- MariaDB (MySQL의 오픈소스 포크)
- PostgreSQL
- SQL Server


# 데이터베이스 및 네트워크 서버 구성 실습

---

## 1. MariaDB 원격 데이터베이스 설정

### 1.1 작업 조건

| 조건 | 내용 |
|------|------|
| 조건 1 | 방화벽 활성화 상태로 진행 (3306/tcp) |
| 조건 2 | 서버1에서 DB 서버 구축 |
| 조건 3 | DB 클라이언트(서버2)에서 DB 접속한 상태로 작업 |

### 1.2 데이터베이스 정보

| 항목 | 설정값 |
|------|--------|
| 데이터베이스명 | my_DB |
| 테이블명 | my_TB |

---

## 2. DB 서버(서버1) 구성

### 2.1 방화벽 설정

```bash
# ufw enable
# ufw allow 22/tcp
# ufw allow 3306/tcp
```

**설명:**
- ufw 방화벽 활성화
- SSH 포트(22/tcp) 허용
- MariaDB 포트(3306/tcp) 허용

---

### 2.2 MariaDB 설치

```bash
# apt install -y mariadb-server
```

---

### 2.3 MariaDB 사용자 권한 설정

```bash
# mysql -u root -p
```

```sql
> grant all privileges on *.* to 'itbank'@'192.168.108.%' identified by 'It1';
> flush privileges;
```

**설명:**
- 사용자: `itbank`
- 비밀번호: `It1`
- 접근 범위: 192.168.108.0 대역 전체 (`192.168.108.%`)
- 권한: 모든 권한 부여

---

### 2.4 MariaDB 원격 접속 설정

```bash
# vi /etc/mysql/mariadb.conf.d/50-server.cnf
```

**수정 내용 (27번 라인):**
```ini
27 #bind-address            = 127.0.0.1
```

**설명:**
- `bind-address` 주석 처리하여 모든 인터페이스에서 접속 가능하도록 설정

---

### 2.5 MariaDB 데몬 재시작

```bash
# systemctl restart mariadb
```

---

## 3. DB 클라이언트(서버2) 구성

### 3.1 MariaDB 클라이언트 설치

```bash
# apt install -y mariadb-client
```

---

### 3.2 DB 서버 원격 접속

```bash
# mysql -h 192.168.108.10 -u itbank -p
```

**옵션 설명:**
- `-h`: 서버 IP 주소 (192.168.108.10)
- `-u`: 사용자명 (itbank)
- `-p`: 비밀번호 입력 프롬프트 표시

---

## 4. 데이터베이스 및 테이블 생성

### 4.1 데이터베이스 생성

```sql
> create database my_DB;
> use my_DB;
```

---

### 4.2 테이블 생성

```sql
> create table my_TB (
    -> name varchar(30),
    -> age int(4),
    -> email varchar(50),
    -> phone_number char(30),
    -> address varchar(200));
```

**테이블 구조:**

| 필드명 | 자료형 | 크기 | 설명 |
|--------|--------|------|------|
| name | varchar | 30 | 이름 |
| age | int | 4 | 나이 |
| email | varchar | 50 | 이메일 |
| phone_number | char | 30 | 전화번호 |
| address | varchar | 200 | 주소 |

---

### 4.3 데이터 입력

#### 방법 1: 한 번에 하나씩 입력

```sql
> INSERT INTO my_TB VALUES(
    -> 'Sam smith', 21, 'S@test.com', '010-1111-1111', 'UK');
> INSERT INTO my_TB VALUES(
    -> 'Taylor swift', 22, 'T@test.com', '010-2222-2222', 'USA');
> INSERT INTO my_TB VALUES(
    -> 'BTS', 23, 'B@test.com', '010-3333-3333', 'KOR');
```

#### 방법 2: 한 번에 여러 개 입력 (권장)

```sql
> INSERT INTO my_TB VALUES
    -> ('Sam smith', 21, 'S@test.com', '010-1111-1111', 'UK'),
    -> ('Taylor swift', 22, 'T@test.com', '010-2222-2222', 'USA'),
    -> ('BTS', 23, 'B@test.com', '010-3333-3333', 'KOR');
```

**입력 데이터:**

| 이름 | 나이 | 이메일 | 전화번호 | 주소 |
|------|------|--------|---------|------|
| Sam smith | 21 | S@test.com | 010-1111-1111 | UK |
| Taylor swift | 22 | T@test.com | 010-2222-2222 | USA |
| BTS | 23 | B@test.com | 010-3333-3333 | KOR |

---