# file_operations, 문자 디바이스의 콜백 함수

디바이스 파일은 프로그램이 장치에 접근할 때 사용하는 파일이다
프로그램은 이 파일을 열고 read()나 write()를 호출해서 장치에 읽기나 쓰기를 요청할 수 있다

커널은 이런 요청을 받으면 어떤 드라이버 함수를 호출할지 알아야 한다
그래서 드라이버는 파일을 열거나 읽고 쓰는 작업을 처리할 함수의 주소를 file_operations라는 구조체에 등록해 둔다

이렇게 커널이 요청을 받았을 때 호출하도록 미리 등록해 둔 함수를 콜백 함수라고 한다

프로그램이 문자 디바이스 파일을 연 뒤 read()를 호출하면 커널은 file_operations에 등록된 읽기 콜백을 호출하고 드라이버가 이 콜백에서 데이터를 가져와 프로그램에 전달한다

## 문자 디바이스의 콜백 함수

### open

디바이스 파일을 열 때 호출되고 장치를 사용하기 전에 필요한 초기화 작업을 할 수 있다
나중에도 사용할 정보가 있으면 그 주소를 private_data에 저장해 둘 수 있다

### read

디바이스의 데이터를 사용자 공간으로 전달할 때 호출된다
가져온 데이터가 커널 버퍼에 있으면 copy_to_user()를 사용해서 프로그램이 전달한 버퍼로 복사할 수 있다

읽기에 성공하면 읽은 바이트 수를, 데이터의 끝에 도달한 경우에는 0을 반환한다

### write

사용자 공간의 데이터를 커널로 가져와 디바이스에 전달할 때 호출된다
copy_from_user()로 사용자 공간의 데이터를 커널 버퍼에 복사한다음에 장치에 전달하는 작업을 한다 쓰기에 성공하면 처리한 바이트 수를 반환한다

### unlocked_ioctl

read(), write()로 처리하기 어려운 디바이스 제어 명령을 처리할 때 호출된다

ioctl()로 볼륨을 조절할 수 있는 장치가 있으면 볼륨을 50으로 바꾸고 싶으면 cmd에는 볼륨을 바꾸라는 명령을 넣고 arg에는 원하는 볼륨 값인 50을 전달할 수 있다
(arg란? ioctl 명령 수행에 필요한 데이터 값이나 데이터 주소를 커널에 전달하는 매개변수다)

### release

열린 파일의 마지막 참조가 해제될 때 호출되는 함수다

프로그램이 close()를 호출하면 사용하던 FD와 struct file의 연결이 끊기고
그 struct file을 가리키는 참조가 하나도 남아 있지 않으면 커널이 release를 호출한다

파일을 열 때 버퍼를 만들어 뒀다면 release에서 그 메모리를 정리(해제)한다

## 참고자료

- [Linux Kernel Labs - Character device drivers](https://linux-kernel-labs.github.io/refs/heads/master/labs/device_drivers.html)
- [Linux Kernel Documentation - VFS](https://docs.kernel.org/filesystems/vfs.html)
- [hyeyoo - 문자 디바이스 드라이버 작성](https://hyeyoo.com/85)
- [MINZKN - ioctl 시스템 콜](https://www.minzkn.com/linuxkernel/pages/ioctl.html)
- [MINZKN - 문자 디바이스](https://www.minzkn.com/linuxkernel/pages/char-device.html)