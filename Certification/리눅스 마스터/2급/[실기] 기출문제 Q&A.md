# [리눅스 마스터 2급 실기] 기출문제 Q&A

<br>

**Q. RAID-5를 구성할 때 몇개의 디스크 용량분을 패리티로 사용하는가?**
<details>
<summary>A.</summary>
RAID -5는 디스크 1개 분의 용량을 패리티 용도로 사용한다.
<br>

디스크 여러개의 일부 용량을 분할하여 총 1개의 용량을 패리티 용도로 사용한다.
</details>

<br>

**Q. 다음 설명에 해당하는 RAID 관련 기술로 알맞은 것은?**
> 연속된 데이터를 여러 개의 디스크에 라운드 로빈 방식으로 기록하는 기술로 하나의 디스크에서 읽어드리는 것보다 더 빠르게 데이터를 읽거나 쓸 수 있다.
<details>
<summary>A.</summary>
스트라이핑
<br>

여러 디스크를 하나로 합쳐서 하나의 디스크처럼 사용하는 기술,
하나의 데이터를 여러 디스크에 나눠 써 이론 상 디스크 수만큼 읽기/쓰기 속도를 극대화 하는 기술,
RAID-0에서 활용된다.
* 장점 : 높은 속도와 100% 용량 활용 가능
* 단점 : 데이터 보호 기능이 없으므로 고장 시 데이터 손실 위험
* [부록] RAID 버전 별 사용되는 기술
  * RAID1 : 미러링 - 디스크를 이중화
  * RAID5, 6 : 패리티 - 디스크 하나에 대해 손실된 데이터 복구
  * RAID3 : ECC(Error check - Correction) - 바이트 단위 스트라이핑, 전용 패리티 디스크를 사용해 에러 복구
</details>

<br>

**Q. 다음 중 LVM을 구성할 때 가장 먼저 생성되는 것은?**
> 1. VG(Volume Group)
> 2. LV(Logical Volume)
> 3. PV(Physical Volume)
> 4. PE(Physical Extent)

<details>
<summary>A.</summary>
PV - VG - LV 순이다.
<br>

LVM : Logical Volume Manager로 리눅스에서 디스크 파티션 관리, 확장, 제어를 위한 시스템 도구
* PV : 물리적인 저장장치 (HDD, SSD, 파티션) 등을 나타냄
    * ex) /dev/sda, /dev/sdb와 같은 디스크
* VG : 여러개의 PV를 하나로 묶은 논리적인 그룹
* LV : 실제로 데이터를 저장하는 논리적인 볼륨
  * VG에서 할당된 공간을 바탕으로 LV를 만든다.
* PE : VG 내부를 고정된 크기로 나눈 블록 (해당 없음)
</details>
 
<br>

**Q. 프린터 큐의 작업 정보를 확인하는 명령어는?**
<details>
<summary>A.</summary>
lpstat
<br>

* lp : 프린터 디바이스 path를 사용해 직접 인쇄
* lpr : 인쇄 작업을 수행 (lp run)
* lprm : 프린터 큐의 작업을 삭제할 때 사용 (lp remove)
</details>

<br>

**Q. 리눅스 및 유닉스 계열 운영체제에 사운드를 만들고 캡처하는 기능을 하며, POSIX에 기반을 둔 인터페이스는?**
<details>
<summary>A.</summary>
OSS(Open Sound System)
<br>

* ALSA(Advanced Linux Sound Architecture) : 사운드 카드를 자동으로 구성하며, 사운드 장치 관리를 목적으로 사용
* CUPS : 애플이 개발한 오픈 소스 프린팅 시스템
* SANE(Scanner Access Now Easy) : 스캐너 관련 API
</details>

<br>

**Q. 데비안 계열 리눅스에서 환경 설정 파일을 포함해 vsftpd 패키지를 제거하는 명령은?**
<details>
<summary>A.</summary>
apt-get purge vsftpd
<br>

* apt-get 하위 삭제 관련 명령어
    * purge : 환경설정까지 삭제
    * remove : 패키지 제거 (erase와 동일)
* apt-get : advanced packagin tool - get의 약자로, 리눅스의 소프트웨어 패키지 처리 시스템
  * apt-get 하위 기타 명령어
    * update : 패키지 저장소의 최신 패키지 명령어와 버전 정보를 가져옴 (업데이트 수행은 X)
    * upgrade : 업데이트를 수행
    * clean : .deb 패키지파일 같은 캐시 파일을 삭제
    * autoremove : 더이상 필요 없는 의존성 패키지를 제거
    * dist-upgrade : upgrade보다 강력한 업데이트 수행
</details>

<br>

**Q. rpm 명령으로 의존성이 존재하는 nmap 패키지를 제거하는 명령은?**
<details>
<summary>A.</summary>
rpm -e nmap --nodeps
<br>

* -e : 삭제 명령
  * -d나 erase, delete는 잘못된 문법
* --nodeps : 의존성 체크를 무시하고 삭제
</details>

<br>

**Q. yum으로 확장 패키지 관련 저장소를 설치하는 명령어는?**
<details>
<summary>A.</summary>
yum install epel-release
<br>

* epel-release : Extra Packages for Enterprise Linux - Release 의 약자이다.
  * epel-release를 설치하면 yum 과 같은 도구를 사용해 EPEL 저장소에서 패키지와 의존성 패키지를 설치할 수 있다.
* 존재하지 않는 패키지 이름
  * epel, epel-repository, epel-download
</details>

<br>

**Q. abc.tar.bz2 소스 파일의 내용만을 확인하는 tar 명령어는?**
<details>
<summary>A.</summary>
tar.jxvf abc.tar.bz2
<br>

