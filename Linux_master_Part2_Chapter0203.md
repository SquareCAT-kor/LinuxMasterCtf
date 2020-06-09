# 리눅스 마스터 1급 

## Part2 리눅스 시스템 관리

### chapter 2 장치 관리



#### 1. 모듈 (Module)

* 개요 : 모듈의 사전적인 의미는 프로그램이나 하드웨어 기능단위, 교환 가능한 구성부분 등
  * 리눅스에서의 커널 모듈은 필요할 때 커널 이미지에 합류하고, 필요하지 않을 때에는 커널에서 빠져 나와 모듈 형태로 존재하여 시스템의 메모리를 절약한다.
  * 즉 커널 외부에서 내부로 로드 되었다가 언로드 되었다가 하면서, 메모리 절약에 도움을 준다.
* 모듈 관리
  * 리눅스 커널 모듈은 C 컴파일러로 만들어진 오브젝트 파일 `*.ko` 형태로 생성
  * 각 시스템에서 가능한 모듈은 ***'lib/modules/커널버전/kernel'***디렉터리 안에 생성되어 있다.
* 모듈 관련 명령어
  * lsmod : 모듈 정보 출력 
    * `lsmod`
  * insmod  : 커널에 모듈을 적재(load) 하는 명령. 모듈을 자동으로 검색하고 삽입
    * `insmod module_name`
  * rmmod : 커널에서 모듈을 제거하는 명령, ***다른 모듈에 의해 사용 중인 모듈은 제거할 수 없다.***
    * `rmmod module_name`
  * ***modprobe*** : 리눅스 커널에 모듈을 적재하거나, 제거하는 명령. insmod나 rmmod 명령은 다른 모듈에 의존되어 있는 경우 사용 불가하지만, modprobe 는 단일 모듈, 의존성이 있는 여러 모듈 , 특정 디렉터리의 모든 모듈들을 적재할 수 있다. 
    * `modprobe [option] 모듈 [기호=값]`
    * 옵션 종류
      * -l : 사용 가능한 모듈 정보 출력
      * -r : 모듈을 제거할 때 사용하는 옵션, 의존성 있는 모듈들을 찾아서, 사용되지 않는다면 자동 제거
      * -c : 모듈 관련 환경 설정 파일의 내용을 전부 출력한다.
* 모듈 관련 설정 파일 
  * 환경설정 파일 경로 : ***/etc/modprobe.conf 또는 /etc/modprobe.d 안의 '.conf'로 끝나는 모든 파일***
* 모듈 의존성 파일 : **modules. dep**  
  * 경로 : ***'lib/modules/커널버전/modules.dep*** 

#### 2. 커널 ( kernel )

* 개요 : 시스템 자원을 소유하고 관리하는 역할 담당. 하드웨어, 메모리, 프로세스 스케줄링을 담당하고 프로그램이 하드웨어 자원을 간접적으로 접근할 수 있도록 해준다. 

  * `uname -r` 명령으로 커널 버전 확인 가능. 
  * http://www.kernel.org 에서 배포되고 , 버전은 4.12.2 형태로 배포. 

* 커널 컴파일 : 커널 소스를 다운로드하여 사용하는 시스템에 최적화된 커널을 만드는 과정

  * C 컴파일러인 gcc, 어셈블러, 링커 , make 유틸리티 등의 개발도구 설치

  * ***/usr/src/kernels*** 디렉터리에 다운로드.

  * 커널 컴파일 작업 순서 

    * ***순서 및 내용 매우중요!***

      ```bash
      tar Jxvf linux-4.12.2.xz # 압축 해제
      cd linux-4.12.2
      make mrproper # 설정 초기화
      make menuconfig # 커널 컴파일 옵션 설정 도구 실행
      make bzImage # 커널 이미지 생성 # bzip2로 압축해서 생성. 
      make modules # 커널 모듈 생성을 위한 컴파일 # 커널 옵션 설정 시 m 으로 설정한 항목 모듈화
      make modules_install # 모듈 설치 작업 # /lib/modules/커널버전 디렉터리에 안에 복사
      make install # 커널 모듈 파일 복사, grub.conf 파일 자동 수정 작업
      reboot # 시스템 재가동 후 새로운 커널로 부팅
      uname -r # 커널 버전 확인
      
      ```

    * 커널 컴파일 주요 도구

      ```bash
      make config # 텍스트 기반의 설정 도구로, 터미널 환경에서 y,m,n 으로 설정
      make menuconfig # 텍스트 기반의 컬러 메뉴를 제공, 커서를 이용해서 이동이 가능. 가장 보편적으로 사용 
      make nconfig # 텍스트 기반의 컬러 메뉴, 커서와 F1~F9까지의 기능키 제공 도구
      make xconfig # X 윈도 환경의 Qt 기반의 설정 도구
      make gconfig # X 윈도 환경의 Gtk+ 기반의 설정 도구
      ```

