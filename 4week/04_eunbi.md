### 운영체제
#### 3.1 운영체제와 컴퓨터
- 운영체제는 사용자가 컴퓨터를 쉽게 다루게 해주는 인터페이스다.
- 한정된 메모리나 시스템 자원을 효율적으로 분배한다

##### 3.1.1 운영체제의 역할과 구조
- 운영체제의 역할
	- CPU 스케줄링과 프로세스 관리: CPU 소유권을 어떤 프로세스에 할당할지, 프로세스의 생성과 삭제, 자원 할당 및 반환 관리
	- 메모리 관리: 한정된 메모리를 어떤 프로세스에 얼만큼 할당할지 관리
	- 디스크 관리: 디스크 파일을 어떠한 방법으로 보관할지 관리
	- I/O 디바이스 관리: 마우스, 키보드와 컴퓨터 간에 데이터 주고받는 것을 관리
- 운영체제의 구조
	- GUI, 시스템콜, 커널, 드라이버
	- GUI: 사용자가 전자장치와 상호 작용할 수 있도록 하는 사용자 인터페이스의 형태
	- 드라이버: 하드웨어를 제어하기 위한 소프트웨어
	- CUI: 그래픽이 아닌 명령어로 처리하는 인터페이스
- 시스템콜
	- 운영체제가 커널에 접근하기 위한 인터페이스. 유저 프로그램이 운영체제의 서비스를 받기 위해 커널 함수를 호출할 때 씀. 컴퓨터 자원에 대한 직접 접근을 차단하고 프로그램을 다른 프로그램으로부터 보호할 수 있음
	- 추상화 계층이기 때문에 네트워크 통신이나 데이터베이스와 같은 낮은 단계의 영역 처리에 대한 부분을 크게 신경쓰지 않고 프로그램 구현 가능
	- modebit: 시스템콜이 작동될 때 modebit을 참고하여 유저모드와 커널모드 구분. 0은 커널모드, 1은 유저모드로 설정. 유저 모드일 경우 시스템콜을 못하게 막아서 한정된 일만 하게 함

##### 3.1.2 컴퓨터의 요소
- CPU, DMA 컨트롤러, 메모리, 타이머, 디바이스 컨트롤러 등으로 이루어졌다.

- CPU: 산술논리연산장치, 제어장치, 레지스터로 구성되어 있는 컴퓨터 장치. 인터럽트에 의해 메모리에 존재하는 명령어를 해석해서 실행
	- 제어장치: 프로세스 조작을 지시하는 CPU의 한 부품. 입출력장치 간 통신 제어, 명령어를 읽고 해석, 데이터 처리를 위한 순서 결정
	- 레지스터: CPU 안에 있는 빠른 임시기억장치. CPU는 자체적으로 데이터를 저장할 수 없어서 레지스터를 거쳐 데이터를 전달
	- 산술논리연산장치: 덧셈, 뺄셈 같은 두 숫자의 산술 연산과 배타적 논리합, 논리곱 같은 논리 연산을 계산하는 디지털 회로
	- 인터럽트: 어떤 신호가 들어왔을 때 CPU를 잠깐 정지시키는 것
		- 하드웨어 인터럽트: IO 디바이스에서 발생하는 인터럽트
		- 소프트웨어 인터럽트: 트랩이라고도 함. 프로세스가 시스템콜을 호출할 때 발동
- DMA 컨트롤러
	- I/O 디바이스가 메모리에 직접 접근할 수 있도록 하는 하드웨어 장치
	- CPU 부하를 막아주며 하나의 작업을 CPU와 DMA 컨트롤러라 동시에 하는 것을 방지
- 메모리
	- 전자회로에서 데이터나 상태, 명령어 등을 기록하는 장치
	- CPU는 계산을 담당, 메모리는 기억을 담당
- 타이머
	- 몇 초 안에는 작업이 끝나야 한다는 것을 정하고 특정 프로그램에 시간 제한을 다는 역할
- 디바이스 컨트롤러
	- 컴퓨터와 연결되어있는 IO 디바이스들의 작은 CPU

#### 3.2 메모리
##### 3.2.1 메모리 계층
- 메모리 계층은 레지스터, 캐시, 메모리, 저장장치로 구성
- 레지스터: CPU 안에 있는 작은 메모리, 휘발성, 속도 가장 빠름, 기억 용량 가장 적음
- 캐시: L1, L2 캐시를 지칭. 휘발성, 속도 빠름, 기억 용량이 적음
- 주기억장치: RAM. 휘발성, 속도 보통, 기억 용량 보통
- 보조기억장치: HDD, SDD. 휘발성, 속도 낮음, 기억 용량 많음

