# 21-5 실습: unsigned `<<`, `>>`
이론: [note](../../../notes/21-bitwise-operators/21-5-unsigned-shifts.md)
## 실습 목적
valid count에서 unsigned shifts를 수행한다.
## 작성할 파일
`unsigned_shifts.c`
## 해야 할 일
unsigned int width를 계산하고 count를 검사한 뒤 left/right 결과를 출력한다.
## 사용할 개념
`<<`, `>>`, `1u`, `CHAR_BIT`, shift count.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic unsigned_shifts.c -o unsigned_shifts
```
## 실행 방법
```sh
./unsigned_shifts
```
## 예상 관찰 결과
정의된 unsigned shift 결과가 hex로 출력된다.
## 확인 포인트
invalid count와 signed shift를 실행하지 않는다.
## 추가 실습
- ★ count 0을 시험한다.
- ★★ valid counts를 순회한다.
- ★★★ UB cases를 분석한다.
## 완료 기준
count 검사를 포함해 warning 없이 실행한다.