#### 3. 주변장치 관리

	##### 디스크 확장

```bash
fdisk -l #dev/sdb ( dev/sdx 로 인식될것임)
fdisk /dev/sdb # fdisk 장치명 명령으로 원하는 용량만큼 할당한다.
# 과정을 마치면 /dev/sdb1 으로 생성
reboot # 시스템 재부팅
cat /proc/partitions # 파티션 활성화 여부 확인 
mkfs.ext4 /dev/sdb1 # ext4 파일 시스템으로 생성
mkdir /backup # 마운트 포인트에 해당하는 /backup 디렉터리 생성
mount -t ext4 /dev/sdb1 /backup # 마운트 작업. ext4 시스템을 backup 마운트 포인트로 
mount # 마운트
df -h  # 용량 확인 
vi /etc/fstab # vi 편집기를 이용하여 /etc/fstab 에 등록하여 부팅 시 자동 마운트 되도록 한다. 
```



##### 프린터

* 프린팅 시스템 개요 
  * LPRng : BSD 계열 유닉스에서  사용. 
    * lpr,lpq,lprm 프린터 명령 & System V 계열 lp,lpstat,cancel 도 가능
    * 설정한 정보는 /etc/printcap 파일에 저장된다. 
  * CUPS ( Common Unix Printing System ) 는 애플이 개발한 오픈 소스 프린팅 시스템으로 유닉스 계열 운영체제의 시스템을 프린터 서버로 사용 가능하도록 해준다. 
    * HTTP 기반의 IPP을 사용하고, SMB 프로토콜로 부분적으로 지원
      * lpadmin 명령을 이용하여 웹 상에서 제어 가능
      * http://localhost:631 포트 에서 네트워크 프린터처럼 설정 가능 ( CUPS )
    * 관련 파일
      * `/etc/cups/cupsd.conf` : CUPS 프린터 데몬의 환경 설정 파일, 기본 문법이 아파치의 httpd.conf 와 유사.
      * `/etc/cups/printers.conf` : 프린터 큐 관련 환경 설정 파일로 lpadmin 명령을 이용하거나 웹을 통해 제어
      * `/etc/cups/classes.conf` : CUPS 프린터 데몬의 클래스(class) 설정 파일
      * `cupsd` : CUPS의 프린터 데몬
    * 프린터의 설정 : X-Window 상에서 손쉽게 설정 
      * printtool, printconf 사용 redhat-config-printer 를 거쳐 system-config-printer 를 사용한다. 
* 네트워크 프린터 관련 
  * AppSocket : Hp 의 jetDirect 처럼 프린터가 직접 네트워크에 연결 시 이용
  * Internet Printing Protocol (IPP) : IPP 프로토콜 기반 프린터 설정 시 사용
  * Internet Printing Protocol (Http) : https 프로토콜 기반 프린터 설정 시 사용
  * LPD/LPR Host or Printer : LPRng 와 같은 LPD 프로토콜 기반의 프린터 설정 시 사용
  * ***Windows Printer via SAMBA*** : 윈도우 시스템에서 연결된 프린터 설정 시 사용, 삼바 기반의 SMB 프로토콜을 사용

##### 프린터 관련 각종 명령어 

* `lpr`

```bash
lpr # 프린터 작업을 요청 
lpr -#2  # 매수 지정
lpr -m # mail . E-mail 로 관련 정보 전송
lpr -P printername # 사용할 프린터 지정
lpr -T # Title 타이틀 페이지에 들어갈 타이틀명 설정
lpr -r # 출력한 뒤에 지정한 파일 삭제
lpr -l # 필터링 없이 직접 보낸다. 
lpr -# 2 -P lp test.txt 
>>> test.txt 라는 문서를 lp 라는 이름을 가진 프린터를 이용해 2장 출력한다.
# 파이프 사용하기
cat test.txt | lpr 
>>> cat 명령 + lpr 명령어를 이용하여 test.txt 를 출력한다.
직렬 포트로 연결 시
cat test.txt > /dev/lp0 # 이런식으로도 가능
pr -l80 test.txt | lpr 
lpr -r test.txt # text.txt를 출력한 후에 파일 삭제
```

* `lpq` : 프린터 큐에 있는 작업의 목록을 출력

```bash
lpq -a # 설정되어 있는 모든 프린터의 작업정보 목록 출력
lpq -l # 출력 결과를 자세하게 (long format) 출력
lpq -P printername # 특정 프린터를 지정할 때 사용
```

* `lprm` : 프린터 큐에 대기중인 작업을 삭제, 취소할 프린트 작업의 번호를 입력

```bash
lpq - # 프린터 큐에 있는 모든 작업 취소
lpq -U username # 지정한 사용자의 인쇄 작업 취소
lpq -P printername #특정 프린터를 지정할 때 사용
lpq -h server[:port] # 지정한 서버의 인쇄 작업을 취소
```

* `lpc` : 대화형 프린터 제어

