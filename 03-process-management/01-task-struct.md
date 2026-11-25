# task_struct란?

task_struct는 리눅스 커널에서 태스크의 실행 상태와 관련 정보를 관리하는 구조체다
(태스크란? 커널이 CPU를 배정하는 단위다)

## task_struct에 저장되는 정보

- pid: 각 태스크를 구분하는 번호
- tgid: 같은 스레드 그룹에 있는 태스크들이 공유하는 번호
- comm: 태스크의 이름
- mm: 가상 메모리 정보를 관리하는 mm_struct를 가리킨다
- files: 파일 디스크립터 정보를 관리하는 files_struct를 가리킨다

그리고 task_struct에는 태스크가 실행할 수 있는 상태인지 작업을 기다리는 중인지 같은 정보도 저장된다 커널이 이 정보를 써서 실행할 준비가 된 태스크를 확인할 수 있다

## 참고자료

- [Linux Kernel Labs - Processes](https://linux-kernel-labs.github.io/refs/heads/master/lectures/processes.html)
- [MINZKN - 프로세스 관리](https://www.minzkn.com/linuxkernel/pages/process-management.html)
- [Electronic Artist - task_struct 구조체](https://eartist.tistory.com/590)