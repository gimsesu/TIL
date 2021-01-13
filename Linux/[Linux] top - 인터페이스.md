# [Linux] top - 인터페이스

## top

**t**able **o**f **p**rocesses의 약자로 Unix 기반 운영체제에서 주로 사용하는 작업 관리자 프로그램이다. CPU 및 메모리, 프로세스의 현재 사용 정보를 제공한다. Windows의 작업관리자와 유사한 역할을 한다고 보면 된다.

```shell
$ top 
## `q` 또는 `CTRL + c` 키를 누르면 종료된다.
```

### 기능

- top은 사용자가 지정한 기준에 따라 실행 중인 프로세스의 목록을 생성하고 주기적으로 업데이트한다. (기본적으로 3초마다 새로 고침)

- 기본적으로 CPU 사용량을 기준으로 표시하며, 상위 CPU 점유 프로세스만 표시된다.

- 현재 Linux 커널에서 관리 중인 프로세스/스레드 목록뿐만 아니라 시스템 요약 정보를 표시할 수 있다. 

- 표시되는 정보의 유형과 순서 및 크기는 사용자가 재구성할 수 있다.

---

</br>

## 목표

top에는 다양한 사용자 정의 방법과 기능들이 존재하지만, 이 글에서는 top을 실행시켰을 때 나타나는 기본 인터페이스의 요소에 대해서만 살펴본다.