```bash
lpc
exit help quit status ? 
disable enable down up
# down : 지정한 프린터 사용불가
# up : 모든 환경 활성화, 관련 데몬을 새롭게 구동
# help ? : 사용 가능한 명령 출력
# quit, exit : 종료
```

* `lp` = SYSTEM V 계열 명령 == `lpr` of BSD

```bash
lp -d #: 다른 프린터 지정
lp -n #: 인쇄 매수 지정
lp /etc/passwd #: /etc/passwd 파일 내용 출력
lp -n 2 /etc/passwd #: 2장출력
```

* `lpstat` : 프린터 큐의 상태를 출력

```bash
lpstat -p #: 인쇄 가능 여부출력
lpstat -t #: 프린터 상태 정보 출력
lpstat -a #: 받아들이는 요청들의 상태 출력
```

* `cancel` : 프린트 작업 취소명령 , lpstat 을 이용해 Request-ID 를 확인해야한다.

`cancel -a` : 모든 작업 취소

ex: `cancel printer-7`: 아이디가 printer-7 인 작업 취소

##### 사운드 카드

* 고급 리눅스 사운드 아키텍처 : ALSA ( Advanced Linux Sound Architecture)
  * 사운드 카드용 장치 드라이버를 제공하기 위한 리눅스 커널의 요소
  * **GPL , LGPL 라이센스**, **Jaroslav Kysela**
* 오픈 사운드 시스템 OSS ( Open Sound System )
  * OSS는 초기에 free 였기에, 리눅스 커널에서 구현되었던 OSS/free 를 포기하고 ALSA 로 대체.
* ALSA 명령어
  * `alsactl` ***=ALSA 사운드카드를 제어하는 명령어***
    * 옵션 
      * -d : 디버그 모드를 사용한다. (--debug) 
      * -f : 환경 설정 파일을 선택한다. 기본 파일은 /etc/asound.state 
    * 명령
      * store : 사운드카드에 대한 정보를 환경 설정 파일에 저장한다.
      * restore : 환경 설정 파일로부터 선택된 사운드카드 정보를 다시 읽어 들인다.
      * init : 사운드 장치를 초기화한다. 
  * alsamixer : 커서(ncurses) 라이브러리 기반의 ALSA 사운드카드 오디오 믹서 프로그램
  * cdparanoia : 오디오 CD에서 음악 파일을 추출할 때 사용하는 명령
    * cdparanoia [option] 
      * -w : wav 파일로 추출
      * -a : Apple AIFF-C 포맷으로 추출
      * -B : 모든 트랙의 음악을 Cdda2wav 스타일로 추출한다. 'track#.' 형태로 파일 이름이 생성된다.
        * 디스크 전체에서 각 트랙별로 추출해서 파일로 생성한다.
      * `cdparanoia "1[:30.12]-1[1:10]"` : 트랙 1번의 시간 0:30.12 부터 1:00.00 까지 추출
      * `cdparanoia -- "-3"` : 트랙 3번부터 추출한다. 

##### 스캐너

* SANE (Scanner Access Now Easy)

  * 평판 스캐너, 핸드 스캐너, 비디오 캠 등 이미지 관련 하드웨어를 사용할 수 있도록 해주는 API
  * sane-backends 와 sane-frontends 등 2개의 패키지로 배포

* XSANE (X based interface for the SANE)

  * SANE 스캐너 인터페이스를 이용하여 X-Window 기반으로 만든 프로그램
    * GTK+ 라이브러리로 만들어짐. 이미지 수정작업 가능
    * `xsane` 명령어로 실행

* 관련 명령어

  * `sane-find-scanner` : USB 및 SCSI 스캐너와 관련 장치 파일을 찾아주는 명령어.

    * 보통 SCSI 스캐너는 /dev/sg0, /dev/scanner 로 인식
    * USB 스캐너는 /dev/usb/scanner, /dev/usbscanner 등으로 사용
    * 옵션
      * -q : 스캐너 장치만 출력
      * -v : 자세한 정보 출력
      * -p : 병렬 포트에 연결된 스캐너만 찾는다.

  * ```bash
    sane-find-scanner-v
    sane-find-scanner /dev/scanner
    sane-find-scanner-p
    ```

  * `scanimage` : 이미지를 스캔하는 명령

    ```bash
    scanimage -h # help , 도움말 옵션
    scanimage -d # SANE 장치 파일명 적는 옵션 --device-name
    scanimage --format # 이미지 형식을 지정하는 옵션 pnm 과 tiff 지정 가능, default pnm
    scanimage -L # 사용 가능한 스캐너 장치 목록 출력 --list-devices
    
    scanimage -L # 스캐너 목록을 출력
    scanimage > image.pnm # 이미지를 기본 설정된 값으로 스캔하여 image.pnm 파일로 저장
    scanimage -x 100 -y 100 --format=tiff > image.tiff
    # 100*100 mm 크기로 스캔하고, 이미지 파일 형식으로 tiff로 하여 image.tiff로 저장한다.
    ```

  * `scanadf` : 자동 문서 공급 장치 (ADF: Automatic Document Feeder) 가 장착된 스캐너에서 여러 개의 사진을 스캔할 때 사용하는 명령어

    ```bash
    scanadf -h #: 도움말 옵션으로 사용 가능한 옵션 목록을 출력 
    scanadf -d #: SANE의 장치 파일명을 적는 옵션 --device-name
    scanadf -L #: 사용 가능한 스캐너 장치 목록을 출력 --list-devices
    ```

  * `xcam` : GUI 기반으로 평판 스캐너가 카메라로부터 이미지를 스캔해 주는 명령

    ```bash
    xcam #: 실행 명령.
    
    lspci #: 설치된 PCI 관련 장치의 목록을 확인 
    ```



