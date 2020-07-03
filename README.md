<h1> 리눅스 마스터 1급 자습 페이지 </h1>
> 핵심개념 ~ 유용한 개념 위주로 정리 
>
> 기출문제 위주로 정리 파일 업데이트
>
> https://www.notion.so/1-4b32dc4d3bf840139fcf05492761399d 노션 링크 추가
>
> 2020.06.13 리눅스마스터 1급 1차 시험 응시 - 결과발표 7월
>
> 2020.07.02 리눅스마스터 1급 1차 합격
>
> 2020.07.06 접수 예정
>
> 2020.08.08 리눅스마스터1급 2차 시험 예정



## 1. 소프트웨어 라이센스 정리

* GPL ( General Public License)
  * 어떠한 목적으로도 사용 가능 ( 위법 제한 )
  * 소스 코드 반드시 공개, 프로그램의 복사본은 무료배포 해야한다. 
* LGPL ( Lesser General Public License)
  * 독립적, 독점 소프트웨어에서도 사용가능. 수정 시 라이브러리 소스코드 반드시 공개
  * LGPL 로 개발 후 GPL 로 변경 가능하나 GPL 을 LGPL 로 임의변경은 불가하다.
* BSD ( Berkely Software Distribution)
  * 누구나 개발 및 사용 가능, 2차 수정본 또한 소스코드 공개가 의무가 아님.
* Apache 
  * 소스코드는 비공개 가능하나, Apache 제단의 프로그램을 사용하였음을 반드시 명시
* MPL ( Mozilla Public License)
  * 소스코드 수정시에는 공개가 필수, 다른 코드와 결합한 경우 다른 코드는 공개하지 않아도 된다.



## 2. Linux 배포판의 종류 , 계열

* 최초 배포판 : SLS - Softlanding Linux system -> Slackware -> Yggdrasil -> Debian Project
* 분류 ( 패키지 관리 기법에 따라 분류)
  * 슬랙웨어 : 소프트웨어를 최상단에서 최대한 수정하여 배포, 내장 프로그램 사용은 편리하나, 새로운 패키지를 적용하거나 수정하기가 어렵다.
    * **SuSE 수세**, Porteus, Vector, Salix OS 등
  * 데비안 : dpkg, apt 라는 독자적 패키지 관리 도구 사용 
    * **Ubuntu 우분투**,  Linux Mint , Knoppix, Corel, Lindows, Elementary OS
    * 해킹 및 보안 관련 정보 보안 내장 : BackTrack --> Kali Linux  
  * 레드햇 : RPM 및 YUM 패키지 관리 도구 사용  ( RHEL : Red Hat Enterprise Linux)
    * **CentOS**, Fedora, Oracle Linux, Scientific Linux, Asianux, Mandrivia Linux, Mandrake 등.



## 3. Linux 클러스터링

* 클러스터 cluster : 무리, 송이, 한 덩어리 - 여러 대의 컴퓨터를 연결하여 하나의 컴퓨터처럼 구성하는 시스템.
  * HPC - 고계산용 클러스터 : 고성능의 계산 능력을 제공하기 위한 목적, 과학계산용 ( 슈퍼컴퓨터 , 베어울프 컴퓨터 ) 
  * LVS - 부하분산 클러스터 : Linux Virtual Server , 대규모의 서비스를 제공하기 위한 목적으로 사용되는 클러스터 기법, 웹 서비스에서 주로 사용 . Load Balancer 를 두고 운영한다. 
  * HA - 고가용성 클러스터 : High Avaliablity , LVS 와 연동하여 지속적인 서비스 제공을 목적. 
    * Primary Node가 부하분산 처리를 수행하고 다른 하나의 Backup Node 또는 Secondary Node 가 Primary Node 오류 발생 시 분산처리를 수행하는 방식.

## 4. 클라우드 컴퓨팅 (Cloud Computing)

1. 정의 : IT 자원들은 어디에나 존재하고, 사용자는 단지 필요할 때 활용하기만 하면 된다.
   * Gird 컴퓨팅, 분산 컴퓨팅, 유틸리티 컴퓨팅, 웹 서비스, 서버 및 스토리지 가상화 기술 등 기존의 개발 기술들을 융합하여 하나의 커다란 구름( Cloud ) 와 같은 컴퓨팅 환경을 만드는 기술  
   * 서로 다른 물리적인 위치에 존재하는 컴퓨팅 자원을 가상화 기술로 통합하여 제공하는 기술 개념 
   * 인터넷을 이용한 IT 자원의 주문형 ( On - demand ) 아웃소싱 서비스, 이용자 중심의 컴퓨팅 환경
2. 클라우드 컴퓨팅의 제공 서비스
   1. IaaS : Infrastructure as a Service - IT 하드웨어 자원을 클라우드 서비스로 빌려서 사용
      * Amazon Ec2, S3, Infrastructure 
   2. PaaS : Platform as a Service - 소프트웨어를 개발할 수 있는 환경(플랫폼) 을 클라우드에서 제공받음
      * Apps, AppEngine, DB Server, Google, Servers
   3. Saas : Software as a Service - 기업에서 사용하는 소프트웨어를 통째로 클라우드 서비스 사업자에게 빌려 쓰는 개념
      * Apps, GoogleApps, SalesForce CRM 



