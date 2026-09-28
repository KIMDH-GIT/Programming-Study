# 29-3 실습: Bubble Sort
이론: [note](../../../notes/29-basic-algorithms/29-3-bubble-sort.md)
## 실습 목적
pass invariant와 early exit를 가진 ascending Bubble Sort를 구현한다.
## 작성할 파일
- `main.c`
## 해야 할 일
valid adjacent pair만 비교하고 swap 없는 pass에서 종료한다.
## 사용할 개념
Bubble Sort, nested loop, swap, stable, in-place, early exit.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o bubble_sort
```
## 실행 방법
```sh
./bubble_sort
```
## 예상 관찰 결과
대표 입력 `{4,2,7,2,1}`은 `1 2 2 4 7`이 된다.

| 입력 | 기대 결과 | 확인 경계 |
|---|---|---|
| empty | empty | no access |
| `{1}` | `{1}` | minimum |
| `{1,2,3}` | unchanged | early exit |
| `{3,2,1}` | `{1,2,3}` | reverse |
| `{2,1,2}` | `{1,2,2}` | duplicate |
## 확인 포인트
각 pass 뒤 확정되는 suffix와 `i < end`를 기록한다.
## 추가 실습
- ★ pass 수를 센다.
- ★★ descending order로 변경한다.
- ★★★ key와 original position으로 stability를 확인한다.
## 완료 기준
모든 boundary vector가 정렬되고 bounds 밖 access가 없다.