### 4. 시스템 보안 및 관리

#### 시스템 분석

##### 시스템 로그 분석 및 관리

* 시스템 로그의 개요 : 시스템에서 일어나는 모든 사건이나 이벤트 등은 각 서비스별로 기록되는데, 이러한 기록들을 로그(log) 라 부른다. 

* syslog 패키지

  * syslogd : /etc/syslog.conf 설정 파일을 기반으로 서비스별 로그 파일을 /var/log 디렉터리에 생성
  * 최근 rsyslog 로 대체되었음.(rocket-fast system for log processing)
    * /var/log 디렉터리에 생성. 

* rsyslog 

  * rsyslogd 데몬이 동작하면서 로그를 기록, 데몬의 동작은 `/etc/rc.d/init.d/rsyslog` 스크립트 이용

    `/etc/rc.d/init.d/rsyslog` : rsyslog 데몬을 동작 시키는 스크립트로 start,stop,restart 등의 인자값을 사용

    `/etc/rsyslog.conf`: rsyslogd 데몬의 환경 설정 파일

    * facility.priority		action :
      `facility` 는 일종의 서비스, 메시지를 발생시키는 프로그램의 유형
      `priority` 는 위험의 정도(설정한 수준보다 높아야 전송)
      `action` 은 메시지를 보낼 목적지나 행동들에 관한 설정(보통 파일명)

      * facility 종류 

        * cron : `cron`,`at` 등의 스케줄링 프로그램이 발생한 메시지 

        * auth, security : login과 같이 인증프로그램 유형이 발생한 메시지

        * authpriv : ssh 와 같이 인증이 필요한 프로그램에서 발생한 메시지, 사용자 추가 시에도 발생

          * `authpriv.*			root,posein` : (ssh)인증 관련 로그를 root 및 posein 사용자의 터미널로 전송한다.
          * `authpriv.*			/dev/tty2` : 인증 관련 로그를 /dev/tty2 로 전송한다.  

        * daemon : telnet,ftp 등 여러 데몬에서 발생한 메시지

        * kern : 커널에서 발생한 메시지

        * lpr : 프린트 유형의 프로그램에서 발생한 메시지

        * mail : mail 시스템에서 발생한 메시지

          * `mail.*;mail.!=info		/var/log/maillog` : mail 관련한 모든 정보는 

            var/log/maillog에 기록하는데, info 수준의 로그는 제외한다.

        * mark : syslogd에 의해 만들어지는 날짜 유형

        * news : 유즈넷 뉴스 프로그램 유형에서 발생한 메시지

        * syslog : syslog 유형의 프로그램이 발생시킨 메시지

        * user : 사용자 프로세스

        * uucp : uucp(UNIX to UNIX Copy Protocol) 시스템에서 발생한 메시지

          * `uucp,news.crit			/var/log/news` : uucp 및 news 에서 발생하는 crit 수준 이상의 메시지는 /var/log/news 에 기록한다.

        * local0~local7 : 여분으로 남겨둔 유형

        * `*`: 모든 facility 를 의미

      * priority 종류

        * none :  지정한 facility 를 제외. 보통 앞에 다른 facility 에 대한 설정을 하고 `;` 뒤에 특정한 facility 를 제외할 때 사용

          * `*.=crit;kern.none   	/var/log/critical`

            `>>>` 모든 facility가 발생하는 메시지 중에 crit 수준의 메시지만 /var/log/critical 파일에 기록하는데, kernel 이 발생시키는 메시지는 제외한다.

        * debug : 프로그램을 디버깅할 때 발생하는 메시지

        * info : 통계, 기본 정보 메시지

        * notice : 특별한 주의가 필요하나 에러는 아닌 메시지

        * warning, warn : 주의가 필요한 경고 메시지

        * error ,err : 에러가 발생하는 경우의 메시지

        * crit : 크게 급하지는 않지만 시스템에 문제가 생기는 단계의 메시지

        * alert : 즉각적인 조정을 해야 하는 상황

        * emerg, panic : 모든 사용자들에게 전달해야 할 위험한 상황

          * `*.emerg			* `	   : 모든 emerg 수준 이상의 문제가 발생하면 모든 사용자에게 메시지를 전달한다.

      * action의 종류

        * file : 지정한 파일에 로그를 기록
        * @host : 지정한 호스트로 메시지를 전달
        * user : 지정한 사용자가 로그인한 경우 해당 사용자의 터미널로 전달
        * `*` : 현재 로그인되어 있는 모든 사용자의 화면으로 전달
        * 콘솔 또는 터미널 : 지정한 터미널로 메시지를 전달

    `/etc/sysconfig/rsyslog` : rsyslogd 데몬이 실행과 관련된 옵션이 설정되는 파일

    `/sbin/rsyslogd`: 실제 rsyslogd 데몬 실행 명령

