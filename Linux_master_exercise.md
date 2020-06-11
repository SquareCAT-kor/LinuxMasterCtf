# 리눅스 마스터 1급 

## Part3 네트워크 및 서비스의 활용

### Chapter 1 네트워크 서비스

 #### 1.1 웹 관련 서비스

##### 웹 서비스 구성 요소 : 웹 서버, 웹 문서, 웹 즈라우저

* 웹 브라우저 종류
  * 파이어폭스 : Mozilla 재단에서 개발, Gecko 레이아웃 엔진 
    * OS : 윈도우, 맥 ,안드로이드
  * 오페라 Opera : ASA 에서 윈도우용으로 개발
    * 지원 OS : 리눅스 Mac OS, Android, iphone, Linux, Window 등 사용 가능
  * 크롬 Chrome : 구글에서 Webkit 레이아웃 엔진을 이용하여 개발
  * **사파리 Safari : 리눅스 OS 지원 안함**
* 웹 작동 원리
  * HTTP 프로토콜 사용 
  * 주요 메소드
    * GET : 서버에서 자료 요청 
      * 예시 `telnet www.hnu.kr 80` 입력하여 웹 서버 정보 파악 가능. **웹 서버 버전 확인**
  * HTTP 응답 코드
    * 404 : Not Found
    * 400 : Bad Request
    * 403 : Forbidden
    * 500 : Internal Server Error

#### 1.1.2 웹 관련 서비스 운영

##### 웹 서버 설치 개요

* Apache HTTP 서버 를 많이 사용 

* 리눅스에서 Linux + Apache + MySQL + PHP 을 조합하여 LAMP라 부른다.

* Apache HTTP 서버 와 PHP

  * 1.3 버전까지는 정적모듈이었으나 2.X 버전부터는 무조건 동적모듈 방식(DSO)으로 PHP를 지원한다.

* MPM ( 멀티 프로세싱 모듈)

  * **worker**  : 초기에 시작하는 프로세스의 개수를 지정, 스레드 수 초과 시 새로운 자식 프로세스를 생성
  * prefork : 아파치 1.3 프로세스 모듈과 같은 방법, 클라이언트의 요청을 대기하다가 생성 여부 관리

* Apache,PHP,MySQL 연동 설치

  * configure --help : 필요한 옵션 확인

* 소스파일 검증? : `md5sum` 또는 (sha1sum, sha256sum 등 이용)

* Apaache 설치 과정

  * `killall` -> `rpm -e `예전 버전 삭제 -> `tar` 압축 해제 -> `cd ~~`압축 풀린 디렉토리 이동 
    -> 환경 설정 `--enable-mods-shared=all` -> 컴파일`make` -> 설치 `make install`
    * `--enable-mods-shared=all` : 모두 동적모듈로 설치하도록 지정

* MySQL 설치  : `cmake ` 필수 설치

  * 이전 버전 : `mysql_install_db ` -> 5.7.6부터 `mysqld` 사용 기본 관리 데이터베이스 생성

  * mysql 설치 과정

    1.`killall mysqld` 프로세스 데몬 정지

    2.`rpm -qa | grep mysql` 관련파일 삭제

    3.`tar zxvf mysql~~` :압축해제

    4.`cd mysql~~` :경로이동

    5.`cmake~~~`: 환경설정 및 컴파일

    6.`make install` : 컴파일한것 설치

    7.`mysqld` : 기본 관리데이터베이스 설정

    ​	+ `./mysqld --initialize --user=root` : 비밀번호 설정

    8.`mysqld_safe` : 데몬 실행

    9.`mysql -p 패스워드`: 프로그램 접근

    10. mysql 접속하여 root 패스워드 변경 `alter user 'root@localhost' identified by 패스워드`

        `flush privileges` : 권한정보 갱신

* PHP 설치 

  * 환경설정 : DSO 모듈 관련 `apxs` - PHP가 아파치데몬에 동적모듈로 로딩되도록 한다.

  ---

