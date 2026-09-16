# 10-4 실습: 반환값과 `void`

이론: [note](../../../notes/10-functions/10-4-return-void.md)
## 실습 목적
값을 반환하는 함수와 동작만 수행하는 함수를 구분한다.
## 작성할 파일
`return_void.c`
## 해야 할 일
`void print_message(void)`와 `int absolute(int value)`를 정의해 각각 호출한다.
## 사용할 개념
return type, `return expression;`, `return;`, 두 위치의 `void`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic return_void.c -o return_void
```
## 실행 방법
```sh
./return_void
```
## 예상 관찰 결과
메시지와 `absolute(-5)`의 결과 5가 출력된다.
## 확인 포인트
`void` 함수 결과를 값처럼 사용하지 않았는가?
## 추가 실습
- ★ 인사 함수 - ★★ 양수 판별 - ★★★ 잘못된 return 분석
## 완료 기준
- [ ] 경고 없음 - [ ] 두 함수 동작 확인 - [ ] `void` 의미 구분