![top_1](https://user-images.githubusercontent.com/51101183/104422201-c65c8a00-55bf-11eb-9e7f-e18d0ee059d1.png)



## 1) 요약 정보

### UPTIME and LOAD Averages

단일 행으로 구성된다.

```shell
top - 14:46:30 up 281 days,  3:13,  2 users,  load average: 6.79, 5.80, 3.96
```

| 항목                             | 내용                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| top                              | 프로그램 또는 창 이름 (디스플레이 모드에 따라 다름)          |
| `4:46:30 up 281 days,  3:13`     | 현재 시간 및 마지막 부팅 시간으로부터 지난 시간 및 부팅 시간 |
| users                            | 총 사용자 수                                                 |
| `load average: 6.79, 5.80, 3.96` | 지난 1, 5, 15 분 동안의 평균 시스템 로드                     |



### TASK and CPU States

이 부분은 최소 2행으로 구성된다. 대화형 명령 `H`를 이용하여 스레드-모드 상태로 변경할 수 있다.

```shell
# CPU 모드
Tasks: 411 total,   1 running, 410 sleeping,   0 stopped,   0 zombie 
%Cpu(s): 34.3 us,  0.8 sy,  0.0 ni, 64.0 id,  0.7 wa,  0.0 hi,  0.1 si,  0.0 st
```

```shell
# 스레드 모드
Threads: 1512 total,   2 running, 1510 sleeping,   0 stopped,   0 zombie 
%Cpu(s):  0.6 us,  0.5 sy,  0.0 ni, 98.8 id,  0.1 wa,  0.0 hi,  0.1 si,  0.0 st 
```

</br>

#### $ Line 1

```shell
Tasks: 411 total,   1 running, 410 sleeping,   0 stopped,   0 zombie 
```

| 항목     | 내용                                                         |
| -------- | ------------------------------------------------------------ |
| Tasks    | 프로세스 또는 스레드의 총 개수를 표시한다. 이 수치는 아래의 항목으로 다시 분류된다. |
| running  | 실행 중이거나( *Running* ) 실행 가능한( *Runnable* ) 상태    |
| sleeping | 프로세스가 CPU 자원을 기다리고 있는 상태                     |
| stopped  | CPU 자원을 할당 받을 수 없는 상태 (중단)                     |
| zombie   | 프로세스의 수행이 끝났음에도 자원을 반환받지 못한 상태       |

💡**zombie**: 커널은 프로세스를 추적하기 위해 메모리에 다양한 데이터 구조를 유지한다. 프로세스는 여러 자식 프로세스를 생성할 수 있으며 부모가 아직 있는 동안 종료될 수 있다. 그러나 이러한 데이터 구조는 부모가 자식 프로세스의 상태를 얻을 때까지 유지되어야 한다. 데이터 구조가 여전히 존재하는 종료된 프로세스를 **좀비**라고 한다.

</br>

#### $ Line 2

```shell
%Cpu(s): 34.3 us,  0.8 sy,  0.0 ni, 64.0 id,  0.7 wa,  0.0 hi,  0.1 si,  0.0 st
```

마지막 새로 고침 이후 간격을 기반으로 CPU 상태를 백분율로 보여준다.</br>

기본적으로 개별 범주에 대한 백분율이 표시된다. 두 개의 레이블이 있는 경우, 최신 커널 버전은 전자로 표시된다.

| 항목        | 내용                                                         |
| ----------- | ------------------------------------------------------------ |
| us, user    | un-niced 사용자 프로세스 실행 시간                           |
| sy, system  | 커널 프로세스 실행 시간                                      |
| ni, nice    | niced 사용자 프로세스 실행 시간                              |
| id, idle    | 커널 유휴 핸들러에서 보낸 시간                               |
| wa, IO-wait | I/O 완료를 기다리는 시간                                     |
| hi          | 하드웨어 인터럽트 서비스에(바로 실행된 인터럽트 핸들러에서) 든 시간 |
| si          | 소프트웨어 인터럽트 서비스에(대기 후 실행된 인터럽트 핸들러에서) 든 시간 |
| st          | *Steal Time*. 하이퍼바이저의 가상 프로세서( *vm* )에 의해 도난당한 시간 |

💡**Steal Time**: 가상 환경에서 동작하는 vm(virtual machine)은 단일 호스트에 있는 다른 인스턴스와 리소스를 공유한다. CPU Steal Time을 통해 vm에서 동작하는 CPU가 물리 머신으로부터 자원을 할당받기 위해 얼마나 대기하고 있는지를 알 수 있다.

</br>

대화형 명령 `t`로 CPU 상태 표시 모드를 변경할 수 있다.

```shell
            a   b      c  d
%Cpu(s):  27.8/0.7    28[|||||||||||||||||||||||||||||                                                          ]
```

- a : `us`와 `ni`의 비율의 합
- b : `sy`의 백분율
- c : 총 합계
- d : 그래프

</br>

### MEMORY Usage

2행으로 구성된 메모리 사용 정보는 대화형 명령 `E`를 통해 표시되는 사용량의 단위를 KiB에서 EiB까지 표현할 수 있다.

```shell
KiB Mem : 13192420+total, 25071360 free, 16886676 used, 89966176 buff/cache 
KiB Swap:  1003516 total,   454356 free,   549160 used. 11294108+avail Mem
```

```shell
GiB Mem :  125.813 total,   24.217 free,   15.771 used,   85.825 buff/cache 
GiB Swap:    0.957 total,    0.433 free,    0.524 used.  108.047 avail Mem
```

</br>

Mem과 Swap 줄은 각각 RAM과 스왑 공간에 대한 정보를 표시한다.</br>

Swap 공간은 RAM처럼 사용되는 하드 디스크의 일부다. RAM 사용량이 가득 차면 RAM에서 자주 사용되지 않는 영역이 스왑 공간에 기록되어 나중에 필요할 때 검색할 수 있다. 그러나 디스크 액세스가 느리기 때문에 스와핑에 너무 의존하면 시스템 성능이 저하될 수 있다.

- total, free, used, buff/cache : RAM 공간에 대한 정보 표시
- avail Mem : 스왑 없이 새 애플리케이션을 시작하는 데 사용할 수 있는 실제 메모리의 추정치

</br>

대화형 명령 `m`으로 메모리 표시 모드를 변경할 수 있다.

```shell
            a      b     c
KiB Mem : 14.0/13192420+[||||||||||||||                                                                        ]
KiB Swap: 54.7/1003516  [|||||||||||||||||||||||||||||||||||||||||||||||||||||||                               ]
```

- a : 사용된 비율

- b : 사용 가능한 양
- c : 그래프

</br>

## 2) - 3) 헤더 및 프로그램 영역

사용 가능한 프로세스의 각 정보 필드를 나타낸다. 대화형 명령 `f` 또는 `F`를 통해 각 필드의 표시 여부를 설정할 수 있다. 또한 특정 필드를 기준으로 정렬할 수도 있다.

```shell
   PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND 
156744 root      20   0 1612648  28132   7772 S  10.6  0.0  11:40.95 ucid_server 
157738 root      20   0   25504   3652   2564 R   1.0  0.0   0:00.28 top 
156170 root      20   0  105976   7356   6228 S   0.7  0.0   0:45.99 sshd
```

물리 메모리 또는 가상 메모리와 관련된 필드는 기본적으로 KiB 단위로 표시한다. 대화형 명령 `e`를 통해 KiB부터 PiB까지 표시 단위를 변경할 수 있다.

</br>

#### 헤더

| 항목    | 내용                                                         |
| ------- | ------------------------------------------------------------ |
| PID     | Process ID. 프로세스를 식별하는 고유한 양의 정수             |
| USER    | 작업 소유자의 유효한 사용자 이름                             |
| PR      | Priority. 작업의 스케줄링 우선순위. 특정 프로세스에서 이 필드에 `rt`가 표시돼 있다면, 실시간 스케줄링 우선순위 정책 아래에서 실행되고 있다는 뜻이다. |
| NI      | 프로세스의  *nice* 값을 보여준다. 값이 음수면 우선순위가 높고, 양수면 우선순위가 낮다. `0`값은 프로세스의 *dispatch-ability* 가능성을 결정할 때, 우선순위가 조정되지 않음을 의미한다. |
| VIRT    | Virtual Memory Size. 프로세스에 사용된 가상 메모리의 총량. 이 값에는 프로그램의 코드, 프로세스가 메모리에 저장한 데이터, 디스크로 Swap된 메모리 영역이 포함된다. |
| RES     | Resident Memory Size. 프로세스가 사용하고 있는 Swap 되지 않은 물리 메모리 |
| SHR     | Shared Memory Size. 다른 프로세스와 잠재적으로 공유될 수 있는 메모리를 반영한다. |
| S       | Process Status. 프로세스의 상태를 나타낸다. 아래의 값 중 하나를 갖는다.<br />D = uninterruptible sleep. I/O 작업이 완료되기를 기다리는 중<br />R = running. CPU에서 실행 중이거나 실행할 준비가 됨<br />S = sleeping or interruptible sleep. 이벤트가 완료되길 기다리는 중<br />T = stopped by job control signal. 작업 제어 신호(`Ctrl + Z`처럼)에 의해 중지됨<br />t = stopped by debugger during trace. 추적 중인 디버거에 의해 중지됨<br />Z = zombie. 좀비 프로세스 |
| $CPU    | CPU Time. 마지막 새로 고침 이후 지난 CPU 시간 동안의 프로세스 점유율. 총 CPU 시간의 백분율로 표시된다. |
| %MEM    | Memory Usage. `RES` 값을 이용해 사용 가능한 RAM을 백분율로 표시한다. |
| COMMAND | Command Name or Command Line. 프로세스를 시작하는 데 사용한 command line 또는 관련 프로그램의 이름을 표시한다. <br />대화형 명령 `c`를 사용하면 자세한 command line을 확인할 수 있다. `c`를 사용하면 command line이 따로 없는 프로세스의 경우, 대괄호 `[]` 안에 프로그램 이름만 표시된다. <br />(e.g. 커널스레드 - `[ktreadd]`) |

</br>

## 📜참고

- `$ man top`
- [top (software) - Wikipedia[Website].(2020.01.13)](https://en.wikipedia.org/wiki/Top_(software))

- [리눅스 top 정리 및 설명 · 어쩐지 오늘은[Website].(2020.01.13)](https://zzsza.github.io/development/2018/07/18/linux-top/)
- [centos - What does the 'g' mean under VIRT in "top" on linux? - Super User[Website].(2020.01.13)](https://superuser.com/questions/1063432/what-does-the-g-mean-under-virt-in-top-on-linux/1063760)

- [Load Average에 대하여 :: Lunatine's Box  — Lunatine's Box[Website].(2020.01.13)](https://lunatine.net/2016/02/19/about-load-average/)

- [CPU Steal Time의 원인과 대책 | 와탭 블로그[Website].(2020.01.13)](https://www.whatap.io/ko/blog/25/)

- [A Guide to the Linux "Top" Command - Boolean World[Website].(2020.01.13)](https://www.booleanworld.com/guide-linux-top-command/)