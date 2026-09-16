# 9-3 실습: `do-while`

이론: [note](../../../notes/09-loops/9-3-do-while.md)
## 실습 목적
후평가와 최소 1회 실행을 관찰한다.
## 작성할 파일
`do_while_countdown.c`
## 해야 할 일
3부터 1까지 출력하는 `do-while`을 작성하고 초기값 0에서도 실행 흐름을 추적한다.
## 사용할 개념
`do-while`, 후평가, 감소, 마지막 세미콜론.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic do_while_countdown.c -o do_while_countdown
```
## 실행 방법
```sh
./do_while_countdown
```
## 예상 관찰 결과
3, 2, 1이 출력되며 초기값 0에서도 본문이 한 번 실행된다.
## 확인 포인트
본문 뒤에 조건을 검사하고 `while (...)` 뒤에 `;`를 썼는가?
## 추가 실습
- ★ 1회 실행 - ★★ 2씩 증가 - ★★★ `while` 버전과 비교
## 완료 기준
- [ ] 경고 없음 - [ ] 후평가 설명 - [ ] 마지막 세미콜론 확인