* 로그 파일 관리 : logrotate

  * 개요 :  log 파일이 계속 쌓이면 용량이 커지기 때문에 이를 분할해 주는 프로그램.
  * 자동 로테이션 기능, 압축 기능, 제거 기능 지원
  * 하루, 일주일, 한달, 연 단위로 로테이션 가능 
  * `/etc/logrotate.conf` 기본적인 로그 설정
  * `/etc/logrotate.d` 디렉토리 내에 응용 프로그램 위치, 로그파일 관리
  * `/etc/cron.daily` 에 등록되어서 cron 에 의해 스케줄링 되어 실행되고 있다.

* logrotate 사용법 

  * `logrotate [option] config_file`
    * 옵션 `-f` : 강제로 환경 설정 파일을 읽어 들여서 실행한다. --force
    * `logrotate -f /etc/logrotate.conf`

* /etc/logrotate.conf  주요 설정

  * 주요 항목 

    * weekly : 로그 파일을 일주일마다 rotate 한다. 가장 맨 위에 등록되어 있는 경우 특별히 명시하지 않은 로그 파일들은 이 파일에 적용을 받는다. 기간과 관련된 옵션은 
       `daily, weekly, monthly, yearly` 를 사용

  * `rotate 4` : 최대 4번까지 rotate를 설정

    * 기본 파일로 logfile,logfile.1,logfile.2,logfile.3,logfile.4형태로 생성

  * `create` : 로테이트를 한 후 비어 있는 로그 파일을 생성하도록 설정

    * 허가번호 / 소유자 / 그룹 순으로 서술.

  * `dateext` : 로테이션으로 생성되는 로그 파일에 해당 날짜를 덧붙여서 생성하는 항목. 
    예시 : `mailog` 라는 이름이 있다면 mailog-20150513 형태

  * `compress` : 로테이트 한 후에 생성된 로그 파일에 대해 압축

  * `include /etc/logrotate.d` : /etc/logrotate.d 디렉터리 안에 설정된 파일에 대해서 로테이트를 적용하는 설정

  * `nomissingok` : 로그 파일이 존재하지 않는 경우에 에러 메시지를 출력. 기본값으로 설정

  * `missingok` : 로그 파일이 존재하지 않는 경우 다음 파일로 이동

  * ```bash
    /var/log/wtmp {
    	monthly
    	create 0644 root utmp
    		minsize 1M
    	rotate 1
    }
    >>> 로그 파일명을 명기하면 별도로 지정이 가능. /var/log/wtmp 는 한 달마다 로테이트하지만, 파일 크기가 1MB가 되면 그 이전이라도 로테이트를 실행한다. 파일 생성 시에 허가권 값은 0644, 소유자는 root, 소유 그룹을 utmp 로 설정한다. 또한 로테이션으로 생성되는 백로그 파일은 1개만 생성한다.
    
    ```

* 설정 예시

  ```bash
  /var/log/httpd/access.log {
  	rotate 5
  	mail posein@gmail.com
  	size 100k
  	missingok
  	dateext
  	postrotate
  		/usr/bin/killall -HUP httpd
  	endscript
  }
  >>> /var/log/httpd/acces.log 파일에 대한 로테이션 설정을 파일의 크기가 100k 일 때 설정한다. 
  --> size 옵션은 기간 옵션 (daily,weekly...등)과 함께 사용할 수 없다.
  postrotate ~~ endscript 는 로그 파일이 로테이트 된 후에 실행될 명령어를 작성한다.
  
  ```

* 관련 파일 : /var/lib/logrotate.status

  * 각 로그 파일별로 로테이션된 날짜가 기록된 파일. 

##### 로그 관련 주요 파일

1. `/var/log/messages` : 시스템 표준 메시지 기록, `root` 만 조회 가능

2. `/var/log/secure ` : 인증에 기반한 접속, 로그 `login, telnet, ssh, tcp_wrappers,xinetd` 등 

3. `/var/log/dmesg`: 시스템 부팅할 때 출력되었던 로그 기록, 보통 커널 부트 메시지

4. `/var/log/maillog`: `sendmail, dovecot` 등 메일 관련 작업이 기록되는 로그 파일

