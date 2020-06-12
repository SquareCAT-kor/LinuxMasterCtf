# 리눅스 tar, yum, rpm 과 그 옵션

#### 1. 리눅스 tar 옵션 

tar 사용법과, tar 백업에 관하여

* tar : Tape Archiver
  * -f : 대상 tar 아카이브 지정 
  * -c : tar 아카이브 생성 및 덮어쓰기 ( 파일 묶기 =create? )
  * -x : tar 파일 추출 ( 압출 풀기 = extract )
  * -v : verbose 자세하게 나열 ( 과정 보이게함 )
  * -z : gzip 압축 옵션 (gz) 파일
  * -j : bzip2 압축 옵션 (bz) 파일
  * -t : tar 아카이브에 **포함된 내용 확인**
  * -C : 대상 디렉토리 경로 지정 
  * -A : 지정된 파일을 tar 아카이브에 추가 
  * -d : tar 아카이브와 파일 시스템 간 차이점 검색 (diff)
  * -r , -u : tar 아카이브의 마지막에 **파일들 추가**
  * -k : 아카이브 추출 시 기존 파일 유지
  * -U : 아카이브 추출 전 기존 파일 삭제
  * -w : 모든 진행 과정에 대해 확인 요청 (interactive)
  * -e : 첫 번째 에러 발생 시 중지

* 예시
  * backup.tar 파일에 추가로 파일 압축할 떄 ?
    * tar -rvf backup.tar lin.txt joon.txt
  * 추가된 내용 확인?
    * tar -tvf backup.tar

* tar로 증분 백업?
  * -g : tar -g list -cvfp home1.tar /home
    * list 라는 파일의 내용을 토대로 증분 백업을 실시한다.
  * `tar -g list -cvfp home2.tar /home 
    * list 라는 파일을 토대로 증가된 것만 home2.tar 로 백업한다.



#### 2. yum 명령어

* yum install 
* yum search
* yum list
* yum remove
* yum info

#### 3. rpm ( Redhat Package Manager)

 * 설치 ( -i 계열 )
    * -v (verbose) 자세히
    * -h (hash)
    * -U(Upgrade) 
    * -p (package file)
       * --replacepkgs
       * --replacefiles
       * --oldpackage
       * --force(pkgs+files+old)
    * rpm -i 패키지명
    * rpm -ivh 패키지명 ( verbose, hash 옵션 추가)
    * rpm -ivh 패키지명 --replacepkgs ( 설치되어 있는 패키지 교체 옵션)
 * 확인 및 질의?
    * rpm -q (query)
       * -a (all) 
       * -l(list)
       * -i (information)
       * -f (files)
       * -R (Requires)
       * -c (configure files)
       * -d (document files)
 * 제거 (-e 계열)
    * rpm -e 
      	* --nodeps(no dependencies) : 의존성 무시 삭제