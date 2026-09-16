# 16-3 실습: 역참조로 호출자 객체 변경

이론: [note](../../../notes/16-pointers-and-functions/16-3-modify-caller-through-pointer.md)
## 실습 목적
pointer parameter를 통해 caller object를 수정한다.
## 작성할 파일
`modify_caller.c`
## 해야 할 일
함수에서 caller int를 100으로 바꾸고 caller에서 출력한다.
## 사용할 개념
pointer value copy, indirection, caller object, valid target.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic modify_caller.c -o modify_caller
```
## 실행 방법
```sh
./modify_caller
```
## 예상 관찰 결과
caller에서 100이 출력된다.
## 확인 포인트
call by reference가 아니라 copied pointer dereference로 설명하는가?
## 추가 실습
- ★ double - ★★ conditional modify - ★★★ state diagram
## 완료 기준
- [ ] 경고 없음 - [ ] caller 100 - [ ] p/*p 구분