* tar 명령어에서 압축 파일을 다룰 떄 사용하는 명령어
  * .xz = **J**xvf
  * .bz2 = **j**xvf
  * .gz = **z**xvf
  * compress = **Z**xvf
* 함께 사용하는 공통 옵션
  * c : 압축파일 생성 (**c**reate)
  * x : 압축파일 해제 (e**x**tract)
  * t : 아카이브 내 파일 목록 보기 (lis**t**)
  * v : 자세한 정보 출력 (**v**erbose)
  * f : 파일 이름 및 디렉토리 지정 (**f**ile)
</details>

<br>

**Q. 소스 파일의 압축을 푼 디렉터리에서 한 번 작업한 설정이나 파일을 삭제하고 작업을 진행할 때 사용하는 명령어는?**
<details>
<summary>A.</summary>
make clean
<br>

생성되는 중간 파일 (컴파일된 파일, 빌드 환경)을 삭제하고 이전 빌드의 영향을 없애기 위해 사용한다. 
</details>

<br>

**Q. 프로그램을 소스 파일로 설치하는 과정의 명령어는?**
<details>
<summary>A.</summary>
configure -> make -> make install
<br>

* configure : 소스 코드를 컴파일하기 전에 시스템 환경을 검사하고, makefile을 생성
* make : makefile에 기록된 지시에 따라 소스 코드를 컴파일, 실행 파일을 생성
* make install : 컴파일된 프로그램을 시스템의 적절한 위치에 설치
</details>

<br>

**Q. 다음 중 리눅스에서 사용되는 온라인 패키지 관리 도구로 거리가 먼 것은?**
> 1. dnf
> 2. rpm
> 3. zypper
> 4. apt-get
<details>
<summary>A.</summary>
rpm은 redhat package manager로, 오프라인 패키지 관리 또한 수행한다.

<br>

* dnf : 페도라 및 레드햇 계열 온라인 패키지 도구
* zypper : 오픈수세(OpenSUSE) 에서 사용하는 온라인 패키지 도구
* apt-get : Debian 및 Ubuntu 계열에서 사용하는 온라인 패키지 도구
</details>

<br>

**Q. 다음 중 레드햇 계열 리눅스에서 사용되는 온라인 패키지 관리 도구로 거리가 먼 것은?**
> 1. dnf
> 2. rpm
> 3. zypper
> 4. apt-get
<details>
<summary>A.</summary>
zypper는 오픈수세(OpenSUSE) 에서 사용하는 온라인 패키지 도구이다.

<br>

페도라, 데비안, 우분투는 레드햇 계열이다.
</details>

<br>

**Q. vi 편집기의 ex 명령모드에 대한 설명으로 틀린 것은?**
> 1. w : 작업중인 내용을 저장한다.
> 2. w 파일명 : 지정한 파일명으로 저장한다.
> 3. wq : 변경된 내용을 저장하고 종료한다.
> 4. q : 수정된 사항이 있어도 무조건 종료한다.
<details>
<summary>A.</summary>
q는 수정된 내용이 있으면 종료되지 않고 오류 메시지를 출력한다.
<br>

강제 종료하려면 q!를 사용해야 한다.
</details>

<br>

**Q. 다음 괄호 안에 들어갈 내용으로 알맞은 것은?**
> vi 편집기의 명령 모드 상태에서 특정 문자열을 아래 방향으로 검색하기 위해서는 (a) 기호를 선언한 뒤, 
> 찾으려는 문자열 패턴을 덧붙여서 기재한다. 만약 다음 문자열을 찾으려면 (b) 키를 이용한다.
<details>
<summary>A.</summary>
a : /, b : n 
<br>

* /(검색어) : 커서 위치부터 순방향(아래)로 문자열 검색
* n : 검색어를 찾은 다음 위치로 이동
* N : 검색어를 찾은 이전 위치로 이동
* ?(문자열) : 커서 위치부터 순방향(위)로 문자열 검색
</details>
<br>

**Q. vi 편집기에서 linux로 끝나는 줄의 마지막에 마침표(.)를 덧붙이도록 치환하는 명령어는?**
> vi 편집기의 명령 모드 상태에서 특정 문자열을 아래 방향으로 검색하기 위해서는 (a) 기호를 선언한 뒤,
> 찾으려는 문자열 패턴을 덧붙여서 기재한다. 만약 다음 문자열을 찾으려면 (b) 키를 이용한다.
<details>
<summary>A.</summary>
:% s/linux$/linux./
<br>

* % : 전체 파일의 범위를 나타내는 기호, 파일 전체에서 찾고자 하는 패턴을 검색한다.
* s : substitue : 치환을 나타내는 명령어
* linux$ : $는 줄의 끝을 의미하므로 linux로 끝나는 부분을 찾는다.
* linux. : linux로 끝나는 부분에 마침표를 추가한다.
</details>

<br>

**Q. 다음 중 emacs 편집기를 개발한 인물로 알맞은 것은?**
> 1. 빌 조이
> 2. 리처드 스톨만
> 3. 브람 브레나르
> 4. 귀도 반 로섬
<details>
<summary>A.</summary>
리처드 스톨만
<br>

* 빌 조이 : vi 개발
* 리처드 스톨만 : emacs 개발
* 브람 브레나르 : vim 개발
* 귀도 반 로섬 : python IDLE 개발
</details>

<br>

**Q. nano 편집기에서 현재 커서가 위치한 줄의 처음으로 이동할 때 사용하는 키 조합은?**
<details>
<summary>A.</summary>
Ctrl + a
<br>

* Ctrl + e : 현재 행의 끝 부분으로 커서를 이동
* Ctrl + o : 파일을 저장하기 위해 사용
* Ctrl + i : 탭을 삽입
</details>

