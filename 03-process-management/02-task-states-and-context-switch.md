# 실행 가능 상태, 대기 상태, 문맥 교환(Context Switch)

## 실행 가능 상태

실행 가능 상태는 태스크가 CPU에서 실행될 준비가 되어 있는 상태다

CPU에서 태스크 A가 실행 중이면 태스크 B는 실행할 준비가 끝났어도 CPU를 배정받을 때까지 기다릴 수 있다
리눅스에서는 실행 중인 A와 CPU를 기다리는 B가 둘 다 TASK_RUNNING 상태다

## 대기 상태

대기 상태는 태스크가 작업을 이어가는 데 필요한 조건이 충족될 때까지 잠들어 있는 상태다

태스크가 read()를 호출했는데 읽을 데이터가 아직 없으면 데이터가 들어올 때까지 잠들 수 있다
데이터가 준비되면 커널이 태스크를 깨우고 태스크는 다시 실행 가능한 상태가 된다
그다음에 CPU를 배정받으면 읽기 작업을 이어서 처리한다

리눅스에서는 시그널이 왔을 때 깨어날 수 있는지에 따라 대기 상태를 구분한다

TASK_INTERRUPTIBLE 상태에서는 기다리던 이벤트가 발생하거나 처리할 시그널이 오면 깨어난다
TASK_UNINTERRUPTIBLE 상태에서는 시그널이 왔다는 이유만으로 깨어나지 않는다

## 문맥 교환(Context Switch)

문맥 교환은 CPU에서 실행 중인 태스크를 다른 태스크로 바꾸는 과정이다

태스크 A가 데이터를 기다리면서 대기 상태가 되면 스케줄러는 실행할 준비가 된 태스크 B를 선택할 수 있다

이때 커널은 A의 레지스터나 스택 포인터 같은 실행 정보를 저장하고 B의 실행 정보를 복원한다
A가 다시 CPU를 배정받으면 저장해 둔 정보를 사용해서 멈췄던 부분부터 이어서 실행한다

## 참고자료

- [Linux Kernel Labs - Processes](https://linux-kernel-labs.github.io/refs/heads/master/lectures/processes.html)
- [MINZKN - 프로세스 관리](https://www.minzkn.com/linuxkernel/pages/process-management.html)
- [MINZKN - 문맥 교환](https://www.minzkn.com/linuxkernel/pages/context-switching.html)