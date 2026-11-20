# 일반 사용자, root 권한 차이

리눅스에서는 같은 명령어를 실행해도 UID, GID, 파일 권한, capability 등의 영향에 의해 실행 결과가 달라질 수 있다

커널은 프로세스가 작업을 요청하면 그 프로세스의 사용자, 그룹 정보, 권한 등을 확인해서 작업을 허용 또는 거부한다

### 일반 사용자

일반 사용자는 자신에게 허용된 권한 안에서 작업을 수행할 수 있다
예를 들면 파일을 읽거나 쓸 때 파일의 소유자, 그룹 이나 읽기, 쓰기 권한 등을 확인해서 접근이 가능한지 판단한다

### root 사용자

root는 UID가 0인 사용자고 시스템 관리에 필요한 여러 권한을 가지고 있다
일반 사용자가 수행할 수 없는 작업도 가능하지만 root도 모든 작업을 무조건 실행할 수 있는 건 아니다

capability가 제한되어 있거나 SELinux 같은 별도의 보안 정책이 적용되어 있으면 일부 작업이 제한될 수 있다

## Capability란?

Capability는 root가 가지고 있던 권한을 세부 권한으로 나눈 것이다
필요한 capability를 부여받으면 사용자로 실행한 프로세스도 해당 권한으로 작업을 수행할 수 있다

몇가지 예를 들어보자면

### CAP_DAC_READ_SEARCH

파일 읽기와 디렉터리 탐색에 대한 일반적인 파일 권한 검사를 우회할 수 있는 권한이다
/etc/shadow 파일은 일반 사용자가 기본적으로 읽을 수 없지만 이 권한을 가지고 있다면 읽을 수 있다

### CAP_CHOWN

파일의 소유자나 그룹을 변경할 수 있는 권한이다

사용자는 자신이 소유한 파일이어도 소유자를 다른 사용자로 변경할 수 없지만 이 권한이 있으면 변경할 수 있다

### CAP_KILL

다른 프로세스에 시그널을 보낼 때 적용되는 일반적인 권한 검사를 우회할 수 있는 권한이다

사용자는 자신과 UID가 일치하는 프로세스에 시그널을 보낼 수 있지만 이 권한을 가지고 있다면 다른 사용자의 프로세스에도 시그널을 보낼 수 있다

이렇게 capability마다 허용하는 작업이 달라서 하나의 root권한을 가지고 있더라도 무조건 모든것을 실행시킬 수 없을수도 있다

## root 권한, 커널 모드 차이

root 권한은 프로세스가 어떤 작업을 수행할 수 있는지와 관련되어 있고
커널 모드는 CPU가 커널 코드를 실행할 때 사용하는 모드다

root로 실행한 일반 프로그램도 사용자 모드에서 실행되고 커널의 기능이 필요하면 시스템 콜을 사용해야 한다

커널은 요청한 프로세스의 권한을 확인해서 작업을 처리하기 때문에 커널 모드에서 작업이 실행된다고 모든 요청이 허용되는 것은 아니다

## 참고자료

- [Linux manual - credentials(7)](https://man7.org/linux/man-pages/man7/credentials.7.html)
- [Linux manual - capabilities(7)](https://man7.org/linux/man-pages/man7/capabilities.7.html)
- [Linux manual - path_resolution(7)](https://man7.org/linux/man-pages/man7/path_resolution.7.html)
- [Linux Kernel Documentation - Linux Security Modules](https://docs.kernel.org/security/lsm.html)
