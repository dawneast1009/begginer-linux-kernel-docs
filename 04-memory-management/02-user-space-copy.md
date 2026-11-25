# copy_to_user(), copy_from_user()란?

프로그램이 read()나 write()를 호출하면 데이터를 주고받을 버퍼의 주소를 전달한다
그래서 커널에서는 사용자 공간에 있는 버퍼와 데이터를 주고받을 때 copy_to_user()나 copy_from_user()를 사용한다

## copy_to_user()

커널 공간의 데이터를 사용자 공간으로 복사할 때 사용하는 함수다

프로그램이 문자 디바이스에서 read()로 데이터를 읽으려고 하면 드라이버는 읽은 데이터를 프로그램이 전달한 버퍼에 넣어 줘야 해서 copy_to_user()로 커널 버퍼에 있는 데이터를 프로그램의 버퍼로 복사한다

## copy_from_user()

사용자 공간의 데이터를 커널 공간으로 복사할 때 사용하는 함수다

프로그램이 write()를 호출하면 드라이버는 프로그램이 전달한 버퍼에서 데이터를 가져와야 해서
copy_from_user()로 데이터를 커널 버퍼로 가져오고 그 뒤에 장치에 전달한다

## 반환값

두 함수는 복사할 대상 주소, 원본 주소, 복사할 크기를 인자로 받는다
복사가 끝나면 복사하지 못한 바이트 수를 반환한다

전부 복사하면 복사하지 못한 바이트가 없으므로 0을 반환한다

## 참고자료

- [Linux Kernel Documentation - 사용자 공간 복사 함수](https://docs.kernel.org/kernel-hacking/hacking.html#copy-to-user-copy-from-user-get-user-put-user)
- [Linux Kernel Labs - Character device drivers](https://linux-kernel-labs.github.io/refs/heads/master/labs/device_drivers.html)
- [MINZKN - 문자 디바이스](https://www.minzkn.com/linuxkernel/pages/char-device.html)