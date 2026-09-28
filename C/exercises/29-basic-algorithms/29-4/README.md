# 29-4 실습: Selection Sort
이론: [note](../../../notes/29-basic-algorithms/29-4-selection-sort.md)
## 실습 목적
suffix minimum으로 sorted prefix를 확장한다.
## 작성할 파일
- `main.c`
## 해야 할 일
각 pass의 `min_index`를 찾고 필요한 경우에만 swap한다.
## 사용할 개념
Selection Sort, minimum search, prefix invariant, in-place.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o selection_sort
```
## 실행 방법
```sh
./selection_sort
```
## 예상 관찰 결과
대표 입력 `{4,2,7,2,1}`은 `1 2 2 4 7`이 된다.

| 입력 | 기대 결과 | 확인 경계 |
|---|---|---|
| empty | empty | no `values[0]` |
| `{5}` | `{5}` | minimum |
| `{1,2,3}` | unchanged | sorted |
| `{3,2,1}` | `{1,2,3}` | reverse |
| `{2,1,2}` | `{1,2,2}` | duplicate |
## 확인 포인트
`start < count`일 때만 `min_index = start`를 만든다.
## 추가 실습
- ★ swap 수를 기록한다.
- ★★ maximum 선택으로 descending sort를 만든다.
- ★★★ pair data로 stability를 관찰한다.
## 완료 기준
prefix invariant와 모든 boundary output을 설명한다.
