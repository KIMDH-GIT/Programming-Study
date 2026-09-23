# 22-9 실습: driver interface
이론: [note](../../../notes/22-function-pointers/22-9-driver-interface.md)
## 실습 목적
function pointer members로 작은 mock driver interface를 구성한다.
## 작성할 파일
`led_driver_interface.c`
## 해야 할 일
set/get members와 compatible mock implementation을 연결해 state를 변경·조회한다.
## 사용할 개념
struct, designated initializer, function pointer member, compatible signature.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic led_driver_interface.c -o led_driver_interface
```
## 실행 방법
```sh
./led_driver_interface
```
## 예상 관찰 결과
set한 LED state가 get 결과로 출력된다.
## 확인 포인트
구조체에 function 자체가 아니라 pointer values가 저장됨을 설명한다.
## 추가 실습
- ★ toggle operation을 추가한다.
- ★★ 두 mock implementations를 바꿔 연결한다.
- ★★★ optional member contract와 null check를 설계한다.
## 완료 기준
모든 member signatures가 맞고 warning 없이 state를 관찰한다.
