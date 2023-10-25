# Linux 기초
1. AWS Free-tier 계정으로 EC2 Amazon Linux2 인스턴스(t2a.micro)를 생성하고 ssh로 접속해보세요.
    - 위 sshd 접속 과정을 설명해주세요.
        1. 클라이언트 : ssh-keygen > Key Pair(~/.ssh/{id_rsa, id_rsa_pub}) 생성
        2. ssh-copy-id > 서버로 공개키 전달 또는 ~/.ssh/authorized_keys) 직접 등록
        3. ssh [user]@[ip] > 공개키와 함께 인증 요청 전달
        4. 서버 : 전달 받은 공개키 > authorized_keys 등록 확인
        5. 난수와 해시 값을 생성한다.
        6. 공개키 > 난수 암호화 후 전달
        7. 클라이언트 : 개인키 > 전달 받은 난수 복호화 후 결과 값으로 해시 값 생성 및 전달
        8. 서버 : 자신의 해시 값과 비교, 일치하면 인증 성공
    - 퍼블릭 키는 어디에 있나요?
        - 클라이언트 > ~/.ssh/id_rsa.pub
        - 서버 > ~/.ssh/authorized_keys
    - 리눅스 내부에서 접속 포트 번호를 22에서 2022로 변경하려면 어떻게 할까요?
        ```bash
        # 보안그룹 또는 방화벽에서 2022 포트 개방 선행
        # 주석 제거 및 포트 번호 변경
        $vi /etc/ssh/sshd_config
        ...
        #Port 22 > Port 2022

        $systemctl restart sshd
        ```
2. 현재 사용 중인 리눅스의 파일 시스템을 조회하는 명령어를 입력하고 결과를 작성해주세요.
    - df -T 또는 mount | grep ^/dev
3. 최상위 루트 디렉토리('/')의 하위 디렉토리를 간략하게 설명해주세요.
    - /bin : Binary, 주 명령어(cat, ls, cp등)의 실행 파일
    - /dev : Device, 물리적으로 연결된 하드웨어를 다루기 위한 파일
    - /etc : 리눅스와 애플리케이션 환경 설정 파일
    - /lib : Library, 시스템 부팅 및 관리에 필요한 공유 라이브러리와 커널 모듈 저장
        - 커널 모듈 : 커널의 기능을 확장하거나 추가하기 위해 사용하는 모듈
        - 공유 라이브러리 : 여러 프로그램이 공통으로 사용하는 코드와 데이터 모음
    - /mnt: Mount, 외부 저장장치와 같은 임시 파일 시스템의 마운트 포인트 수동 관리
    - /media : 외부 저장장치가 연결될 때 자동으로 장치를 인식하고 마운트 포인트를 생성하는 디렉토리
    - /proc: Process, 현재 실행 중인 프로세스 정보
    - /usr: User, 일반 사용자가 설치한 애플리케이션 관련 파일
    - /var : 애플리케이션 실행 중에 만들어진 데이터 및 로그 저장
4. 현재 사용 중인 쉘과 사용 가능한 쉘의 목록을 확인하는 명령어를 각각 입력해주세요.
    - grep [user] /etc/passwd, cat /etc/shells
    - 쉘이란 무엇일까요?
        - 사용자와 OS간 명령어 인터페이스
        - 입력(stdin)받은 명령어를 커널이 이해할 수 있도록 해석 및 전달하며 적절한 시스템 콜을 호출하고 결과 값을 반환(stdout)한다.
5. ls 명령어를 입력하면 현재 디렉토리 내 파일과 디렉토리의 목록을 반환합니다. 해당 명령어의 입력부터 출력까지의 과정을 설명해주세요.
    - 작성 키워드 : **fork & exec(system call)**
        1. Shell에 ls 명령어를 입력한다(stdin)
        2. 명령어를 해석하고 이를 실행하기 위해 필요한 시스템 콜을 호출한다.
        3. fork 시스템 콜을 통해 자식 프로세스를 생성한다.
        4. exec 시스템 콜을 통해 자식 프로세스에 ls 프로그램을 로드한다.
        5. 프로그램을 실행하고 결과를 stdout으로 반환하며 터미널에 출력한다.
    - system call이란 무엇일까요?
        - User mode의 프로세스가 Kernel mode의 자원에 접근하기 위한 인터페이스
6. ps -ef | grep "/bin/sh" 명령어를 입력해보시고, 파이프라인('**|**') 문자가 어떤 기능인지 설명해주세요.
    - 파이프라인으로 연결된 명령어는 앞 명령어의 출력(stdout)을 받기 위해 대기하고 받은 결과를 stdin으로 입력 받아 동작합니다.
    - 마찬가지로 ls | sort | cd /home/ec2-user 명령어를 입력하고, 어떤 내용이 출력되는지 작성해주세요.
        - 만약 출력되지 않는다면 그 이유를 설명해주세요.
            - ls 출력(stdout)이 자식 프로세스(cd)로 넘어가므로 부모 프로세스(Shell)는 출력을 받을 수 없다.
7. pstree 명령어를 입력해주세요.
    - 최상단 프로세스(init 또는 systemd)의 역할은 무엇인지 설명해주세요.
        - 부팅 시 최초 실행 프로세스(PID : 1)
        - 이 프로세스를 fork하여 부팅 시 필요한 모든 프로세스를 실행한다.
    - init과 systemd의 차이점을 설명해주세요.
        - init > /etc/rd.d 하위 연결한 스크립트와 서비스를 순차적으로 실행 > 호위 효과 발생
        - systemd > 의존성을 만족하는 서비스는 병렬로 실행하고 의존성이 있는 서비스는 순차적으로 실행한다.
            - 의존 관계 : 서비스 A > 서비스 B, 서비스 C > 서비스 D인 경우 : 서비스 B, D 병렬 실행 후 서비스 A, C 순차 실행
