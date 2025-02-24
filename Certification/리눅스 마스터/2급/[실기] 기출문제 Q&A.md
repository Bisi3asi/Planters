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

