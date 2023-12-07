# Linux 심화
1. 네트워크 성능 저하가 발생하여 netstat -s 명령어를 수행한 결과 아래와 같이 패킷 손실이 발생하는 상황이다.
    ```bash
    $netstat -s
    Tcp:
        ...
         4056591 segments retransmited
         1057 bad segments received
         31228471 resets sent
    ```
    - ethtool -S eth0 | grep -i errors 명령어를 통해 NIC에서 패킷을 처리하지 못하고 Drop 시키는 현상이 발생하였음을 확인하였습니다.
        ```bash
        $ethtool -S eth0 | grep -i errors
        ...
        rx_missed_erros: 342
        ...
        ```
        - 해당 현상에 대해 올바른 조치를 취해주세요.
2. 다음은 df -h 명령어를 입력했을 때 나올 수 있는 파일 시스템 이름입니다. 각 파일 시스템별 특징을 설명해주세요.
    - /dev/sda
    - /dev/hda
    - /dev/vda
3. Memory Commit과 Overcommit이란 무엇인가요?
    - 현재 시스템의 Memory Commit 비율을 어떻게 확인할 수 있을까요?
    - Memory Commit 옵션 값으로는 어떤 값들이 있을까요? 또한 어떻게 변경할 수 있나요?
4. 부하가 발생하여 top 또는 uptime 명령어를 수행한 결과로 Load Average 값이 지속적으로 증가하는 상황이라 가정하겠습니다.
    -  상태(실행 또는 I/O 완료 대기)별로 부하가 발생한 프로세스 개수를 확인할 수 있는 방법이 있을까요?
5. free 명령어 출력 결과 중 buff/cache는 각각 무슨 영역인지 설명해주세요.