8. 네이버(www.naver.com)의 IP 주소를 확인하는 명령어는 무엇인가요?
    - 실행 결과(IP 주소)를 받는 과정을 설명해주세요(DNS 질의 과정)
        1. Local DNS Cache > 이전 질의 결과 확인
        2. /etc/hosts > 호스트 네임과 IP 주소 매핑 정보 확인
        3. /etc/resolv.conf > 지정된 DNS 서버에 질의 전달
            - Root : .com(TLD) 서버 정보 전달
            - TLD : naver.com(SLD) 서버 정보 전달
            - SLD : IP 주소 정보 전달
9. curl, telnet, ping 차이점을 설명해주세요.
    - curl : Application(HTTP 등) Layer 동작
    - telnet : Transport(TCP) Layer 동작
        - 데이터를 평문으로 송/수신하므로 패킷 스니핑 위험이 있다.
    - ping : Network Layer(ICMP) 동작 - 포트 사용 X
10. yum 패키지를 통해 httpd를 설치하고, 80 포트를 개방하여 서비스를 실행해주세요.
    - yum과 apt의 차이점은 무엇인가요?
        - yum : RedHat, .rpm 확장자
        - apt : Debian, .deb 확장자
    - httpd 포트 상태(LISTEN…)를 확인하는 명령어를 입력하고 결과를 작성해주세요.
        ```bash
        $yum install net-tools
        $apt-get install net-tools
        $netstat -tnp | grep "httpd"
        ```
11. 다음은 리소스 모니터링을 수행하는 명령어 중 top 명령어를 수행한 결과입니다. 각각의 값이 무엇을 의미하는지 설명해주세요.
    ```bash
    $top
    top - 22:39:40 up 0 min,  0 users,  load average: 0.00, 0.00, 0.00
    Tasks:   5 total,   1 running,   4 sleeping,   0 stopped,   0 zombie
    %Cpu(s):  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
    MiB Mem :   6948.5 total,   6844.0 free,     75.0 used,     29.5 buff/cache
    MiB Swap:      0.0 total,      0.0 free,      0.0 used.   6733.4 avail Mem

    PID USER     PR  NI    VIRT    RES    SHR  S  %CPU  %MEM     TIME+  COMMAND
    1   root     20   0    1276    788    472  S   0.0   0.0   0:00.03  init
    14  root     20   0    1284    388     20  S   0.0   0.0   0:00.00  init
    15  root     20   0    1284    396     20  S   0.0   0.0   0:00.00  init
    16  eljoelee 20   0    6200   5028   3316  S   0.0   0.1   0:00.02  bash
    27  eljoelee 20   0    7788   3268   2904  R   0.0   0.0   0:00.00  top
    ```
    - load average : CPU 자원을 사용하고 있는 실행 중인 프로세스 수 + I/O 작업 완료 대기 프로세스의 평균 수를 1/5/15분 단위로 나타낸 값
    - tasks : 프로세스 개수
        - sleeping : I/O 작업 완료 대기 프로세스 수
        - zombie : 부모 프로세스가 종료된 자식 프로세스 수
    - Cpu(s) : cpu 사용률
        - hi : 하드웨어 인터럽트(비동기 인터럽트, I/O 인터럽트)
        - si : 소프트웨어 인터럽트(동기 인터럽트, Exception, CPU가 명령어 수행 중 발생한 예외 또는 System Call 등)
    - MiB : 메모리 사용률
        - buff/cache : bufferd I/O 메모리 + 캐시 메모리 사용률
    - PR : CPU 스케줄링 우선순위
    - VIRT : 프로세스가 사용하는 가상 메모리 양(code, data, heap, stack)
    - RES : 프로세스가 사용하는 non-swapped 물리 메모리(RAM) 양
        - non-swapped Physical Memory : Swap-out되지 않고 RAM에 남아있는 메모리
    - SHR : 다른 프로세스와 공유하는 공유 메모리 양
    - MEM : 물리 메모리(RAM)에서 RES가 차지하는 비율
12. 메모리 사용량을 확인하는 명령어를 입력하고 결과를 작성해주세요.
    - swap이란 항목은 무엇을 의미하는 걸까요?
        - 하드 디스크의 일부를 RAM처럼 사용하는 가상 메모리의 일부 영역
        - 2GB 가량의 swap 메모리를 설정하고 메모리 사용량을 확인하는 명령어를 통해 결과를 작성해주세요.
            ```bash
            # fallocate 또는 dd 명령어로 파일 생성 가능
            $fallocate -l 2GB /swapfile

            # bs(블록 크기): 128M * count(블록 개수): 16 = 2GB
            $dd if=/dev/zero of=/swapfile bs=128M count=16

            # 읽기/쓰기 권한 부여
            $chmod 600 /swapfile

            # swap 영역 생성
            $mkswap /swapfile

            # swap 영역 활성화
            $swapon /swapfile

            # 부팅 시 자동 활성화
            $vi /etc/fstab
            /swapfile swap swap defaults 0 0

            # 영역 확인
            $free -h

            # swap 영역 비활성화
            $swapoff /swapfile

            # swap 파일 삭제
            $rm -rf /swafile
            ```