<br>

**Q. 다음 중 X 윈도우 환경에서만 사용 가능한 편집기는?**
> 1. nano
> 2. pico
> 3. kwrite
> 4. vim
<details>
<summary>A.</summary>
kwrite은 X윈도우 환경에서만 사용 가능한 GUI 편집기이다.

<br>

* nano : CLI 기반 편집기
* pico : nano의 원조인 터미널 기반 편집기
* vim : CLI에서 동작하지만, GUI 버전인 gvim 또한 존재한다.
</details>

<br>

**Q. 백그라운드 프로세스에서 작업번호가 2번인 백그라운드 프로세스를 종료시키는 명령은?**
<details>
<summary>A.</summary>
kill %2

<br>
* kill %2 : 현재 쉘 세션에서 2번째 백그라운드 작업을 종료
* kill 2 : PID가 2인 프로세스 종료
</details>

<br>

**Q. ps 명령의 상태 코드 중에 작업은 종료되었으나 부모 프로세스에 의해 회수되지 않아 메모리를 차지하고있는 상태를 나타내는 코드는?**
<details>
<summary>A.</summary>
Z (Zombie) : 프로세스가 종료되었으나, 부모 프로세스가 아직 종료상태를 확인하지 않은 상태. 
일반적으로 시스템 리소스를 소비하지 않으며, 부모 프로세스가 해당 종료 상태를 처리할 때까지 존재한다.

<br>

* R (Running) : 프로세스가 실행 중인 상태
* S (Sleeping) : 프로세스가 현재 실행 대기 중인 상태
* T (Stopped) : 프로세스가 현재 중지된 상태, SIGSTOP 등 시그널을 받아 중지된 상태이며, 다시 시작할 수 있다.
</details>

<br>

**Q. 프로세스 관련 명령어로 설정 가능한 NI 값의 범위는?**
<details>
<summary>A.</summary>
-20 ~ 19, 기본값은 0이다.

<br>

* NI(Nice Value) : 프로세스의 스케줄링 우선순위를 조절하는 값 
</details>

<br>

**Q. cron을 이용해서 a.sh 스크립트를 매주 월요일 1시 1분에 주기적으로 실행하려고 한다면 cron 설정은 어떻게 해야 하는가?**
<details>
<summary>A.</summary>
1 1 * * 1 a.sh

<br>

* cron 단위별 설정 : 분 / 시간 / 일 / 월 / 요일

</details>

<br>

**Q. 프로세스 관련 명령어로 설정 가능한 NI 값의 범위는?**
<details>
<summary>A.</summary>
-20 ~ 19, 기본값은 0이다.

<br>

* NI(Nice Value) : 프로세스의 스케줄링 우선순위를 조절하는 값
  * 백그라운드에서 실행 가능한 프로세스의 우선순위를 낮춰 시스템 리소스 확보 가능
  * 사용자가 조절할 수 있는 범위의 값임
    * 일반 사용자는 NI 값을 높일 수만 있으며, 낮추려면 root 권한이 필요하다.
* PR(Priority) : NI + 커널이 측정한 프로세스의 우선순위 값
  * NI의 범주와 다르게 PR - NI 값은 커널이 자동 산정하는 값으로 조절이 불가능하다.
  * 최종적으로 PR 값이 낮을 수록 스케줄링 우선순위가 높으며, 높을 수록 우선순위가 낮다.
</details>

<br>

**Q. 다음 명령의 결과를 설명하시오.**
> `# nice bash`
<details>
<summary>A.</summary>
bash 프로세스의 우선순위를 낮춘다.

<br>

* nice <프로세스명> : nice (NI) 값을 10 증가시킨다.
  * 즉 
</details>

<br>

**Q. 포어그라운드 프로세스를 종료하기 위해 사용하는 키 조합은?**
<details>
<summary>A.</summary>
Ctrl + c

<br>

* Ctrl + c : 포어그라운드 프로세스를 종료할 때 사용
* Ctrl + d : 현재 세션에서 로그아웃
* Ctrl + z : 현재 실행중인 포어그라운드 프로세스를 일시 중지하고 백그라운드로 이동
* Ctrl + a : 현재 입력 라인의 시작 부분으로 커서를 이동

</details>

<br>

**Q. 다음 중 standalone 방식과 inetd 방식에 대한 비교 설명으로 알맞은 것은?**
> 1. inetd 방식이 standalone 방식보다 메모리 관리가 더 효율적이다.
> 2. inetd 방식이 standalone 방식보다 관련 서비스 처리가 빠르다.
> 3. 웹과 같은 빈번한 요청이 들어오는 서비스는 inetd 방식이 적합하다.
> 4. 사용자가 많은 서비스는 standalone 방식보다 inetd 방식이 적합하다.
<details>
<summary>A.</summary>
1. inetd 방식이 standalone 방식보다 메모리 관리가 더 효율적이다.
<br>

* inetd : 가끔 실행되는 서비스 방식
  * ex) ftp, telnet
  * 서비스 처리 속도는 느리나 메모리 절약 우수
* standalone : 항상 실행되는 서비스 방식 
  * ex) 웹서버, DB 서버
  * 빠른 서비스 처리 및 응답 속도이이나 메모리를 많이 사용
</details>

<br>

**Q. 사용자가 본인이 실행한 백그라운드 프로세스 목록을 확인하는 명령어는?**
<details>
<summary>A.</summary>
jobs 

<br>

* ps : 현재 실행중인 프로세스를 확인
* bg : 중지된 백그라운드 작업을 다시 실행
* jobs : 현재 셸에서 실행중인 작업의 목록 (백그라운드, 포그라운드를 모두 표시)
* exec : 셸 스크립트가 다른 프로세스로 대체

