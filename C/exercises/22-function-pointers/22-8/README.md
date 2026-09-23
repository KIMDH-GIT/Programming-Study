# 22-8 실습: interrupt handler와 일반 callback의 차이
이론: [note](../../../notes/22-function-pointers/22-8-interrupt-handler-vs-callback.md)
## 실습 목적
portable 일반 callback을 작성하고 platform ISR과 구분한다.
## 작성할 파일
`ordinary_callback.c`
## 해야 할 일
nullable `void (*)(void)` callback을 `notify`에서 검사한 뒤 호출한다.
## 사용할 개념
ordinary callback, null check, ISO C17와 platform extension 구분.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic ordinary_callback.c -o ordinary_callback
```
## 실행 방법
```sh
./ordinary_callback
```
## 예상 관찰 결과
callback이 있을 때만 event 메시지가 출력된다.
## 확인 포인트
이 program을 interrupt handler 예제라고 부르지 않고 vendor attribute를 넣지 않는다.
## 추가 실습
- ★ 다른 ordinary callback을 전달한다.
- ★★ null callback 경로를 확인한다.
- ★★★ MCU 문서의 ISR contract를 별도 표로 조사한다.
## 완료 기준
portable callback이 warning 없이 실행되고 ISR 차이를 설명한다.
