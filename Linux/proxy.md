
## 5. Proxy (프록시)

> **'대리인'** 역할을 수행하는 서버  
> 웹 클라이언트와 웹 서버 사이에서 데이터를 상호 전달해주는 중간 역할

```
user ← proxy ← web
user → proxy → web
```

### 프록시 장점

| 번호 | 장점 |
|------|------|
| 1 | 웹 서핑 속도 향상 |
| 2 | 검색 제한을 피하기 위해 사용 |
| 3 | 특정 사이트의 접근 차단 |

---

### 패키지 및 설정

| 항목 | 값 |
|------|----|
| 패키지 이름 | `squid` |
| 설정 파일 | `/etc/squid/squid.conf` |
| 포트 번호 | `3128/tcp` |

---

### squid.conf 주요 설정

#### ACL 정의

```
acl [ACL명] src [IP 대역]
```

> **ACL (Access Control List)** : 특정 이용자를 정의하고 해당 이용자들에 대한 접근을 허용·차단할 때 사용  
> 번호·이름을 사용하여 정책을 생성하고 해당 정책으로 접근을 필터링

#### 접근 제어

```
http_access [allow/deny] [ACL명]
```

| 키워드 | 의미 |
|--------|------|
| `allow` | 허용 |
| `deny` | 거부 |

#### 캐시 설정

```
cache_dir ufs /var/spool/squid 1000 16 256
```

> 캐시 저장과 관련된 설정

#### ACL 예시

```
acl localnet src 192.168.108.0/255.255.255.0
```

---

## 6. 부하 분산 (Load Balancing)

> 1번 서버가 고장나면 전체 서비스가 마비될 수 있음  
> 1대보다 2대, 2대보다 3대가 더 안정적

### 패키지 및 설정

| 항목 | 값 |
|------|----|
| 패키지 이름 | `haproxy` |
| 설정 파일 | `/etc/haproxy/haproxy.cfg` |

---

### haproxy.cfg 설정 예시

```haproxy
frontend itbank_front
        bind *:80
        default_backend itbank_back

backend itbank_back
        balance roundrobin
        server srv2 192.168.108.20:80 check
        server srv3 192.168.108.30:80 check
```

| 항목 | 설명 |
|------|------|
| `frontend` | 클라이언트 요청을 받는 진입점 설정 |
| `bind *:80` | 모든 IP의 80번 포트에서 수신 |
| `default_backend` | 요청을 전달할 백엔드 지정 |
| `backend` | 실제 서비스를 제공하는 서버 그룹 설정 |
| `balance roundrobin` | 라운드 로빈 방식으로 서버에 부하 분산 |
| `server` | 백엔드 서버 등록 (`check`: 상태 확인 활성화) |
