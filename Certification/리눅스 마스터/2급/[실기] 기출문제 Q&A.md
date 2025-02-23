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