</details>

<br>

**다음 보기의 시그널을 번호값이 낮은 순 -> 높은 순으로 정렬 시 세번째에 해당하는 시그널 이름은?**
> 1. SIGSTOP
> 2. SIGKILL
> 3. SIGINT
> 4. SIGTERM
<details>
<summary>A.</summary>
4. SIGTERM

<br>

* 시그널 / 신호값 / 설명
  * SIGSTOP / 20 / 프로세스 정지 (Ctrl + z)
  * SIGKILL / 9 / 프로세스 강제종료
  * SIGINT / 2 / 프로세스 인터럽트 (Ctrl + z)
  * SIGTERM / 15 / 프로세스 정상 종료 요청 

</details>

<br>

**다음 괄호 안에 들어갈 내용으로 알맞은 것은?**
> * 하나의 프로세스가 다른 프로세스를 실행하기 위한 호출 방법에는 (ㄱ) 와 (ㄴ) 가 있다.
> * (ㄱ) 은 새로운 프로세스를 위해 메모리를 할당받아 복사본 형태의 프로세스를 실행하며, 기존의 프로세스는 그대로 실행되고 있다.
> * (ㄴ)은 원래의 프로세스를 새로운 프로세스로 대체하는 형태로 호출해 프로세스의 메모리에 새로운 프로세스 코드를 덮어 씌운다.
<details>
<summary>A.</summary>
ㄱ. fork, ㄴ. exec

<br>

* fork : 새로운 프로세스를 위해 메모리를 할당받아 복사본 형태의 프로세스 실행
* exec : 원래의 프로세스를 새로운 프로세스로 대체
* background : 백그라운드 상태에 있지만 동작 code가 있음
* foreground : 앱이 실행되어 사용자에게 보여지고 있는 상태

</details>

<br>

**Q. 다음 보기의 시그널을 번호값이 낮은 순 -> 높은 순으로 정렬 시 세번째에 해당하는 시그널 이름은?**
> 1. SIGSTOP
> 2. SIGKILL
> 3. SIGINT
> 4. SIGTERM
<details>
<summary>A.</summary>
4. SIGTERM

<br>

* 시그널 / 신호값 / 설명
  * SIGSTOP / 20 / 프로세스 정지 (Ctrl + z)
  * SIGKILL / 9 / 프로세스 강제종료
  * SIGINT / 2 / 프로세스 인터럽트 (Ctrl + c)
  * SIGTERM / 15 / 프로세스 정상 종료 요청

</details>

<br>

**Q. 모든 사용자에게 적용되는 alias와 함수를 설정하려고 할 때 사용하는 파일명은? **
<details>
<summary>A.</summary>
/etc/bashrc

<br>

* /etc/bashrc : 모든 사용자 대상, alias와 함수 설정
* /etc/profile : 모든 사용자 대상, 환경변수와 시작 관련 프로그램 설정
* ~/.bashrc : 개인 사용자 대상, alias와 함수 설정
* ~/.bash_profile : 개인 사용자가 정의한 환경변수와 시작 관련 프로그램 설정
</details>

<br>

**Q. 다음과 같이 입력했을 때, 결과값은?**
> `user=kaitman` <br>
> `echo "$user"`
<details>
<summary>A.</summary>
kaitman

<br>

* echo "$user" 명령은 user 변수 값 출력
* echo "$USER" 명령은 현재 환경변수인 사용자 이름 출력
* 큰 따옴표는 있으나 없으나 차이 없다.
* 작은 따옴표는 작은 따옴표 안의 문자가 그대로 출력된다. (별도 치환 없음)
</details>

<br>

**Q. 가장 최근에 실행한 명령을 재실행할 때 사용하는 명령은?**
<details>
<summary>A.</summary>
!!
<br>

* history : 최근에 실행한 명령의 히스토리를 출력한다.
* !1 ~ !n : 히스토리에 출력된 명령 목록에서 해당 시퀸스 번호의 명령을 재실행한다. 
</details>

<br>

**Q. 셸 변수를 선언한 후에 관련 내용을 확인하는 과정이다. 괄호 안에 들어갈 명령으로 알맞은 것은?**
> $ a=1 <br>
> $ b=2 <br>
> $ ( )
<details>
<summary>A.</summary>
set
<br>

* set :  옵션이나 인자 없이 set 명령어 사용하면 선언된 변수 및 함수를 출력한다.
  * set -o : 쉘의 옵션 활성화 (설정)
  * set +o : 쉘의 옵션 비활성화 (설정 해제)
* unset : 지정된 환경변수나 함수를 제거하는데 사용
* env : 현재 쉘 세션의 환경변수를 출력하거나 변경된 환경에서 명령을 실행
* printenv : 현재 쉘 환경의 환경 변수를 출력

</details>

<br>

**Q. 특정 사용자가 로그인 시에 부여되는 셸 정보를 확인할 수 있는 파일은?**
<details>
<summary>A.</summary>
/etc/passwd

<br>

* /etc/passwd : 시스템에 등록된 각 사용자의 계정 정보, 사용자 ID, 그룹 ID, 홈 디렉토리, 로그인 셸 정보를 포함
  * 로그인 셸 정보는 가장 마지막 레코드에 표기됨
* /etc/shells : 시스템에서 사용 가능한 셸 목록을 포함
* /etc/bashrc : 시스템 전체의 셀 세션에 대해 alias와 함수 설정 등 기본 설정 가능
* /etc/profile :  시스템 전체 시작 시 로그인 셸 세션을 위한 초기화 스크립트 파일
  * 로그인 셸 세션 시작 시 실행되는 파일
</details>

<br>

