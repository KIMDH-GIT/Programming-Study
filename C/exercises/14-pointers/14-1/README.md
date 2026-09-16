# 14-1 실습: 메모리 주소란 무엇인가

이론: [note](../../../notes/14-pointers/14-1-memory-address.md)
## 실습 목적
object value와 object를 가리키는 pointer value를 구분한다.
## 작성할 파일
`object_address.c`
## 해야 할 일
두 int objects의 values와 addresses를 올바른 format으로 출력한다.
## 사용할 개념
object, identifier, value, address, `%p`, `(void *)`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic object_address.c -o object_address
```
## 실행 방법
```sh
./object_address
```
## 예상 관찰 결과
정수 values와 구현이 표시한 pointer values가 각각 출력된다.
## 확인 포인트
address를 `%d`로 출력하거나 물리 RAM 번호라고 단정하지 않는가?
## 추가 실습
- ★ char address - ★★ 반복 관찰 - ★★★ 계층 구분표
## 완료 기준
- [ ] 경고 없이 컴파일 - [ ] format 정확 - [ ] value/address 구분
