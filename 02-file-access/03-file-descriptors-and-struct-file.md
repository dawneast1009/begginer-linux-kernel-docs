# 파일 디스크립터(FD), struct file이란?

파일 디스크립터는 프로그램이 파일을 열었을 때 커널이 반환하는 번호다
프로그램은 이 번호를 사용해서 어떤 파일을 읽거나 쓸지 구분한다

예를 들어 open()으로 파일을 열었을 때 11이라는 번호를 받았다면 read(11, buf, size)를 호출해서 그 파일을 읽을 수 있다

## struct file이란?

struct file은 커널에서 열린 파일의 상태를 관리하는 구조체다

예를 들어 open()이 11을 반환하면 FD 테이블의 11번에 해당 파일의 struct file이 연결된다
프로그램이 read(11, buf, size)를 호출하면 커널은 FD 테이블에서 그 struct file을 찾아 읽기 요청을 처리한다


### f_pos

f_pos는 파일에서 읽거나 쓸 오프셋을 저장하는 역할을 한다
파일의 시작점을 0으로 두고 거기서 몇 바이트 떨어져 있는지를 숫자로 저장한다

ABCDE가 들어 있는 파일을 열면 f_pos가 0이라서 맨 앞에 있는 A부터 읽는다

read()로 2바이트를 읽으면 A, B를 읽고 f_pos는 2가 된다
그래서 다음 read()는 C부터 시작하고 2바이트를 요청하면 C와 D를 읽는다

### f_flags

f_flags에는 열린 파일을 사용할 때 적용되는 설정이 저장된다
예를 들면 데이터를 읽을 때 기다리지 않게 하는 O_NONBLOCK이 있다

시리얼 포트에 읽을 데이터가 아직 없으면 read()는 데이터가 들어올 때까지 기다릴 수 있는데 O_NONBLOCK이 설정되어 있으면 데이터를 기다리지 않고 바로 반환한다

다른 설정은 [open(2) 매뉴얼](https://wariua.github.io/man-pages-ko/open%282%29/)에서 확인할 수 있다

### f_op

f_op는 열린 파일에서 사용할 file_operations를 가리킨다

예를 들어 read() 요청이 들어오면 커널은 f_op가 가리키는 file_operations에서 이 파일의 읽기를 처리할 함수를 찾는다
file_operations는 문자 디바이스의 콜백 함수를 설명하는 글에서 따로 다룬다

### private_data

private_data에는 드라이버가 열린 파일을 처리하는 데 필요한 정보의 주소를 저장한다
파일을 열 때 주소를 저장해 두면 나중에 읽거나 쓸 때 그 주소를 다시 사용할 수 있다

## 참고자료

- [Linux manual - open(2)](https://wariua.github.io/man-pages-ko/open%282%29/)
- [MINZKN - 리눅스 커널의 VFS 계층](https://www.minzkn.com/linuxkernel/pages/vfs.html)
- [CodeByDev - 파일 디스크립터와 열린 파일 설명](https://codebydev.co.kr/software-engineering/os/file-descriptor-open-file-description/)
- [쿠오 - 파일 시스템, inode와 VFS](https://parksangji.github.io/posts/os-filesystem-inode-vfs/)