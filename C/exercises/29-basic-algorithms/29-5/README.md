# 29-5 실습: Insertion Sort
이론: [note](../../../notes/29-basic-algorithms/29-5-insertion-sort.md)
## 실습 목적
sorted prefix에 key를 안전하게 삽입한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`j > 0` guard 뒤 `values[j - 1]`을 읽고 큰 원소를 shift한다.
## 사용할 개념
Insertion Sort, sorted prefix, shift, stability, unsigned index.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o insertion_sort
```
## 실행 방법
```sh
./insertion_sort
```
## 예상 관찰 결과
대표 입력 `{4,2,7,2,1}`은 `1 2 2 4 7`이 된다.

| 입력 | 기대 결과 | 확인 경계 |
|---|---|---|
| empty | empty | loop skipped |
| `{1}` | `{1}` | minimum |
| `{1,2,3}` | unchanged | best path |
| `{3,2,1}` | `{1,2,3}` | maximum shifts |
| `{2,1,2}` | `{1,2,2}` | equal handling |
## 확인 포인트
`j >= 0`을 `size_t` loop condition으로 쓰지 않는다.
## 추가 실습
- ★ shift 수를 기록한다.
- ★★ descending order를 구현한다.
- ★★★ equal-key original positions로 stability를 확인한다.
## 완료 기준
unsigned underflow 없이 모든 vectors가 정렬된다.