**Q. chsh (괄호) 는 사용자가 변경 가능한 셸의 목록 정보를 확인할 수 있다. 이 때 괄호 안에 들어갈 내용은?**
<details>
<summary>A.</summary>
-l : 사용자가 사용할 수 있는 쉘의종류를 확인한다.

<br>

* -u : help, 명령어의 사용법, 가능한 옵션에 대한 간단한 설명 제공
* -s : 사용자의 기본 셸을 변경
* -c : comment, 변경된 셸에 대한 추가적인 설명을 제공
* -v : version, 버전 확인
</details>

<br>

**Q. 히스토리, alias, 작업 제어와 같은 유용한 기능이 포함된 셸로 1978년에 버클리 대학의 빌 조이가 개발한 셸은?**
<details>
<summary>A.</summary>
csh
<br>

* bourne shell : unix 시스템에서 기본적으로 사용되는 셸
* dash : debian art shell의 약자로, debian에서 sh의 대체품으로 사용
* bash : bourne again shell의 약자로, GNU 프로젝트의 일환으로 만들어진 쉘
</details>

<br>

**Q. 현재 시스템에 마운트된 파일 시스템 정보를 저장하고 있는 파일로 실제 파일은 /proc/self/mounts 인 파일은?**
<details>
<summary>A.</summary>
/etc/mtab
<br>

* /etc/fstab : 파일 시스템 테이블을 나타내는 파일
* /etc/mtab : 현재 마운트된 파일 시스템 정보를 나타내는 파일
* /etc/mounts : 마운트 여부와 관계 없이 파일 시스템 정보를 보여주는 파일
* /proc/partitions : 현재 시스템에 있는 파티션 정보를 제공
</details>

<br>

**Q. 다음 중 /etc/fstab 파일의 첫번째 필드에 설정할 수 있는 값이 아닌 것은?**
> 1. UUID
> 2. LAVEL
> 3. 마운트 포인트
> 4. 장치 파일명
<details>
<summary>A.</summary>
3. 마운트 포인트
<br>

* 첫번째 필드에 설정할 수 있는 값은 장치명 =/dev/UUID, 라벨명, 네트워크 주소, 파일명
* 두번째 필드에는 마운트 포인트 설정이 가능
</details>

<br>

**Q. abc 사용자의 홈 디렉터리가 차지하고 있는 디스크 용량을 확인하는 명령어는?**
<details>
<summary>A.</summary>
du -sh ~abc 혹은 du -sh /home/abc
<br>

* du : 디렉터리의 디스크 사용량을 확인
* df : 디스크 공간에 대한 정보를 보여줌
* quota : 디스크 사용량 제한 및 현재 사용량을 확인
</details>

<br>

**Q. fdisk 작업 후 변경된 파티션 정보를 저장하고 종료하는 명령어는?**
<details>
<summary>A.</summary>
w (write)
<br>

* n : 새로운 파티션 생성 (new)
* q : 저장 안하고 종료 (quit)
* x : 전문가 모두 (expert)
</details>

<br>

**Q. 다음 결과를 출력하는 명령어로 알맞은 것은?**
> `dev/sda1 : UUID='7214520d-725c-441a-918c-a120912398f" TYPE="xfs"`
<details>
<summary>A.</summary>
blkid : block id, 블록 장치에 있는 파일 시스템의 UUID와 유형을 확인
<br>

* lsblk : 블록 장치에 대한 정보를 트리 형식으로 보여줌
* fdisk : 디스크 파티션을 관리하기 위한 도구
* UUID : UUID 생성 간에 사용
</details>

<br>

**Q. 다음중 설정된 umask 값이 022인 경우 생성되는 파일의 허가권 값은?**
<details>
<summary>A.</summary>
-rw-r--r--
<br>

* umask : 값을 빼야 할 권한을 지정하는 것이다.
  * 기본 권한에서 umask를 뺀 것이 최종 파일 / 디렉터리 권한이 된다.
  * 디렉토리의 최대 권한은 777, 파일은 666이다.
  * 따라서 umask값이 022인 경우 설정되는 파일의 권한은 644가 되며, 이를 이진수로 변환하면 -/rw-/r--/r--가 된다.
</details>

<br>

**Q. project 그룹에 속한 사용자들이 /project 디렉터리에서 파일 생성은 자유로우나 삭제는 본인의 생성 파일만 가능하도록 설정하려고 한다.
또한 파일 생성 시 자동으로 그룹 소유권이 project로 부여되도록 설정하려고 할때, 
project 디렉터리의 정보가 다음과 같을 때 어떤 명령을 해야 하는가?**
> `$ ls -ld /project` <br>
> $ `drwxr-x---` ... 
<details>
<summary>A.</summary>
chmod 3770 /project

<br>

* chmod ~ /디렉터리명 : 권한 설정을 하는 명령어
* 3770
  * 3 : 1 + 2의 특수 비트 값의 합
    * Sticky Bit(1) : 본인만 삭제 가능 / 위 문제에 해당
    * SGID(2) : 파일 생성 시 그룹 소유권 상속 / 위 문제에 해당
    * SGID(4) : 파일 실행 시 소유자 권한으로 실행되도록 하는 특수권한
  * 770 : rwxr-x 에서 소유자 제외 그룹도 읽기, 쓰기. 실행이 가능해야하므로 rwxrwx로 변경
</details>

<br>

**Q. 다음 명령의 결과로 설정되는 lin.txt 파일의 허가권 값으로 알맞은 것은?**
> $ ls -l lin.txt <br>
> -rw-rw-r--, ~ lin.txt <br>
> chmod g=r lin.txt
<details>
<summary>A.</summary>
-rw-r--r--

<br>