- 캐시:
	- 데이터를 미리 복사해 놓음 임시 저장소, 빠른 장치와 느린 장치에서 속도 차이에 따른 병목 현상을 줄이기 위한 메모리
	- 지역성의 원리: 캐싱을 직접 설정할 때 자주 사용하는 데이터에 대한 근거가 되는 것.
	- 시간 지역성: 최근 사용한 데이터에 다시 접근하려는 특성
	- 공간지역성: 최근 접근한 데이터를 이루고 있는 공간이나 그 가까운 공간에 접근하는 특성
	- 캐시히트와 캐시미스
		- 캐시에서 원하는 데이터를 찾으면 캐시히트, 없으면 주메모리로 가서 데이터를 찾아오는 것은 캐시미스.
		- 캐시히트는 데이터를 제어창치를 거쳐 가져오게 되어 빠름
		- 캐시미스는 메모리에서 가져옴. 시스템 버스 기반으로 작동하여 느림
	- 캐시매핑
		- 캐시가 히트되기 위해 매핑하는 방법
		- 직접 매핑: 처리가 빠르지만 충돌 발생이 잦음
		- 연관 매핑: 순서 일치 없이 관련 있는 캐시와 메모리 매핑. 충돌은 적지만 모든 블록을 탐색해야해서 속도가 느림
		- 집합 연관 매핑: 직접 매핑과 연관 매핑을 합쳐 놓은 것
	- 웹 브라우저의 캐시
		- 쿠키, 로컬 스토리지, 세션 스토리지가 있음. 사용자의 커스텀 정보다 일증 모듈 관련 사항들을 웹브라우저에 저장하여 추후 사용
		- 쿠키: 만료기한이 있는 키-값 저장소. 보통 서버에서 만료기한을 정함
		- 로컬 스토리지: 만료기한이 없는 키-값 저장소. 웹브라우저를 닫아도 유지된다. 
		- 세션 스토리지: 만료기한이 없는 키-값 저장소. 클라이언트에서만 수정 가능
##### 3.2.2 메모리 관리
- 가상 메모리
	- 메모리 관리 기법의 하나로 컴퓨터가 실제로 이용 가능한 메모리 자원을 추상화하여 사용자에게 매우 큰 메모리로 보이게 만드는 것
	- 이때 가상으로 주어진 주소는 가상주소, 실제 메모리상에 있는 주소는 실제 주소
	- 가상주소는 메모리 관리 장치에 의해 실제 주소로 변환
	- 스와핑
		- 당장 사용하지 않는 영역을 하드디스크로 옮겨 필요할 때 다시 RAM으로 올리는 것을 반복하여 RAM을 효과적으로 관리하는 것
	- 페이지 폴트
		- 프로세스의 주소 공간에는 존재하지만 지금 컴퓨터의 RAM에는 없는 데이터에 접근했을 경우 발생
	- 스레싱
		- 메모리의 페지이 폴트율이 높은 것. 컴퓨터의 성능 저하 초래함
		- 해결하기 위해서는 메모리를 늘리거나 HDD를 SDD로 바꾸는 것, 운영체제에서는 작업 세트와 PFF
		- 작업세트: 프로세스의 과거 사용 이력인 지역성을 통해 결정된 페이지 집합을 만들어 미리 메모리에 로드하는 것
		- PFF: 페이지 폴트 빈도는 조절하는 것
	- 메모리 할당
		- 메모리에 프로그램을 할당할 때는 시작 메모리 위치, 메모리 할당 크기를 기반으로 할당
		- 연속 할당: 메모리에 연속적으로 공간을 할당
			- 고정 분할 방식: 메모리를 미리 나누어 관리. 융통성 없음, 내부 단편화 발생
			- 가변 분할 방식: 매시점 프로그램의 크기에 맞게 동적 메모리를 나눠 사용. 외부 단편화는 발생할 수 있음. 최초 적합, 최적 적합, 최악 적합이 있음
		- 불연속 할당: 메모리를 연속적으로 할당하지 않음
			- 페이징: 동일한 크기의 페이지 단위로 나누어 프로세스를 할당. 홀의 크기가 균일하지 않은 문제는 없어지지만 주소 변환 복잡해짐
			- 세그멘테이션: 의미 단위인 세그먼트로 나누는 방식. 공유와 보안 측면에서는 좋지만 홀 크기가 균일하지 않음
			- 페이지드 세그멘테이션: 공유나 보안을 세그먼트로 나누고 물리적 메모리는 페이지로 나누는 것
	- 페이지 교체 알고리즘
		- 스와핑이 많이 일어나지 않도록 설계되어야 함. 페이지 교체 알고리즘 기반으로 스와핑이 일어남
		- 오프라인 알고리즘: 먼 미래에 참조되는 페이지와 현재 할당하는 페이지를 바꾸는 알고리즘. 미래에 사용되는 프로세스를 알 수 없기에 사용할 수 없지만 다른 알고리즘과의 성능 비교에 대한 기준 제공
		- FIFO: 가장 먼저 온 페이지를 교체 영역에 가장 먼저 놓는 방법
		- LRU: 참조가 가장 오래된 페이지를 바꿈. 오래된 것을 파악하기 위해 페이지마다 계수기, 스택을 두어야 함. 보통 해시 테이블과 이중 연결 리스트로 구현
		- NUR: LRU에서 발전한 알고리즘. clock 알고리즘. 시계 방향으로 돌면서 최근에 참조되지 않은 프로세스를 찾아서 교체
		- LFU: 가장 참조 횟수가 적은 페이지를 교체