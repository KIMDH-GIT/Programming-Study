# 29-13 실습: Part 29 종합 복습
이론: [note](../../../notes/29-basic-algorithms/29-13-part-29-review.md)
## 실습 목적
Part 29의 contract·invariant·complexity·C17 safety를 종합한다.
## 작성할 파일
- `main.c`
## 해야 할 일
sorted array search 예제를 작성하고 전체 Step의 boundary ledger를 완성한다.
## 사용할 개념
precondition, postcondition, invariant, termination, complexity, bounds.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o part29_review
```
## 실행 방법
```sh
./part29_review
```
## 예상 관찰 결과
sorted `{1,2,4,7}`에서 4는 found, 3과 empty input은 absent다.

| scenario | 기대 결과 | 확인 범위 |
|---|---|---|
| target present | found | normal |
| target absent | not found | failure |
| empty range | not found | boundary |
| first/last target | found | off-by-one |
| duplicate input | documented result | contract |
## 확인 포인트
algorithm·C17·compiler·ISA evidence를 같은 주장으로 섞지 않는다.
## 추가 실습
- ★ 13개 Step의 empty behavior를 표로 만든다.
- ★★ search·sort invariant를 비교한다.
- ★★★ 임시 oracle로 representative algorithms를 differential test한다.
## 완료 기준
strict build와 boundary ledger가 모두 PASS이고 각 층을 설명한다.
