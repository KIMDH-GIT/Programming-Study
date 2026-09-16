# 10-2 실습: 함수 선언과 prototype

이론: [note](../../../notes/10-functions/10-2-declaration-prototype.md)
## 실습 목적
정의보다 앞선 호출에 prototype이 필요한 이유를 확인한다.
## 작성할 파일
`function_prototype.c`
## 해야 할 일
`int subtract(int, int);`를 `main` 위에 선언하고 definition은 `main` 아래에 둔다.
## 사용할 개념
declaration, prototype, definition, call, declaration order.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic function_prototype.c -o function_prototype
```
## 실행 방법
```sh
./function_prototype
```
## 예상 관찰 결과
`subtract(9, 4)`의 결과 `5`가 출력된다.
## 확인 포인트
prototype과 definition의 함수 타입이 일치하는가?
## 추가 실습
- ★ 이름 없는 parameter 목록 - ★★ `(void)` 비교 - ★★★ 선언 제거 오류 분석
## 완료 기준
- [ ] 경고 없음 - [ ] 결과 5 - [ ] 네 용어 구분