* g : 그룹을 의미함
* = : 덮어쓰기, 이후 우변처럼 권한이 변경됨
* ㄱ : 읽기 권한을 의미함
* 종합 : lin.txt의 그룹 권한을 읽기만 가능하게 변경하는 명령어
</details>

<br>

**Q. 다음 명령의 결과로 설정되는 lin.txt 파일의 허가권 값으로 알맞은 것은?**
> $ ls -l lin.txt <br>
> -rw-rw-r--, ~ lin.txt <br>
> chmod g=r lin.txt
<details>
<summary>A.</summary>
-rw-r--r--

<br>

* g : 그룹을 의미함
* = : 덮어쓰기, 이후 우변처럼 권한이 변경됨
* ㄱ : 읽기 권한을 의미함
* 종합 : lin.txt의 그룹 권한을 읽기만 가능하게 변경하는 명령어
</details>

<br>

**Q. 파일이나 디렉터리의 소유자를 변경하는 명령어는?**
<details>
<summary>A.</summary>
chown
<br>

* ls : 파일 및 디렉토리의 목록을 표시
* chgrp : 파일 및 디렉토리의 그룹 소유권을 변경하는데 사용
* chown : 파일 및 디렉토리의 소유자와 그룹을 변경하는데 사용
* umask : 새로운 파일 및 디렉토리 생성 시 권한 제어를 위해 사용
</details>

<br>

**Q. 클라우드 서비스에서 이용자의 설정이 많은 순으로 나열된 것은?**
> 1. SaaS
> 2. PaaS
> 3. IaaS
<details>
<summary>A.</summary>
3 > 2 > 1

<br>

* IaaS : Infrastructure as a service
* PaaS : Platform as a service
* Saas : Software as a service
* 따라서 규모 순으로 보았을 때 인프라 < 플랫폼 < 소프트웨어 이므로 설정 많은 순은 역순이다.
</details>

<br>

**Q. 빅데이터 환경에서 데이터 분석 기술을 통해 분석된 데이터의 의미와 가치를 시각적으로 표현할 때
유용한 프로그래밍 언어이다.**
<details>
<summary>A.</summary>
R
<br>

* Hadoop : 대규모 데이터 세트를 분산 처리하기 위한 프레임워크
* NoSQL : RDBMS의 대안으로 웹 애플리케이션을 위한 DB 기술
* Cassandra : 고가용성, 확장성을 제공하는 분산 NoSQL DB 시스템
</details>

<br>

**Q. 빅데이터 환경에서 데이터 분석 기술을 통해 분석된 데이터의 의미와 가치를 시각적으로 표현할 때
유용한 프로그래밍 언어이다.**
<details>
<summary>A.</summary>
R
<br>

* Hadoop : 대규모 데이터 세트를 분산 처리하기 위한 프레임워크
* NoSQL : RDBMS의 대안으로 웹 애플리케이션을 위한 DB 기술
* Cassandra : 고가용성, 확장성을 제공하는 분산 NoSQL DB 시스템
</details>

<br>

**Q. 다음 중 CPU 반가상화를 지원하는 가상화 기술로 알맞은 것은?**
> 1. Xen
> 2. KVM
> 3. Docker
> 4. VirtualBox
<details>
<summary>A.</summary>
1. Xen 
<br>

* 반가상화(Partial Virtualization) : 하드웨어 가속을 사용하지 않고, OS가 가상화 환경을 인식하도록 고안된 방식
  * 게스트 OS가 직접 하이퍼바이저와 소통하며 성능을 최적화
* Xen : 반가상화와 전가상화를 전부 지원
* KVM, VirtualBox : 하드웨어 가속을 사용하는 전가상화 방식을 지원
* Docker : 컨테이너 가상화 개념으로, 가상머신이 아님
</details>

<br>

**Q. 다수의 웹 서버를 운영하는 환경으로 하나의 로드 밸런서 시스템으로 부하를 분산할 때 적합한 클러스터링 기술은?**
> 1. 고계산용 클러스터
> 2. 베어울프 클러스터
> 3. 고가용성 클러스터
> 4. HPC 클러스터
<details>
<summary>A.</summary>
3. 고가용성 : 시스템의 가용성을 높이기 위해 장애 발생 시 다른 서버가 서비스를 이어 받아 운영되는 클러스터.
로드 밸런싱을 통해 여러 서버로 트래픽을 분산시켜 서비스의 가용성을 보장한다.
<br>

* 고계산용 클러스터 = HPC (High Performance Calculation) 클러스터 : 과학적 계산 및 데이터 처리 등 높은 연산 능력을 요구할 때 수행하는 클러스터
* 베어울프 클러스터 : 저렴한 PC를 연결해 고성능 컴퓨팅을 만드는 기술
</details>

<br>

**Q. SYN Flooding 공격과 같은 네트워크 상태 정보를 확인하는 명령은?**
> 1. netstat
> 2. ip
> 3. arp
> 4. route
<details>
<summary>A.</summary>
1. netstat, netstat -an 명령어를 이용해 어느 포트로 어떤 IP가 접근을 시도하는지 확인 필요
<br>

* netstat -an | SYN
  * netstat: 네트워크 연결 상태 표시
  * -a : 모든 연결 상태 표시
  * -n : IP와 포트를 숫자로 표시 
* ip : ip 주소 설정, 네트워크 인터페이스 확인
* arp : ARP 테이블 확인 (MAC 주소 <-> IP 매핑) 
</details>

<br>

**Q. 다음 설명에 해당하는 파일 명은?**
> abc라고 입력하면 abc.co.kr 도메인이 자동으로 덧붙여지도록 특정 도메인을 등록 해 이름 호출 시 단축하려고 한다. <br>
> ex : abc 호출 시 abc.co.kr로 접속되도록 한다.
<details>
<summary>A.</summary>
/etc/resolv.conf
<br>

