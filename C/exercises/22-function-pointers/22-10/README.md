# 22-10 실습: 모의 GPIO driver와 callback
이론: [note](../../../notes/22-function-pointers/22-10-mock-gpio-driver-and-callback.md)
## 실습 목적
mock GPIO driver operations와 change callback을 함께 사용한다.
## 작성할 파일
`mock_gpio_callback.c`
## 해야 할 일
write/read interface, mock register, nullable change callback을 연결한다.
## 사용할 개념
driver interface, object pointer, function pointer, callback, null check.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic mock_gpio_callback.c -o mock_gpio_callback
```
## 실행 방법
```sh
./mock_gpio_callback
```
## 예상 관찰 결과
update한 GPIO value가 callback을 통해 출력된다.
## 확인 포인트
object pointer와 function pointer를 `void *`로 섞지 않고 mock을 실제 MMIO라고 부르지 않는다.
## 추가 실습
- ★ callback 없이 update한다.
- ★★ 마지막 value를 callback에서 기록한다.
- ★★★ bit set/clear operations를 interface에 추가한다.
## 완료 기준
null-safe callback과 compatible driver members가 warning 없이 동작한다.