5. `/var/log/xferlog`: FTP 접속과 관련된 작업이 기록되는 파일, 총 14개 포맷 영역으로 구분

   current-time (접속한 현재 시간)
   transfer-time (전송된 총 시간 , 초 단위)
   remote-host (원격 호스트 IP) 
   file-size (byte 단위 파일 크기)
   filename (전송된 파일의 절대경로)
   transfer-type (a는 ascii b는 binary)
   special-acition-flag (특정한 action 발생 시? C : 압축 , U : 압축이 안되어있음, T : tar, _ : no action)
   direction ( 전송된 지시 , o : outgoing - 다운로드 , i : incoming - 업로드)
   access-mode (사용자가 어떤 형태로 login 했는지 ,a - 익명, g- guest = user , r = real = local user)
   username ( 로컬 사용자의 ID)
   service-name (발생 service 이름 명기, 보통 ftp 라고 기록)
   authentication-method (인증에 사용된 방법, 0 = none, 1 = RFC931 )
   authentication-user-id ( 인증 방법에 의해 되돌려지는 사용자 계정, method와 연계되어 0 이면 보통 * )
   comletion-status (전송 상태 , c = complete, i = incomplete 불완전 전송)

6. `/var/log/cron` : cron 스케쥴러 관련 기록

7. `/var/log/boot.log`: 부팅 시 발생 메시지 기록 ( 보통 데몬 관련)

8. `/var/log/lastlog` : telnet 이나 ssh 등으로 접속한 마지막 사용자 기록, 바이너리 파일, `lastlog` 로 확인

9. `/var/log/wtmp` : 콘솔,telnet,ftp 접속 사용자 기록, 재부팅 로그 , `last` 로 확인

10. `/var/log/btmp`: wtmp와 반대, 로그 접속 실패한 경우 기록, `lastb` 명령으로 확인



* 관련 명령어

  * `last` : 사용자의 로그인 정보, 재부팅 정보 등을 출력

    * 옵션 

      * `-f 파일명` : 로그 로테이션이 설정 되어 있는 경우 , 기본 로그 파일 이외의 다른 파일 기록 확인 
      * `-n 숫자` : 가장 최근부터 해당 숫자값 만큼 출력 ( `-숫자` 와 같음)
      * `-t YYYYMMDDHHMMSS` : 지정한 시간 이전에 로그인한 기록 출력
      * `-R` : IP 주소나 호스트명을 출력하지 않는다.
      * `-a` : 호스트명이나 IP 주소 필드르 맨 마지막에 출력한다. `-d` 옵션과 함께 사용된다 
      * `-d` : 호스트 이름이 존재하는 경우 IP 주소를 호스트 이름으로 변환하여 출력
      * `-F` : 로그인 및 로그아웃 시간을 출력
      * `-i` : 접속한 호스트의 IP 주소로만 출력
      * `-w` : 사용자 전체 이름이나 전체 도메인 이름을 전부 출력

    * 사용 예시

      ```bash
      last # /var/log/wtmp 가 만들어진 후 관련 정보를 출력
      last posein # posein 사용자의 로그인 정보를 출력
      last reboot # 시스템이 재부팅된 정보를 출력
      last -1 reboot # 가장 최근에 재부팅한 정보 하나만 출력
      last -f /var/log/wtmp.1 # wtmp.1 파일의 정보 출력
      last 2 # /dev/tty2로 로그인한 정보를 출력
      ```

  * `lastlog` : 각각의 사용자가 마지막으로 로그인한 정보를 출력해 주는 명령. 
    바이너리 파일은 /var/log/lastlog 의 내용을 출력

    * 옵션
      * `-u 사용자명` : 특정 사용자에 대한 정보만 출력
      * `-t 날짜` : 오늘부터 지정한 날짜만큼 거슬러 오라가 그 이후에 로그인한 사용자의 정보를 보여준다.

  * `lastb` : last와 반대되는 개념의 명령, 로그인에 실패 정보를 /var/log/btmp 에 기록하는데, 이 파일의 내용을 출력하는 명령. 기본적인 사용법은 last 명령과 동일하지만, root 만 사용 가능하다.

    * 옵션 ( last 옵션과 대부분 동일 )
      * `-f 파일명` : 로그 로테이션 설정이 되어 있는 경우, 기본 로그 파일 이외의 다른 로그 파일의 기록을 볼 경우에 사용
      * `-n 숫자`  : 가장 최근부터 해당 숫자값 만큼만 출력 (`-숫자`)
      * `-t YYYYMMDDHHMMSS` : 지정한 시간 이전에 로그인한 기록을 출력
      * `-R` : IP 주소나 호스트명을 출력하지 않는다.
      * `-a` : 호스트명이나 IP 주소 필드를 마지막에 출력
      * `-d  `
      * `-F`
      * `-i`
      * `-w`

  * `dmseg` : 커널 링 버퍼의 내용을 출력하고 제어하는 명령이다. (커널의 동작과 관련된 메시지 기록)

    * 4096byte -> 8192 byte -> 16384byte -> 512KB 
    * 옵션 `-c` : 커널 링 버퍼에 저장된 메시지 출력 후 지운다.



