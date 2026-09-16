# 10-5 실습: 지역 변수와 scope

이론: [note](../../../notes/10-functions/10-5-local-scope.md)
## 실습 목적
함수별로 같은 이름의 객체가 독립적임을 확인한다.
## 작성할 파일
`local_scope.c`
## 해야 할 일
`main`과 `triple` 함수에 각각 `value`를 두고 호출 전후 값을 출력한다.
## 사용할 개념
parameter, local variable, block scope, 이름 가림.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic local_scope.c -o local_scope
```
## 실행 방법
```sh
./local_scope
```
## 예상 관찰 결과
`triple(4)`는 12를 반환하고 `main`의 원래 값 4는 유지된다.
## 확인 포인트
각 `value`의 선언 위치와 scope를 설명할 수 있는가?
## 추가 실습
- ★ local 결과 변수 - ★★ 안쪽 block 가림 - ★★★ scope 표시
## 완료 기준
- [ ] 경고 없음 - [ ] 결과 12와 4 - [ ] 객체 구분
