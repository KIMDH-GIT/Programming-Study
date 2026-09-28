# 29-1 실습: 입력·출력·경계 조건
이론: [note](../../../notes/29-basic-algorithms/29-1-input-output-and-boundary-conditions.md)
## 실습 목적
구현 전에 입력·출력 contract와 경계 조건을 작성한다.
## 작성할 파일
- `main.c`
## 해야 할 일
정수 배열의 첫 값을 output parameter로 제공하되 empty input은 status 0으로 처리한다.
## 사용할 개념
input contract, output contract, `size_t`, empty range, status.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o boundary_contract
```
## 실행 방법
```sh
./boundary_contract
```
## 예상 관찰 결과
```text
empty=0
first=4
```

| 입력 | 기대 결과 | 확인 경계 |
|---|---|---|
| `NULL, 0` | 실패 status | empty |
| `{4}, 1` | first 4 | minimum nonempty |
| `{4,2,7,1}, 4` | first 4 | normal |
## 확인 포인트
실패 시 output을 읽지 않고 array access 전에 `count`를 검사한다.
## 추가 실습
- ★ 원소 하나를 검사한다.
- ★★ null output pointer contract를 추가한다.
- ★★★ parsing·validation·algorithm·output을 분리한다.
## 완료 기준
contract와 세 deterministic cases가 일치한다.