* /etc/resolv.conf는 DNS에 보낼 도메인 쿼리를 지정할 수 있다.
  * 네트워크를 통한 DNS 조회가 일어난다.
* /etc/hosts는 특정/도메인 및 호스트명을 강제적으로 특정 IP에 매핑한다.
  * 로컬 시스템에 적용되며, DNS보다 우선적으로 조회가 일어난다.
</details>

<br>

**Q. 네트워크 카드에 물리적으로 케이블이 연결되었는지 점검할 때 사용하는 명령어는?**
> abc라고 입력하면 abc.co.kr 도메인이 자동으로 덧붙여지도록 특정 도메인을 등록 해 이름 호출 시 단축하려고 한다. <br>
> ex : abc 호출 시 abc.co.kr로 접속되도록 한다.
<details>
<summary>A.</summary>
mii-tool
<br>

* mii-tool : 옵션 없이 실행 시 랜카드의 속도, 모드, 연결 상태 등을 보여주므로 네트워크 점검 시 사용
  * -v 옵션을 추가하면 보다 상세한 mii 상태를 확인 가능
* ifconfig : 네트워크 인터페이스의 설정 및 관리에 사용하는 명령어
* ss : 소켓 통계를 보여주는 명령어
* netstat : 네트워크 상태 관련 정보를 보여주는 명령어
</details>

<br>

**Q. 시스템에 설정된 게이트웨이 주소값을 확인하는 명령어로 틀린 것?**
> 1. ip
> 2. route
> 3. netstat
> 4. ethtool
<details>
<summary>A.</summary>
4. ethtool, ethtool은 네트워크 인터페이스 카드의 상태 및 속도를 확인하는 명령어이다.
<br>

* 게이트웨이 : 내부 네트워크와 외부 네트워크를 연결하는 장치 또는 경로
  * 내부 네트워크에서 거쳐가는 출입구 역할을 함
  * ex) ipconfig 시 Default Gateway . . . . . . : 192.168.0.1 로 출력
    * 이 경우 현재 내부 네트워크 -> 외부로 나가는 경로(게이트웨이 주소)는 192.168.0.1가 된다.
</details>

<br>

**Q. 시스템에 설정된 게이트웨이 주소값을 확인하는 명령어로 틀린 것?**
> 1. ip
> 2. route
> 3. netstat
> 4. ethtool
<details>
<summary>A.</summary>
4. ethtool, ethtool은 네트워크 인터페이스 카드의 상태 및 속도를 확인하는 명령어이다.
<br>

* 게이트웨이 : 내부 네트워크와 외부 네트워크를 연결하는 장치 또는 경로
  * 내부 네트워크에서 거쳐가는 출입구 역할을 함
  * ex) ipconfig 시 Default Gateway . . . . . . : 192.168.0.1 로 출력
    * 이 경우 현재 내부 네트워크 -> 외부로 나가는 경로(게이트웨이 주소)는 192.168.0.1가 된다.
</details>

<br>

**Q. 다음과 같은 경우 사용가능한 IP 주소의 개수로 알맞은 것은?**
> C 클래스 네트워크 주소 대역 1개를 할당받은 상태이고, <br>
> 여러 부서가 존재하는 관계로 서브넷 마스크 값은 255.255.255.192로 설정할 예정이다. <br>
> 또한 인터넷 사용 없이 내부 통신망용으로 구축할 예정이다.
<details>
<summary>A.</summary>
248
<br>

* [번외] 255.255.255.192의 CIDR 표기법 : /26 
  * 네트워크 주소를 나타내는 26비트 + 호스트 주소를 나타내는 6비트 이므로
    * 이진수 표현 : 11111111.11111111.11111111.11000000
* [풀이]
  * 255.255.255.192 이진수 변환 시 11111111.11111111.11111111.11000000
  * 32비트 중 호스트 비트 6개, 마지막 8비트를 통해 6비트는 호스트, 2비트는 서브넷 구분을 위해 사용됨을 의미
    * 따라서 각 서브넷별 가용한 IP 개수 : 2^6 - 2 (네트워크 주소, 브로드캐스트 주소 제외) = 62
      * 네트워크 주소 : 서브넷 마스크별 첫번째 주소
      * 브로드캐스트 주소 : 서브넷 마스크별 마지막 주소
    * 가용한 서브넷 마스크 : 2^2 : 4개
    * 따라서 가능한 IP 주소는 62 * 4로 248개이다.
</details>

<br>

**Q. IP 주소 및 서브넷 마스크값이 다음과 같을 때 설정되는 브로드캐스트 주소값은?**
> 192.168.5.189/26
<details>
<summary>A.</summary>
248
<br>

* [번외] 255.255.255.192의 CIDR 표기법 : /26
  * 네트워크 주소를 나타내는 26비트 + 호스트 주소를 나타내는 6비트 이므로
    * 이진수 표현 : 11111111.11111111.11111111.11000000
* [풀이]
  * 255.255.255.192 이진수 변환 시 11111111.11111111.11111111.11000000
  * 32비트 중 호스트 비트 6개, 마지막 8비트를 통해 6비트는 호스트, 2비트는 서브넷 구분을 위해 사용됨을 의미
    * 따라서 각 서브넷별 가용한 IP 개수 : 2^6 - 2 (네트워크 주소, 브로드캐스트 주소 제외) = 62
      * 네트워크 주소 : 서브넷 마스크별 첫번째 주소
      * 브로드캐스트 주소 : 서브넷 마스크별 마지막 주소
    * 가용한 서브넷 마스크 : 2^2 : 4개
    * 따라서 가능한 IP 주소는 62 * 4로 248개이다.
