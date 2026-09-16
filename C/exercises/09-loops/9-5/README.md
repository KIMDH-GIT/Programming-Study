# 9-5 실습: `break`와 `continue`

이론: [note](../../../notes/09-loops/9-5-break-continue.md)
## 실습 목적
현재 반복 건너뛰기와 반복 종료를 구별한다.
## 작성할 파일
`break_continue.c`
## 해야 할 일
1~10을 순회하며 3의 배수는 `continue`, 8은 `break`로 처리하고 나머지를 출력한다.
## 사용할 개념
`for`, `if`, `%`, `break`, `continue`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic break_continue.c -o break_continue
```
## 실행 방법
```sh
./break_continue
```
## 예상 관찰 결과
1, 2, 4, 5, 7이 출력되고 8에서 종료된다.
## 확인 포인트
3과 6은 건너뛰고 8 이후 값은 검사하지 않는가?
## 추가 실습
- ★ 짝수 건너뛰기 - ★★ 첫 7의 배수 종료 - ★★★ `while` 버전 흐름표
## 완료 기준
- [ ] 경고 없음 - [ ] 두 이동 차이 설명 - [ ] 예상 출력 일치
