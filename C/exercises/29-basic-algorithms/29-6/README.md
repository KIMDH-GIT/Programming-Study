# 29-6 실습: Binary Search
이론: [note](../../../notes/29-basic-algorithms/29-6-binary-search.md)
## 실습 목적
sorted precondition 아래 half-open range로 target을 찾는다.
## 작성할 파일
- `main.c`
## 해야 할 일
`mid = begin + (end - begin) / 2`를 사용하고 status와 index를 분리한다.
## 사용할 개념
Binary Search, sorted precondition, half-open interval, midpoint, `size_t`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o binary_search
```
## 실행 방법
```sh
./binary_search
```
## 예상 관찰 결과
정렬된 `{1,3,5,7,9}`에서 7은 index 3, 4와 empty input은 not found다.

| 입력 | target | 기대 결과 | 확인 경계 |
|---|---:|---|---|
| empty | 3 | not found | empty |
| `{5}` | 5 | index 0 | single |
| `{1,3,5,7,9}` | 1 | index 0 | first |
| `{1,3,5,7,9}` | 9 | index 4 | last |
| `{1,3,5,7,9}` | 4 | not found | failure |
## 확인 포인트
매 iteration에서 `[begin, end)` 크기가 줄어드는지 기록한다.
## 추가 실습
- ★ begin·mid·end trace를 작성한다.
- ★★ duplicate target 반환 contract를 정한다.
- ★★★ 같은 vectors의 Linear Search comparison 수와 비교한다.
## 완료 기준
sorted inputs의 boundary와 failure cases가 안전하게 종료한다.