#### 시스템 보안 관리

##### 리눅스와 보안 

1. 물리적 보안

2. 불필요한 서비스 제거

3. 시스템 정보 감추기

4. rott 패스워드 변경 제한

   * ***grub*** : root 패스워드를 변경, 복원 - 로컬 접근이 가능한 사용자가 시스템 재부팅 과정을 거쳐, 손쉽게 root 의 패스워드를 변경할 수 있다.

     * GRUB 패스워드 설정

       * `grub` - `md5crypt` 명령 실행, password 설정

       * 편집기를 이용하여 `/boot/grub/grub.conf` 에 

         `password --md5 ~~~~~~ ` 추가 

5. 사용자 관리

   일반 계정 권한을 얻고 나면 root 권한 획득이 손쉽다.

   일반 계정 사용자이면서 UID가 0인 사용자를 찾아 관리해야한다.

   `PAM, su , sudo ` 등 이용

6. 보안이 강화된 서비스 대체 이용 

   telnet 보다는 `ssh` 이용 ( telnet 은 평문 전송, ssh 는 암호문 전송)
   주요 보안 툴 : ***tcpdump , ethereal, wireshark***( 패킷 캡쳐링 도구)

7. 파일 시스템 관리 : `Set-UID, Set-GID, Sticky-Bit` 권한은 검색을 통해 필요없는 경우 제거

   `/etc/fstab/` 파일에 Set-UID 나 Set-GID 설정을 제한하는 `nosuid` 옵션 지정



##### sysctl 과 보안 

* 개요 : 리눅스 커널 제어를 위한 매개변수 . `/proc/sys` 디렉터리에 존재. 

* 일반적인 커널 매개 변수 변경 방법 

  * 예시 . ping 에 응답하지 않도록 설정하기?

    `sysctl -w net.ipv4.icmp_echo_ignore_all = 1` : 1 이 응답x, 0 이 기본으로 응답 o

* 명령어 `sysctl`

  * 옵션 

    * `-a,-A` : 커널 매개 변수와 값을 모두 출력
    * `-p` : 환경 변수 파일에 설정된 값 출력, 파일명 미지정 시 /etc/sysctl.conf 파일의 내용 출력
    * `-n` : 특정 매개 변수에 대한 값 출력
    * `-w 변수=값` : 매개 변수에 값을 설정

  * 예시

    ```bash
    sysctl -a # 커널 매개 변수와 값 모두 출력
    sysctl -p # 환경 변수 팡리 설정값 출력
    sysctl -n net.ip4.icmp_echo_ignore_all #-n 뒤의 내용 출력
    sysctl -w net.ip4.icmp_echo_ignore_all=0 #변수 변경 ( 핑 응답 O )
    ```

##### SSH (Secure Shell)

* 개요 : 패킷 전송 시 암호화 시켜 안전하게 전송 가능함. 

  * ssh2 와 ssh1 두가지 버전이 존재. 
  * ssh2 는 이중-암호화 RSA 키 교환을 비롯한 다양한 키-교환 방법을 지원.

* ssh 특징 

  * `rlogin` 처럼 패스워드 입력 없이 로그인이 가능
  * `rsh` 처럼 원격 셸 지원
  * `scp` 를 이용한 원격 복사 지원
  * `sftp` 를 이용한 안전한 파일 전송

* ssh 환경 설정 파일?

  * `/etc/ssh/sshd_config` 에 환경 설정 파일 존재
  * `/etc/rc.d/init.d/sshd` 에 스크립트

* 명령어 사용

  ```bash
  ssh [option] 호스트명 or IP 주소
  ssh 계정이름@호스트네임
  ssh 호스트네임 명령
  ```

  * 옵션
    * `-l` : 다른 계정으로 접속할 때 사용, 이 옵션 대신 서버 주소 앞에 @ 붙여 사용 가능
    * `-p`: ssh 서버의 포트 번호가 22번이 아닐 경우 -p 옵션을 사용해서 바뀐 포트를 지정할 수 있다.

  ```bash
  ssh 203.247.40.246 
  ssh -l yuloje 192.168.1.1 #192.168.1.1 서버로 클라이언트의 계정과 다른 계정인 yuloje 로 접속 시도
  ssh yuloje@192.168.1.1 # 192.168.1.1 서버의 yuloje 계정으로 접속 시도
  ssh -p 180 192.168.1.1 # 포트 번호가 22가 아닌 180일 경우 사용 
  ssh -l yuloje posein.org mkdir data #poseing.org 에 yuloje 계정으로 접속하여 data 디렉토리 생성
  ```

