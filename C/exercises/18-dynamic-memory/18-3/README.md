# 18-3 실습: 반환 pointer와 lifetime
이론: [note](../../../notes/18-dynamic-memory/18-3-returning-local-address.md)

## 실습 목적
local 주소 반환과 allocated pointer 반환을 구분한다.
## 작성할 파일
`make_value.c`
## 해야 할 일
`make_value`가 성공 시 allocated `int *`, 실패 시 NULL을 반환하게 한다. caller가 검사·사용·해제한다.
## 사용할 개념
automatic lifetime, allocation-returning function, ownership transfer.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic make_value.c -o make_value
```
## 실행 방법
```sh
./make_value
```
## 예상 관찰 결과
전달한 초기값이 출력된다.
## 확인 포인트
local variable 주소를 반환하지 않는다.
## 추가 실습
- ★ 함수 contract 주석
- ★★ caller-provided output 비교
- ★★★ 잘못된 local 반환 분석
## 완료 기준
failure와 free 책임이 명시되고 leak이 없다.
