# 18-19 실습: dynamic array 정렬
이론: [note](../../../notes/18-dynamic-memory/18-19-dynamic-array-sorting.md)

## 실습 목적
borrowed dynamic array를 count 범위에서 정렬한다.
## 작성할 파일
`dynamic_sort.c`
## 해야 할 일
작은 array를 allocation·초기화하고 `sort(values, count)`로 오름차순 정렬해 출력·해제한다.
## 사용할 개념
in-place sort, borrowed pointer, caller ownership.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic dynamic_sort.c -o dynamic_sort
```
## 실행 방법
```sh
./dynamic_sort
```
## 예상 관찰 결과
1 2 3 4 순서가 출력된다.
## 확인 포인트
sort function이 free하지 않고 caller가 base pointer를 free한다.
## 추가 실습
- ★ descending sort
- ★★ sorted input
- ★★★ ownership contract
## 완료 기준
bounds를 지키며 warning 없이 정렬·해제한다.