* Apache 2.2 환경 설정

  * **httpd.conf**

    * httpd-vhosts.conf : 버추얼 호스트 사용, 여러 도메인 이름 쓸 때 사용

    * LoadModule php5_module modules/**libphp5.so **: DSO 로 사용되는 모듈 나타냄

    * `<ifModule dir_module> ... **DirectoryIndex**` : DirectoryIndex

      * 디렉터리의 인덱스 파일로 사용할 파일 이름 적음. index.html 에 index.htm, index.php 등..

    * `AddType`  : 추가로 지원될 파일을 적는 부분, mime.types 수정 없이 mime 을 사용가능.

      * `AddType application/x-httpd-php ~~~`

        `AddType application/x-httpd-php-source`

  * httpd 명령을 이용한 실행

    * `httpd -t`  응답 : `Sysntax OK` : httpd.conf 파일의 문법적 오류를 검사

  * 스크립트 파일을 이용한 실행

    * `apachectl` configtest : httpd.conf 파일의 문법적 오류 검사
    * `apachectl graceful` : 연결된 접속을 끊지 않고, 환경 설정 파일을 다시 읽어 들임.

* 개인 사용자 홈페이지 사용 설정

  * httpd-userdir.conf 에서 설정
    * UserDir 항목의 기본값은 public_html 
    * 주소 : `http://호스트명/~사용자아이디`
      * 예시 : http://www.poseing.org/~yuloje

* Apache 사용자 인증? : `htpasswd -c /etc/password posein`  **htpasswd** 기억하기

* **liphp5.so** : 동적 모듈 파일명 **LoadModule 지시자 설정 확인** 

* **php 파일 작성시 <? ~ ?> 사용하기**

  * vi php.ini -> short_open_tag = Off 를 On 으로 변경 , 재시작

* 기타 다른 서버와 연동 

  * 리눅스에서 사용하는 데이터베이스 종류
    * RDBMS : MariaDB, Oracle (MySQL ,Oracle DB)
    * NoSQL : **MongoDB**
    * JSP : **Apache Tomcat**
    * WAS 종류 
      * 자바 표준 준수 : Oracle Weblogic , IBM WebSphere, 티맥스 JEUS, 레드햇 JBoss, Cauhco Resinm , Apache Tomcat 등
      * 기타 : 마이크로소프트 .net , Zend 의 **Zend Server**
  * 웹 서버 암호화 관련 
    * SSL 은 80포트 외에 추가로 **443**포트를 사용한다.

#### 1.2 인증 관련 서비스

* 네트워크 기반 인증 서비스 : **NIS, LDAP, Active Directory **
* NIS Network Information Service 
  * Sun Microsystems 사에서 개발한 프로토콜 중 하나
  * 하나의 서버에 등록된 사용자 계정, 암호, 그룹 정보 등을 공유하여 다른 시스템에 제공하는서비스
* LDAP Lightweight Directory Access Protocol 
  * IP프로토콜 기반으로 사용자, 시스템, 네트워크 , 서비스 정보 등의 디렉터리 정보를 공유
  * 자주 변경되지 않는 정보에 사용하기 좋다.
  * 트리 구조에 의해 조직화 된다.
  * 속성 관련 키워드
    * `cn` commonname : 이름과 성의 조합
    * sn : surname : 성
    * givenName : 이름
* NIS 이용하기
  * NIS 구성: RPC (Remote Procedure Call) 사용
    * RPC 관련 데몬 실행  : 기존 **portmap** 에서 RHEL6 부터 **rpcbind** 데몬이 동작.
  * NIS **서버**용 패키지명 
    * `ypserv` : 서버 운영 주 데몬 스크립트
    * `yppasswdd` : 패스워드 적용 데몬 스크립트
    * `ypxfrd` : 맵핑 속도 높여주는 데몬
  * 도메인 설정 , 부팅 시 적용하려면?
    * **/etc/sysconfig/network** 파일에 등록
  * NIS **클라이언트**
    * `ypbind` 와 `yp-tools` 패키지 설치
  * NIS 관련 명령어
    * `ypwhich` : NIS 서버명과 관련 맵 파일 보여줌.
    * `ypcat passwd.byname` : NIS 서버의 사용자 관련 정보 출력 `ypcat passswd` 만 입력해도 된다.

#### 1.3 파일 관련 서비스

##### 1.3.1 SAMBA