## 5. 리눅스 하드웨어

* 리눅스, 하드웨어에서의 IDE = Integrated Drive Eletronics -> ATA 

  * IDE 디스크 , ATA : /dev/hda, /dev/hdb   ( hdx )
  * SCSI, S-ATA, USB Memory, SSD : /dev/sda, /dev/sdb ( sdx )

* RAID : Redundant Array of Independent Disks  : 여러개의 하드디스크가 동일한 데이터를 다른 위치에 중복해서 저장하는 방법  - Parity, ECC 사용 등 구성 방법에 따라 다양한 형태로 존재

  * 백업 가능
  * 안정적인 데이터 보존, 유지, 속도 향상
  * 소프트웨어적 구현, 하드웨어적 구현 가능 (비용 및 성능 하드웨어 > 소프트웨어)
  * 전원이 켜져있는 상태에서 하드드라이브를 교체할 수 있는 Hot Swap Bay 

  1. RAID 사용 기술
     * 스트라이핑 Striping : (줄무늬할때 stripe) - 연속된 데이터를 여러 개의 디스크에 라운드로빈 방식으로 기록 --> 속도가 디스크 개수만큼 빨라진다.
     * 미러링 Mirroing : (결함허용) 데이터 손신을 막기 위해 추가적으로 하나이상의 장치에 중복 저장.
  2. RAID 종류
     1. RAID -0 : 복구 불가, 스트라이핑 사용 - 데이터 중복 및 패리티 없이 분산 기록 / 매우 빠른 속도 
     2. RAID -1 : 미러링 기술 사용, 스트라이핑 미사용 - 읽기 성능은 향상, 쓰기성능은 단일과 같음. 중복 저장으로 인해 디스크 공간 낭비 50%  
     3. RAID -2 : 디스크들은 스트라이핑 기술 이용, 에러 감지 및 수정을 위한 ECC 기술 사용 (Error Check & Correction )
     4. RAID -3 : 스트라이핑 기술을 사용한 디스크 구성, 패리티 정보를 별도의 디스크에 저장. 입출력 겹치게 할 수 없으며, 대형 레코드가 많은 시스템에서 사용 (DIsk 0,1,2,3 & Parity Disk of 'Disk 0to3')
     5. RAID -4 : 블록 형태의 스트라이핑 기술을 사용하여 디스크를 구성, 읽을때는 데이터 중첩 입출력이 가능하나, 쓸때는 불가하다. 입출력 중첩이 불가능하고 시스템에 병목현상이 발생할 수 있음.
     6. RAID -5 : **결함허용 + 공간효율 향상**
        최소 3개의 디스크로 구성, 패리티 또한 분산 기록한다. 데이터를 중복 저장하지 않아 가장 보편적으로 사용. 모든 읽기 및 쓰기가 중첩이 가능.
        * 1/디스크의 개수 만큼의 공간을 패리티에 사용-> 디스크가 늘어날 수록 공간효율성이 늘어남. 
     7. RAID -6 : RAID -5 + 2차 패리티 구성 : 최소 4개의 디스크 사용, 처리 속도는 낮아지나 신뢰도가 올라간다. 
     8. RAID -7 : 하드웨어 컨트롤러에 내장되어 있는 실시간 운영체제를 사용하여 구성. (BUS 이용)
     9. RAID 0+1 : 디스크 2개를 RAID - 0 의 스트라이핑 기술로 구성 / RAID-1 의 미러링으로 구성. 
        * 최소 4개의 디스크 사용
          EX:  6개 (1,2,3,4,5,6) 사용시 3개(1,2,3)를 RAID-0로 구성 하여 1개처럼 만들고, (1,2,3),4 | 5,6 형태로 만들어 4개로 사용한다.  
     10. RAID -10 : RAID 0+1의 반대 개념 - 디스크 2개를 먼저 미러링 한 후 스트라이핑 구성
     11. RAID -53 : RIAD-3 방식에 별도로 스트라이프 Array 추가 구성. 높은 성능에 비하여 높은 구성비용.

* LVM (Logical Volume Manager)

  * 하드디스크를 추가하여 파티션을 분할하고 공간을 할당 할때,  설정한 공간의 크기가 고정이 되어 변경 및 용량 증설이 어려울 때, 이를 해결하기 위해 사용.
* LVM 구성도, 관련 용어
    * PV Physical Volume : 물리적 볼륨 - 실제 디스크에 물리적으로 분할한 파티션 
    * VG Volume Group : 볼륨 그룹 PV 가 모여서 VG , LVM 구성 단위인 PE(Physical Extent) 가 모여서 생성되는 하나의 덩어리
    * LV Logical Volume : 파티션 개념 , VG에서 사용자가 필요한 만큼 할당하여 만들어지는 공간
    * PE Physical Exent : 물리적 확장 , Block 영역, 1PE = 4MB 정도됨.
  * 요약 - 물리적용량 PV 를 합쳐서 VG로 만들고 LV 로 나누어 사용한다. 