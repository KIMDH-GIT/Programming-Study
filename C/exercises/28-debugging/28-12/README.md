# 28-12 실습: Part 28 종합 복습
이론: [note](../../../notes/28-debugging/28-12-part-28-review.md)
## 실습 목적
compiler·GDB·sanitizer·test의 역할을 종합한다.
## 작성할 파일
- `main.c`
## 해야 할 일
linear search 예제를 strict debug build하고 GDB로 index와 return path를 관찰한다.
## 사용할 개념
diagnostic, debug info, breakpoint, stepping, state, regression.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g -Og main.c -o part28_review
```
## 실행 방법
```sh
./part28_review
gdb ./part28_review
```
## 예상 관찰 결과
program은 `2`를 출력하고 GDB에서 target 6을 찾는 path를 관찰한다.
## 확인 포인트
각 evidence가 증명하는 범위를 과장하지 않는다.
## 추가 실습
- ★ not-found input을 검증한다.
- ★★ 도구별 evidence 표를 만든다.
- ★★★ 전체 debugging report를 작성한다.
## 완료 기준
12개 Step의 도구와 C17 경계를 설명하고 실제 관찰한다.