* **ssh-keygen**

  * 옵션

    `-t` : 사용할 암호화 알고리즘을 지정하는 옵션, rsa,dsa 사용 가능. ssh2 버전은 default로 rsa 사용 

  * ssh-keygen -t dsa : dsa 를이용하여 인증키를 생성

  * ssh-keygen 입력시?

    * RSA 를 이용하여 인증키 생성 : id_rsa 와 id_rsa.pub 생성

      패스워드를 설정하지 않으면 서버 접속시 패스워드 필요없음.

      id_rsa.pub 은 클라이언트에서 생성된 공개키로, server의 .ssh/authorized_keys 에 복사해둔다.

      복사 시에는 scp를 사용하자.

      `scp .ssh/id_rsa.pub 203.247.40.246: .ssh/authorized_keys` : 공개키 복사

      `ssh 203.247.40.246` : 접속

##### PAM (Pluggable Authentication Module)

* 개요 : 사용자를 인증하고 그 사용자의 서비스에 대한 접근을 제어하는 모듈화된 방법

* 구성 

  * 라이브러리 : `/lib/security 또는 /lib64/security` 에 위치 , 동적으로 로드 가능한 

    `.so` 형태로 되어 있다. 

  * 서비스  `/etc/pam.d` 디렉터리 안에 설정. 특별히 지정되지 않는 서비스에 대한 인증은
    `/etc/pam.d/other` 에서 관리

* PAM 주요 모듈

  * **pam_securetty.so** : 접속하는 계정이 root 인 경우 `/etc/securetty` 파일에 기록된 터미널을 통하는 경우에만 허가하도록 한다. 
    * `/etc/pam.d/login` , `/etc/pam.d/remote` 파일에 설정되어 있다. telnet 이 적용받는다.



##### sudo (Superuser do)

* 개요 : 특정 사용자가 root 사용자 권한을 가질 수 있도록 일부 명령 또는 모든 명령을 실행할 수 있도록 한다.
* `visudo` 명령 실행 시 vi 편집기가 `/etc/sudoers` 파일을 열면서 설저하도록 되어 있다.
* 적용된 사용자는 `sudo` + 명령어 형태로 사용
  * 예시 . `sudo /usr/sbin/useradd joon` : joon 이란 사용자 추가한다.
  * `yuloje ALL=/usr/sbin/useradd, /usr/bin/passwd` : yuloje 에게 root 권한의 useradd 및 passwd 명령을 부여하고, 접속한 곳에 상관없이(ALL) 사용 가능하다.
  * `jalin localhost=/usr/sbin/useradd, /usr/bin/passwd` : jalin 에게 root 권한의 useradd 및 passwd 명령을 부여하나, localhost 에서 접속한 경우에만 허용



##### 파일 시스템 보안

* lsattr : 파일에 설정된 속성 확인
* chattr : 파일의 속성을 변경하는 명령, root 사용자만 사용 가능
  * `chattr [option] mode 파일명 `
  * `mode` 는 `+ - = ` 을 사용한다. + 는 부여 - 는 해제, = 해당 속성만 부여하고 해제
  * `+a` : 파일 삭제 불가, 추가만 가능
  * ***`+i`: 파일 삭제나 변경 불가***
  * `-i +a` : i속성 제거 a속성 부여
* ACL (Access Control List)
  * 파일이나 디렉터리에 접근 권한을 제어할 수 없도록 만든 시스템
  * `getfacl [option]`
  * `setfacl` 
    * `setfacl -m u:posein:rw jalintxt` : posein 사용자가 jalin.txt 파일에 대해 읽고(read) 쓸 수 (write) 있는 권한을 부여한다. 



##### 주요 보안 도구

1. nmap ( network mapper ) : 네트워크 탐지 도구 및 보안 스캐너 ( 포트 스캔 )

   동종 계열 : WPScan,Nikto 등

2. tcpdump : 네트워크 트래픽 모니터링 도구

   동종 계열 : ethereal, wireshark

3. tripwire : 파일 무결성 검사, 파일의 변조여부 검사 도구

   Backdoor 감지.

4. nessus : 취약점 검사 도구 

   유사 계열 : COPS, SATAN 

5. GnuPG : 공개키와 비밀키를 생성하여 암호화하는 기법인 OpenGPG의 공개 버전

   비밀키를 가진 사용자만이 파일을 해독(복호화)할 수 있다.

6. John The Ripper : 패스워드 크랙 도구, 다양한 운영체제를 지원

   암호화된 패스워드가 들어 있는 파일과 암호로 사용될 만한 형태를 대입하여 패스워드를 알아낸다??

##### SELinux(Security Enhanced Linux)

* 보안이 강화된 리눅스 . root 권한을 획득하더라도 해당 데몬에서만 root 권한이 적용

* `/etc/selinux/config` 또는 vi 편집기로 수정

  `getenforce` : 현재 상태 확인

  `setenforce` :  SELinux 설정 ( 0 하면 Permissive 1 하면 Enforcing )



#### 시스템 백업