* 삼바 개요 : SMB (Server Message Block) 프로토콜을 이용하여 운영체제간 자료 공유, 하드웨어 공유

  * 유닉스와 윈도 환경을 동시에 지원하는 **CIFS (Common Internet FIle System)** 로 확장

* 삼바 구성

  * 2개의 데몬 사용

    * `smbd `: 파일과 프린터 공유, 사용자의 권한 부여 및 확인 등 사용자 인증 담당

    * `nmbd`: WINS 를 담당하는 데몬으로 클라이언트를 위해 NetBIOS nameserver 지원, browsing 지원

      ​			컴퓨터 이름과 IP 주소 연결 지원

  * **smb.conf** 환경설정 파일

    * 디렉터리 : `/etc/samba` 

      * 각 항목의 설정은 `[]` 기호를 이용해 Section 으로 구분
      * `#,;` '#' 과 ';' 모두 주석으로 간주되어 무시된다. ( # 은 유닉스 계열, ; 는 윈도우 계열)
      * `name = value 행` : 사용하는 옵션과 해당 값을 설정하는 행

    * 섹션 `[global]` : 삼바 서버 전체적인 환경 설정을 담당하는 섹션

      * 예시 : hosts allow = 192.168.1.  127. 

        -> 192.168.1.0 네트워크 대역에 속한 모든 호스트들과 로컬 호스트(127.0.0.0 네트워크) 접속 

      * `security = user` : 삼바 서버에 접속할 때 인증 모드를 설정하는 항목 

        * 주요 모드 : `share` - 삼바 서버에 사용자 인증 없이 접근할 때 사용

* 관련 명령어

  * `smbclient` : 접속하는 호스트명, 디렉터리명 입력하는 방법
    * `\ backslash` , `/ slash` 사용가능
    * 맞는 예시  (개수에 주의 . 백슬래쉬 는 4개 2개 , 슬래쉬는 2개 1개)
      * `smbclient \\\\192.168.5.13\\joon`
      * `smbclient //192.168.5.13/joon`
  * `smbstatus` : 삼바 서버에 사용하는 명령어 클라이언트와 연결된 상태 출력
  * `testparm` : 삼바의 환경 설정 파일인 smb.conf 설정 여부를 확인
  * `smbpasswd` : 삼바 사용자를 생성 및 삭제, 패스워드 변경, 활성 및 비활성화 등 관련 정보 변경 명령
    * -x : 삼바 사용자 제거 시 사용
  * `pbedit` : 삼바 사용자의 데이터베이스 파일인 SAM database 를 관리해 주는 명령

* 공유 정의 (Sahre Definitions) 설정 예시

  ```bash
  [www] #윈도우 접근 폴더 이름은 www
  comment = Web Directory #설명은 Web Directory
  path = /usr/local/apache/htdocs #공유 디렉토리 경로 
  valid users = phpman webgirl # 접근 가능 사용자
  writable = yes # 두 사용자 모두 생성 및 삭제 권한 부여
  write list = phpman # 파일 생성 시 삭제 권한은 phpman 만 가능
  
  
  ```

##### 1.3.2 NFS 서버 관리

* NFS Network File System : `rpcbind` 데몬 실행 필요, `nfs-utils` 필요
* 서버 접근 제어 : `/etc/exports` 
* 데몬 스크립트 위치 : `/etc/rc.d/init.d`
  * rw 옵션 지정시 디렉터리 퍼미션도 rw 해줘야함.
* /etc/exports 환경 설정 파일
  * **root_squash** : NFS 클라이언트에 접근하는 root 사용자를 무시하고 nobody 로 매핑시킴.
    * 일반 사용자의 권한은 그대로 유지
  * **no_root_squash** : NFS 클라이언트에 접근하는 root 사용자를 무시하지 않고 root 로 인정
  * **all_squash** : root 를 포함하여 모든 사용자의 권한을 nobody로
  * **anonuid** : 접근하는 사용자 권한을 지정하는 uid 로 매핑
* 관련 명령어
  * `rpcinfo` : rpc 관련 정보 출력
  * `exportfs` : NFS 서버에 익스포트 된 디렉터리 정보를 관리, 확인
  * `showmount`  : ***NFS 클라이언트에서 NFS 서버에 익스포트*** 된 정보를 확인

##### 1.3.3 FTP 서버 관리

* FTP (File Transfer Protocol)
* vsftpd 
  * /etc/vsftpd/ftpusers : PAM 관련 설정 파일인 /etc/pam/vsftpd 에 사용되는 파일
    * 기본 설정이 접근 거부될 사용자 목록으로 이용된다.
  * /etc/vsftpd/user_list : vsftpd 에서 이용하는 사용자 목록 파일, 허가 또는 거부 목록 파일로 사용가능
    * 기본 설정이 userlist_deny = YES , 거부 목록 파일로 사용
* vsftpd.conf 분석
  * dirmessage_enable = YES 
    * 접속 시 메시지를 보여줄 것인지 여부 지정 
    * 홈 디렉터리에 .message 라는 파일에 메시지를 적어놓으면 접속 시 나타난다.
    * anonymous 는 레드햇 리눅스에서 /var/ftp 라는 디렉터리로 연결
  * chroot_local_user = YES 
    * 접속한 사용자의 홈 디렉터리를 최상위 디렉터리로 지정. 이 지시자를 사용하면 접속자에게 적용
  * session_support = YES
    * vsftpd 서버에 접속한 내역을 /var/log/wtmp 파일에 기록하여 last 명령으로 확인
  * max_clients = 50
    * 최대 접속자를 지정하는 항목
  * max_per_ip = 3
    * 한 IP 주소 당 허용할 접속 수 지정

#### 1.4 메일 관련 서비스

##### 1.4.1 메일 관련 서비스 이해

* 메일 관련 **프로토콜**

  * SMTP Simple Mail Transfer Protocol : 메일 서버로 메일 보낼 때 사용 *** 포트 25 ***

  * POP3 Post Office Protocol Version 3 : 서버에 도착한 메일을 클라이언트에 직접 내려받음.

    확인 후 서버에서 해당 메일 삭제하는 단점 존재 *** 포트 110 ***

  * IMAP (Internet Mail Access Protocol) : 메일 확인 후에도 서버에 메일 존재 *** 포트 143 ***

* 메일 관련 **프로그램**

  * MTA Mail Transfer Agent : 메일 서버 전달 프로그램
    * sendmail, qmail, postfix, MS Exchange server 등
  * MUA Mail User Agent : 메일 읽고 보낼 때 사용
    * **evolution**
  * MDA Mail Delivery Agent : 메일 대행 가져오기 또는 전달
    * **procmail** : 스팸 메일 필터링 및 메일 정렬

* 리눅스와 메일 관련 프로그램 

  * 서버 : **sendmail,qmail,postfix**
  * POP3 , imap : **dovecot**

##### 1.4.2 메일 서버 설치 및 활용 (Sendmail)

* sendmail : /etc/mail/sendmail.cf 에서 설정, /etc/rc.d/init.d/sendmail 데몬 스크립트 이용 제어
* Fw/etc/mail/local-host-names : Fw 여러 도메인을 사용하는 경우 별도의 파일을 지정
* Dj : 특정 도메인명으로 지정하여 강제적용 시 사용하는 항목
* 센드메일 관련 주요 파일
  * /etc/mail/sendmail.mc : 삭제 및 복원 시 m4 매크로 프로세서 사용 새롭게 생성.
    * sendmail-cf 패키지 활용 , sendmail.cf 파일 복원
    * 예시 : `m4 /etc/mail/sendmail.mc > /etc/mail/sendmail.cf`
  * /etc/mail/access : 
    * **DISCARD** 옵션 : 메일을 거부 메시지 없이 무조건 거절
    * **REJECT** 옵션 : 메일을 거부 후 거부 메시지를 보냄.
  * /etc/aliases : 특정 계정으로 들어오는 메일을 다른 계정으로 전송되도록 설정
    * 변경 후 `newaliases`, `sendmail -bi` 입력해야함.
  * /etc/mail/virtusertable 
    * 하나의 메일 서버에 여러 도메인을 사용하는 환경에서 동일한 메일 계정 요구 시.
      * (windows.com 과 linux.com 에서 ceo 라는 동일 이메일 계정 요구)
    * /etc/mail/virtusertable.db 참조
    * `makemap hash` + `<` 명령 사용
  * ~/.forward
    * 각 사용자의 개인이 자신에게 들어오는 메일을 다른 메일 주소로 포워딩
* sendmail 명령어
  * `mailq`: 보내는 메일이 대기하는 Queue 를 출력
    * /var/spool/mqueu 의 상태를 출력
  * `sendmail -bp` : mailq 와 같은 명령, Queue 출력

#### 1.5 DNS 관리

##### 1.5.1 DNS Domain Name System 이해

* Cahching Name Server : 관리하는 도메인 없이 리졸빙 만을 제공, 직접 조회하지 않고 바로 응답해 주는 역할
* DNS 서버 프로그램 BIND
  * 관련 디렉터리 
    * /etc/named.conf : (name daemon) : DNS 서버의 전반적인 환경 설정을 담당 파일로 서버에서 사용하는 zone 파일을 지정

##### 1.5.2 DNS 관리 및 활용

* /etc/named.conf 

  * options

    * forward (only|first) : forwarders 옵션과 함께 사용, only 나 first 두 값 중 하나를 가짐
      * only : 자신에게 들어온 도메인 질의를 지정한 다른 서버로 넘김. 응답이 없을 경우 자신도 응답하지 않는다.
      * first : 타 서버 응답이 없으면 자신이 응답해준다.
    * forwarders {네임서버주소1 , 네임서버주소2 ...}
      * 도메인에 대한 질의를 다른 서버로 넘길 때 사용하는 옵션
    * allow-query {192.168.0/24}
      * 네임 서버에 질의할 수 있는 호스트를지정 -> 192.168.0.0 네트워크 주소 대역 호스트 질의 가능

  * acl 구문(Access Control List)

    * 예시

      ```bash
      acl "member" { 210.96.52.110; 203.... 211...}
      options {
      	directory "/var/named"
      	allow-transfer {203.247.50...}
      	dump-file "/var/named/data/cache_dump.db"
      	allow-query {203..... ; member;}
      
      }
      zone "." In{
      	type hint; # ca 파일 타입은 힌트
      	file "named.ca";
      	
      zone "linux.or.kr" In { #.zone 마스터
      	type master;
      	file "linux.zone"
      }
      zone "12.168.192....arpa" In{
      	type master;
      	file "linux.rev" #.rev 역 존, reverse zone 타입 마스터
      
      }
      
      };
      ```

* ZONE 파일

  * 도메인명 ,호스트명
    * @ : 현재 도메인
    * `*` : 모든 호스트명  **반드시 맨 뒤에 `.` 붙여야한다.
    * t
  * type 
    * A : IPV4 주소
    * AAAA : IPV6 주소
    * NS : 도메인 네임 서버
    * MX : 메일 서버 지정 시 사용, MX 뒤에 0 또는 양의 정수 값으로 우선순위
    * CNAME : Canonical Name 레코드 , 별칭 지정 시 사용 Alias
    * PTR : 리버스 존에서만 사용하는 레코드, IP 주소를 도메인으로 변환하기 위해 지정

* DNS 관련 유틸리티

  * named-checkconf : `/etc/named.conf` 파일의 문법적 오류를 찾아주는 명령
  * named-checkzone : **zone 파일**의 문법적 오류를 찾아주는 명령



#### 1.6 가상화 관리

##### 1.6.1 가상화 서비스 개요

* 가상화 기능

  * 에뮬레이션 Emulation  
    * 물리적 차원 자체에는 원래부터 존재하지 않았지만, 가상 자원에는 어떤 기능들이나 특성들이 마치 처음부터 존재했던 것처럼 가질 수 있음.

* 가상화 효과

  * 향상된 프로비저닝 Provisioning
    * 요구사항에 맞게 할당, 배치, 배포할 수 있도록 만들어 놓는 것.
    * 더 세밀한 조각 단위로 자원 할당하여 효율성 증대를 극대화

* 서버 가상화 특징

  * 장점 
    * 효율적인 서버 자원 이용
    * 데이터 및 서비스 가용성 증가
    * 서버가 차지하고 있는 공간 절약
    * 전원 절약
    * 기타 등등
  * 단점
    * 시스템 복잡
    * 장애 발생 시 문제 해결 복잡
    * 호환, 확장 안좋음
    * 라이선스

* 서버 가상화 분류

  * 하드웨어 레벨 
    * Hypervisor 전 가상화 (Bare-Metal , Full Virtualization)
    * 반가상화
    * 호스트 기반 가상화
  * 운영체제 OS 레벨 가상화
    * 컨테이너방식
      * Docker,
    * 하드웨어 에뮬레이터 방식 : 소프트웨어적으로 하드웨어를 가상 에뮬레이팅하는 방식

* 리눅스 기반 가상화 기술의 분류 및 특징

  * XEN 젠 : 다양한 CPU 지원,하이퍼바이저 기반, CPU 반가상화 및 게스트 시스템 실행 가능한 전가상화도 가능, QEMU 기반 동작

  * KVM : 하이퍼바이저 x86 시스템 기반 cpu 전가상화 방식 QEMU CPU 에뮬레이터 사용

    CPU 반가상화 기술 지원하지 않으나, 이더넷 카드 Disk I/O , 그래픽 인터페이스 반가상화는 지원

    상용화 제품 : RHEV

  * Docker 도커 : 서버 운영에 필요한 프로그램과 라이브러리만 이미지로 만들어 프로세스처럼 동작시키는 

    경량화된 가상화 방식, (컨테이너)

##### 1.6.2 가상화 서비스 구축

* KVM 서비스 구축 
  * CPU 지원 확인 
    * `/proc/cpuinfo` 파일 확인
    * `egrep "svm|vmx" /proc/cpuinfo` 명령어 사용
  * 가상장치 관리자 ?
    * `virt-manager`
  * `libvirtd ` 데몬
    * 가상화 API 역할을 수행하는 데몬, 기본으로 실행되는 경우가 많으니 restart해도 된다.
* 시스템 리소스 관리
  * 디스크 사용량 확인 
    * `var/lib/libvirt/images/가상머신이름.img` 로 생성. `du-sh` 명령어로 사용량 확인 가능

#### 1.7 기타 서비스

##### 1.7.1 슈퍼 데몬 관리

* inetd : internet daemon 
  * standalone 방식에 비해 처리 속도는 느리지만, 메모리가 부족하고 다양한 서비스 제공에 적합하다.
  * /etc/inetd.conf 에서 설정
  * 2.4 부터는 xinetd 데몬이 역할 수행 
* TCP Wrapper
  * tcpd 라는 데몬이 슈퍼데몬인 inetd 에 의하여 수행되는 서비스들의 접근을 제어하도록 하는 프로그램
  * `/etc/hosts.allow` 와 `/etc/hosts.deny`
    * 새로운 줄바꿈은 무시된다. 백슬래시 \ 를 사용해야 한다.
    * 빈 줄 혹은 # 으로 시작되는 줄은 주석으로 간주.
    * 예시
      * in.telnetd : 192.168.5.13
      * ALL EXCEPT vsftpd : .ihd.or.kr EXCEPT bad.ihd.or.kr
* xinetd
  * inetd 대체 등장. ( ID 제한 기능 없다)
  * IP 주소 당 접속 수 제한
  * 시단대별 서비스 제한
  * DoS 공격 대비 설정 기능 
  * 구성요소
    * /etc/xinetd.conf : 환경설정 
    * /etc/xinetd.d  : 디렉터리
    * /etc/rc.d/init.d/xinetd  : 데몬 스크립트 (stop start restart 등)
  * log_type :  어떠한 형태로 로그를 저장할 것인지
    * SYSLOG 와 FILE 두 가지 설정이 가능. syslog 에 위임 또는 직접 별도 파일 지정
      * log_type 	= FILE /var/log/xinetd.log  : 이런식으로 사용
  * cps : 초당 요청받은 수에 대한 제한
    * cps = 50 10 : 초당 요청 수가 50개 이상이면 10초동안 접속 연결을 중단
  * instances : 동시에 서비스 할 수 있는 서버의 최대 개수를 지정하는 항목
    * instances = 50 : xinetd 에서 생성하는 관련 데몬의 최대 개수를 50으로 지정
  * only_form : 서비스를 이용할 수 있는 원격 호스트를 설정,
    * only_form = 192.168... , 192.168.12.0/24 등
  * no_access : 이용할 수 없는 원격 호스트 주소 지정
    * only_form 과 중복 지정되면 차단해버림.
  * disabled : 특정 서비스 사용을 막을 때 사용
    * enabled 항목과 같이 설정되면 무시되버림 ( 실행안됨 )
* 서비스 파일에서 사용되는 속성과 속성값
  * access_times = 01:00-07:00 
    * 지정된 시간에만 서비스 이용
  * redirect = 192.168.12.22.23
    * 포트 포워딩 

##### 1.7.2 PROXY 관리

* 프록시 개요
  * 네트워크 속도가 느린 환경에서 보다 빠른 인터넷을 이용하기 위해 사용
  * 자주 방문하는 사이트의 정보를 저장하는 일종의 캐시 서버. 
  * 서버에 저장된 데이터를 전달하여 처리 속도를 높인다.
* 프록시 서버의 구성과 이용
  * squid 
    * /etc/squid/squid.conf
    * /etc/rc.d/init.d/squid
  * squid.conf 설정
    * http_port 3128  : 기본 포트값 3128, http_port 로 설정함
    * cache_dir ufs /var/spool/squid 100 16 25
      * ufs 는 squid 의 저장 포맷, /var/spool/squid 가 관련 디렉터리
      * 100은 정보크기, 단위 MB 16은 캐시가 저장되는 첫번째 하위 디렉터리 개수, 256은 두번째

##### 1.7.3 DHCP 관리

* DHCP Dynamic Host Configuration Protocol 
  * 하드디스크가 없는 원격 호스트에서 이더넷 카드로 부팅할 때 사용
* DHCP 서버 설치
  * /etc/dhcpd.conf : rpm-ql dhcp 명령으로 dhcpd.conf.sample 을 찾아서 복사하거나 이름을 바꾸어 생성
  * /etc/rc.d/init.d/dhcpd : dhcpd 데몬을 제어하는 데몬 스크립트
* /etc/hdcpd.conf 주요 설정
  * range : 클라이언트에게 할당할 IP 주소를 시작 주소와 마지막 주소 기입
  * option domain-name : 도메인명 지정시 사용
  * option domain-name-servers : **네임 서버를 지정** 할때 사용, 도메인 또는 IP 주소 기입
  * option routers : **게이트웨이 주소** 지정 시 사용
  * fixed-address : 192.168.1.22 : 맥 주소에 IP 주소를 할당

##### 1.7.4 VNC 관리

* VNC Virtual Network Computing 
  * VNC는 RFB 프로토콜을 이용하여 원격의 다른 컴퓨터에서 그래픽 환경 기반으로 데스크톱을 공유할 수 있는 환경
* /etc/sysconfig/vncservers 를 편집
  * -gemotry ? : 해상도 설정
* VNC 서버 패스워드 설정
  * vncpasswd
* VNC 서버 시작
  * service vncserver start
* VNC 클라이언트 설치
  * 포트 5900 기본
  * 상태에 따라 5901
  * 관리자 사용시 5902

##### 1.7.5 NTP

* NTP Network TIme Protocol 
  * 컴퓨터간 시간 동기화 프로토콜, UTC를 기준으로하며 1/1000초 까지 동기화 가능
  * stratum 이라는 계층으로 구성
* ntpq  -p : 연결된서버상태 출력
* ntpdate : 시간 동기화시 사용



### Chapter 2 네트워크 보안

#### 2.1 네트워크 침해 유형 및 특징

##### 2.1.1 네트워크 침해 유형 및 특징

* Ping of Death : ICMP 패킷을 정상적인 크기보다 크게 만들어 보내는 공격 (범위 밖)

* UDP Flooding : UDP 패킷 대량 발생

* TCP SYN Flooding : 3-way-handshaking 오류 발생시키기

* Teardrop ATtack : 패킷 분할 재조합 혼란 야기 Boink bonk

* Land Attack : 자신의 IP 주소 및 포트를 대상 서버의 IP 주소 및 포트와 동일하게 하여 서버 공격

* Smurf Attack : ICMP Request 패킷을 브로드캐스트를 통해 다수의 시스템에 전송 

  공격자가 아닌 공격 대상의 서버로 전송

* 시스템 자원 고갈 공격 : 디스크, 메모리 , 프로세스에 대한 자원 고갈 

  * 디스크 : fd - creat ~~ 코드
  * 메모리 : m = malloc(1000) ~~
  * 프로세스 : fork() return 0

* DDOS 공격

  * Trinoo : UDP Flooding
  * TFN : UDP Flooding + TCP SYN Flooding + ICMP 
  * TFN 2K : UDP ,TCP, ICMP 복합사용 위에꺼도 다 가능 + Smurfing
  * Stacheldraht : 독일어로 철조망, 암호화 기능 추가, 패스워드 입력 요구
    * UDP Flooding, TCP SYN Flooding , ICMP Flooding, Smurf

#### 2.2 대비 및 대처 방안

##### 2.2.1 대비 및 대처 방안

* DOS 및 DDOS 공격 대응책
  * Firewall
  * IDS, IPS
  * 로드 밸런싱 트래픽 분산처리, 네트워크 성능 강화
  * 대역폭 제한
* 방화벽 종류 
  * 스크린 서브넷 게이트웨이
    * 외부 네트워크와 내부 네트워크 사이에 완충지대를 두는 방식
    * 완충지대에 보통 DMZ 위치, 방화벽도 이 위치에 설치
    * 융통성이 뛰어나지만 비용이 많이듬
  * 스크린 라우터 
    * 방화벽 역할을 수행하는 라우터, 외부 네트워크 내부 네트워크 경계에 위치 , 필터링 기능 제공
    * 낮은 수준의 접근 제어
* 리눅스 기반 IDS 및 IPS 시스템
  * Snort : 공개형 IDS 및 IPS 프로그램, 침입 탐지
  * Suricata : Snort 와 유사함, 호환 됨, GPU 하드웨어 가속 지원, LUA 스크립트 언어 시그니처 작성 가능
* Iptables 를 이용한 리눅스 방화벽 구축
  * 4계층 테이블을 만들어 사용, ipchains 와 거의 유사한 사슬에 정책을 설정하여 사용
  * **filter,mangle,raw,nat** 4개의 테이블로 구성
  * **INPUT** 사슬 : 접근 통제역할
* Iptables 사용하기
  * iptables [-t table] action chain match [-j target]
    * 매치는 -d, -p 와 같은 소문자 옵션을 사용해서 설정, 타겟은 -j(--jump) 옵션을 사용하여 설정
  * 주요 액션
    * -X 비어있는 사슬 제거
    * -F 사슬로부터 규칙 제거, 모든 사슬에 설정된 정책 모두 제거
    * -D 사슬 규칙 제거 하나만(?)
  * 주요 타겟 (-j)
    * DROP : 패킷 거부, 더이상 어떤 처리도 수행되지 않고 버림
    * REJECT : 패킷을 버리고 동시에 적당한 응답 패킷 전달
* iptables 설정 규칙 저장
  * iptables-save > 파일명 
    * 현재 설정된 정책을 파일명으로 저장한다.
  * iptables-restore < firewall.sh
    * firewall.sh 에 저장된 정책을 불러들여서 반영한다.
* iptables 스크립트 사용
  * /etc/rc.d/init.d/iptables 라는 스크립트 제공
  * /etc/sysconfig/iptables 라는 파일에 관련 정책이 저장된다.
  * `service iptables save`
* iptables 를 이용한 NAT 구현
  * 분류 및 설정
  * SNAT ( Source NAT) : 공인 IP 주소 하나로 다수의 컴퓨터가 인터넷 접속
    * 라우팅 경로가 결정된 이후에 설정 : POSTROUTING
  * DNAT (Destination NAT) : 하나의 공인 IP 주소로 다수의 서버를 운영
    * 라우팅 이전 단계에서 적용 : PREROUTING 
* iptables 공격대비
  * ICMP Flooding 대비
  * TCP Flooding 대비
    * fail2ban
    * sshguard
    * DenyHosts

