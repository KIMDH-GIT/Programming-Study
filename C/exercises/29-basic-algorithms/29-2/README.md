# 29-2 실습: Linear Search
이론: [note](../../../notes/29-basic-algorithms/29-2-linear-search.md)
## 실습 목적
first occurrence contract를 가진 Linear Search를 구현한다.
## 작성할 파일
- `main.c`
## 해야 할 일
성공 status와 output index를 분리하고 `[0, count)`만 순회한다.
## 사용할 개념
Linear Search, invariant, `const`, `size_t`, first occurrence.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o linear_search
```
## 실행 방법
```sh
./linear_search
```
## 예상 관찰 결과
```text
index=1
missing=0
empty=0
```

| 입력 | target | 기대 결과 | 확인 경계 |
|---|---:|---|---|
| empty | 2 | not found | empty |
| `{2}` | 2 | index 0 | first/last |
| `{4,2,7,2}` | 2 | index 1 | duplicate first |
| `{4,2,7,2}` | 9 | not found | failure |
## 확인 포인트
`i < count`와 미발견 시 output index 미사용을 확인한다.
## 추가 실습
- ★ comparison 수를 센다.
- ★★ last occurrence contract로 바꾼다.
- ★★★ explicit tests 뒤 random differential test를 추가한다.
## 완료 기준
first·last·duplicate·empty·missing cases가 contract와 일치한다.
