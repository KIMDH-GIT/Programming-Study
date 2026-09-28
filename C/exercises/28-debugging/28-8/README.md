# 28-8 실습: 변수·메모리 출력
이론: [note](../../../notes/28-debugging/28-8-printing-variables-and-memory.md)
## 실습 목적
typed value와 raw memory 관찰을 구분한다.
## 작성할 파일
- `main.c`
## 해야 할 일
배열과 total을 `print`, `x/3dw`, `info locals`로 읽는다.
## 사용할 개념
GDB expression, memory examination, object type, target.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g -Og main.c -o gdb_data
```
## 실행 방법
```sh
gdb ./gdb_data
```
## 예상 관찰 결과
배열 값 4·5·6, total 15와 program output `15`를 확인한다.
## 확인 포인트
read-only observation을 사용하고 raw bytes를 typed value와 구분한다.
## 추가 실습
- ★ pointer를 출력한다.
- ★★ format을 바꿔 memory를 읽는다.
- ★★★ optimization level별 관찰을 비교한다.
## 완료 기준
variable과 memory를 안전하게 관찰하고 층을 구분한다.
