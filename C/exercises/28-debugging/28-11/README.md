# 28-11 실습: 결함 재현·수정·재검증
이론: [note](../../../notes/28-debugging/28-11-reproduce-fix-retest.md)
## 실습 목적
defined logic bug를 evidence 기반으로 수정한다.
## 작성할 파일
- `bug.c`
- `main.c`
## 해야 할 일
마지막 원소 누락을 재현하고 iteration state를 관찰한 뒤 condition을 수정한다.
## 사용할 개념
reproducer, expected/actual, hypothesis, root cause, regression.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g -Og bug.c -o bug
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g -Og main.c -o fixed_sum
```
## 실행 방법
```sh
./bug
./fixed_sum
```
## 예상 관찰 결과
bug version은 `3`, 수정본은 expected `6`을 출력한다.
## 확인 포인트
두 program 모두 defined이며 차이는 requirement 충족 여부다.
## 추가 실습
- ★ 원소 하나 input을 추가한다.
- ★★ empty range를 검증한다.
- ★★★ evidence ledger를 작성한다.
## 완료 기준
재현·가설·수정·regression evidence를 남긴다.
