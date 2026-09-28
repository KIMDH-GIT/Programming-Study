# 29-12 실습: 배열·함수·포인터로 알고리즘 통합
이론: [note](../../../notes/29-basic-algorithms/29-12-integrating-arrays-functions-and-pointers.md)
## 실습 목적
pointer·count·output·status contract로 min/max를 구한다.
## 작성할 파일
- `main.c`
## 해야 할 일
empty input과 NULL arguments를 먼저 처리하고 nonempty range의 minimum과 maximum을 기록한다. 두 output은 서로 다른 writable `int` objects이며 input array와 겹치지 않는다는 caller precondition을 문서화한다.
## 사용할 개념
array parameter, `const`, `size_t`, output pointer, min/max invariant.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o min_max
```
## 실행 방법
```sh
./min_max
```
## 예상 관찰 결과
empty는 status 0, `{4,9,1,7}`은 `min=1 max=9`를 출력한다.

| 입력 | 기대 결과 | 확인 경계 |
|---|---|---|
| empty | failure status | no first element |
| `{5}` | min=max=5 | single |
| `{2,2,2}` | min=max=2 | all equal |
| `{-3,4,-1}` | min=-3 max=4 | negative |
| `{4,9,1,7}` | min=1 max=9 | normal |
## 확인 포인트
function 안에서 `sizeof(values)`로 count를 구하지 않는다. non-NULL pointer의 lifetime·writability·extent·overlap은 함수가 검사할 수 없으므로 caller contract로 구분한다.
## 추가 실습
- ★ comparison 수를 센다.
- ★★ printing을 별도 function으로 분리한다.
- ★★★ struct result 설계와 비교한다.
## 완료 기준
empty·NULL status와 모든 nonempty outputs가 distinct non-overlapping output contract에 맞게 동작한다.
