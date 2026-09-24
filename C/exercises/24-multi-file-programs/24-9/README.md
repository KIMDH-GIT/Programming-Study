# 24-9 실습: block-scope `static` 복습
이론: [note](../../../notes/24-multi-file-programs/24-9-block-scope-static-review.md)
## 실습 목적
function block의 static object로 호출 사이 상태를 유지한다.
## 작성할 파일
- `main.c`
- `ticket.c`
- `ticket.h`
## 해야 할 일
`ticket_next` 내부의 block-scope static object를 사용해 연속 번호를 반환한다.
## 사용할 개념
block scope, no linkage, static storage duration, function interface.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c ticket.c -o ticket_app
```
## 실행 방법
```sh
./ticket_app
```
## 예상 관찰 결과
세 번의 호출 결과를 별도 variables에 저장해 출력하면 `1 2 3`이 출력된다.
## 확인 포인트
block-scope static을 file-scope internal linkage와 혼동하지 않는다.
## 추가 실습
- ★ 시작 값을 100으로 바꾼다.
- ★★ 각 반환값을 별도 statement에서 저장한다.
- ★★★ file-scope static 구현과 비교한다.
## 완료 기준
세 호출에서 상태가 유지되고 scope·linkage·storage duration을 각각 설명한다.