</details>

<br>

**Q. 다음 중 로컬 시스템에 있는 파일을 FTP 서버에 업로드하는 경우 사용하는 명령어로 알맞은 것은?**
<details>
<summary>A.</summary>
put

<br>

* FTP 서버에 파일을 업로드 하는 명령어 : put
* FTP 서버에 파일을 다운로드 하는 명령어 : get
</details>

<br>

**Q. 원격지의 IP 주소가 192.168.5.13번인 ssh 서버에 kaitman 계정으로 변경해서 접속하는 과정이다. 괄호 안에 들어갈 내용으로 알맞은 것은?**
> $ ssh ( )
<details>
<summary>A.</summary>
kaitman@192.168.5.13

<br>

* ssh [username] @ [hostname or IP 주소]
  * 옵션 -p 포트 설정
</details>

<br>

**Q. 원격지 텔넷 서버에 계정을 변경해 접속하는 과정이다. 괄호 안에 들어갈 옵션으로 알맞은 것은?**
> telnet ( ) kaitman 192.168.5.13
<details>
<summary>A.</summary>
-l

<br>

* -u : UDP 모드로 telnet을 실행
* -n : 호스트 이름을 숫자로 변환하지 않고, 주소로 처리
* -p : 원격 호스트의 포트 지정
* -l : 로그인할 때 사용할 사용자 이름을 지정
</details>

<br>

**Q. 메일 서버 간의 메시지 교환에 사용되는 프로토콜은?**
> telnet ( ) kaitman 192.168.5.13
<details>
<summary>A.</summary>
SMTP

<br>

* SMTP : 포트번호 25번 사용해 메시지 교환(이메일)에 사용되는 
* SNMP : 네트워크 기기의 네트워크 정보를 네트워크 관리 시스템에 보내는데 사용되는 프로토콜
* IMAP : 원격 서버로부터 이메일을 가져오는데 사용되는 프로토콜
* POP3 : 응용 계층 인터넷 프로토콜 중 하나로, TCP/IP 연결을 통해 이메일을 가져오는데 사용
</details>

<br>

**Q. 다음 중 리눅스를 비롯한 유닉스 계열 운영체제와 윈도우 운영체제 간 자료 및 하드웨어 공유를 지원하는 서비스는?**
> 1. NFS
> 2. SAMBA
> 3. Gopher
> 4. FTP
<details>
<summary>A.</summary>
NFS
<br>

* NFS : 네트워크를 통해 파일을 공유할 수 있게 해주는 시스템, 주로 리눅스나 유닉스 시스템끼리 파일 공유시 사용
* SAMBA : 윈도우 파일 공유 프로토콜을 구현, 파일 혹은 프린터를 공유
* Gopher : 인터넷 초기에 사용된 정보 검색 프로토콜임.
* FTP : 컴퓨터와 컴퓨터 사이 파일 전송 프로토콜
</details>

<br>

**Q. 다음 중 리눅스를 비롯한 유닉스 계열 운영체제와 윈도우 운영체제 간 자료 및 하드웨어 공유를 지원하는 서비스는?**
> 1. NFS
> 2. SAMBA
> 3. Gopher
> 4. FTP
<details>
<summary>A.</summary>
NFS
<br>

* NFS : 네트워크를 통해 파일을 공유할 수 있게 해주는 시스템, 주로 리눅스나 유닉스 시스템끼리 파일 공유시 사용
* SAMBA : 윈도우 파일 공유 프로토콜을 구현, 파일 혹은 프린터를 공유
* Gopher : 인터넷 초기에 사용된 정보 검색 프로토콜임.
* FTP : 컴퓨터와 컴퓨터 사이 파일 전송 프로토콜
</details>

<br>

**Q. 다음 중 X 윈도가 설치되지 않은 환경의 콘솔 창에서 사용할 수 있는 웹 브라우저로 알맞은 것은?**
> 1. links
> 2. firefox
> 3. opera
> 4. safari
<details>
<summary>A.</summary>
1. Links
<br>

* links, Lynx는 텍스트 기반의 웹 브라우저이다.
* 나머지는 GUI를 사용한다. (X 윈도우 기반)
</details>

<br>

**Q. 다음 설명에 해당하는 국제기구는?**
> IP 주소, 인터넷 도메인 이름, 프로토콜의 범주 및 포트 할당 등 업무를 담당함.
<details>
<summary>A.</summary>
ICANN
<br>

* ICANN (Internet Corporation of Assigned Names and Numbers) : 인터넷의 도메인 이름과 IP 주소 할당을 관리함
  * 전 세계적으로 인터넷 주소의 안정적 운영을 책임짐.
</details>

<br>

**Q. 다음 설명에 해당하는 국제기구는?**
> IP 주소, 인터넷 도메인 이름, 프로토콜의 범주 및 포트 할당 등 업무를 담당함.
<details>
<summary>A.</summary>
ICANN
<br>

* ICANN (Internet Corporation of Assigned Names and Numbers) : 인터넷의 도메인 이름과 IP 주소 할당을 관리함
  * 전 세계적으로 인터넷 주소의 안정적 운영을 책임짐.
</details>

<br>

**Q. IPv4의 C클래스 주소 대역폭은?**
> IP 주소, 인터넷 도메인 이름, 프로토콜의 범주 및 포트 할당 등 업무를 담당함.
<details>
<summary>A.</summary>
192.0.0.0 ~ 223.255.255.255
<br>

* A : 0.0.0.0 ~ 127.255.255.255
* B : 128.0.0.0 ~ 191.255.255.255
* C : 192.0.0.0 ~ 223.255.255.255
* 앞자리 64씩 증가한다.
</details>