다음의 작업을 수행하시오

(방화벽 enable 상태로 작업)

1. telnet 접속 가능

2. ssh 를 다음의 조건에 맞게 설정하시오.

2-1. root 로그인 가능

2-2. 패스워드 2회 틀리면 연결 차단

2-3. port 65000 번 사용

1.

ufw allow 23/tcp

ufw allow 22/tcp

vi /etc/xinetd.d/telnet

service telnet

{

----------------------------

2.

#vi /etc/ssh/sshd_config

systemctl restart sshd

systemctl status sshd - 데몬 재실행 및 확인 .

(root) #passwd

-관리자 PW 설정.

ufw allow 65000(ssh에서 여는 포트 허용)

-방화벽 설정

보안키 = 데이터를 암호화 하거나, 복호화하는데 사용되는 암호화 기술

대칭키 => 데이터를 암호화 하고 복호화하는데 같은 키를 사용한다.

          => 암호화, 복호화를 같은 키를 사용해 처리 속도가 빠르다!

          => 단점 둘 중 하나라도 뚫리면 다 뚫려버려요~

          => 키 노출에 대한 리스크가 크다

비대칭키 => 데이터를 암호화하는 공개키, 데이터를 복호화는 개인 키.

	    한 쌍으로 사용된다.

	   공개키는 외부에 공개되어 노출되어도 상관 x

	    개인키는 외부에 노출 x 사용자가 안전하게 보관 ~

	   공개 키로 암호화된 데이터는 해당 공개키와 짝이 되는 개인키로만 복호화 가능.

-----------------------------------------

scp

(ssh+copy) => 원격 접속 프로토콜을 이용한 파일 복사.

scp [원본 파일] [복사할 위치]

scp itbank@192.168.108.10:[원격지 경로]

----------------------------------------

비대칭키

.ssh 에 만들어진 키를 리눅스에 업로드 하고 그후에 mobxtaem 뭐시기에 세션 만들어서 개인키로 들어갈 수 있도록 세팅.

ssh -keygen -t rsa

scp id_rsa.pub itbank@192.168.108.10:/home/itbank

--------------------------------------

DHCP (Dynamic Host Configuration Protocol)

- 동적 호스트 설정 프로토콜

- 네트워크에서 클라이언트 컴퓨터에 네트워크 설정을 할당해주는 프로토콜

- 클라이언트가 네트워크에 대한 지식이 없어도 네트워크를 사용 가능하다.

---------------

[패키지 이름]

isc-dhcp-server

------------------

설정파일

/etc/dhcp/dhcpd.conf

->

subnet 192.168.108.0 netmask 255.255.255.0(서버에서 명시){

	(클라이언트 안에 부여해지는..)

        range 192.168.108.100 192.168.108.110;

        option subnet-mask 255.255.255.0;

        option routers 192.168.108.2;(게이트웨이)

        option domain-name-servers 8.8.8.8;(DNS서버)

}

ipconfig 명령어

>ipconfig

-네트워크 설정 출력

>ipconfig /all

-네트워크 상세 정보 출력

>ipconfig /release

-네트워크 설정 반납

>ipconfig /renew

-네트워크 설정 재할당 요청

[데몬 이름]

