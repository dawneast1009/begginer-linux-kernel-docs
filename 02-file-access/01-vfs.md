# 5. VFS(Virtual File System)란?

VFS는 리눅스 커널에서 서로 다른 파일 시스템을 같은 방식으로 사용할 수 있게 해주는 계층이다

파일 시스템은 파일과 디렉터리 등의 정보를 관리하는데 ext4, xfs처럼 파일 시스템마다 내부 구조나 데이터를 저장하는 방식이 다르다

프로그램이 파일 시스템마다 다른 동작 방식을 알아야 하게되면 작업이 복잡해져서 VFS로 파일을 사용할 수 있는 공통된 계층을 사용한다

이로인해 파일 시스템의 구조를 몰라도 open(), read(), write() 같은 함수로 파일을 사용할 수 있다

## 동작 방식

프로그램이 파일을 열거나 읽으려고 하면 VFS가 그 요청을 처리하는 데 필요한 동작으로 연결한다

프로그램이 open()을 호출하면 파일 경로를 탐색해서 그 파일을 찾는 작업을 한다
파일을 연 뒤 read()나 write()를 사용하면 VFS는 열린 파일에 연결된 연산 함수를 통해 읽기나 쓰기 작업을 실행한다

## 사용하는 이유

파일 시스템마다 내부 구조가 달라도 프로그램이 같은 방식으로 파일을 사용할 수 있도록 하기 위해서 VFS를 사용한다

물론 파일을 열거나 읽을 때 필요한 접근 권한 검사도 이루져서 VFS를 사용한다고 모든 파일에 자유롭게 접근할 수는 없다

## 시스템 콜과의 관계

시스템 콜은 사용자 프로그램이 커널에 작업을 요청할 때 사용하는 인터페이스고
VFS는 커널 내부에서 파일 시스템 관련 요청을 처리하는 계층이다

프로그램이 파일을 읽으려고 read()를 호출하면 시스템 콜로 커널에 요청이 전달되고 
VFS를 거쳐 파일의 읽기 동작이 처리된다

그래서 시스템 콜은 프로그램의 요청을 커널에 전달하고
VFS는 파일 시스템의 차이를 추상화해서 파일 관련 작업을 처리할 수 있도록 연결하는 역할을 한다

## 참고자료

- [Linux Kernel Documentation - VFS](https://docs.kernel.org/filesystems/vfs.html)
- [Linux Kernel Labs - File Systems](https://linux-kernel-labs.github.io/refs/heads/master/so2/lec8-filesystems.html)
- [Linux manual - open(2)](https://man7.org/linux/man-pages/man2/open.2.html)
