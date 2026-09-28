# 29-8 실습: `my_strcpy`
이론: [note](../../../notes/29-basic-algorithms/29-8-my-strcpy.md)
## 실습 목적
capacity를 검사한 뒤 terminator까지 string을 복사한다.
## 작성할 파일
- `main.c`
## 해야 할 일
exact fit과 부족한 buffer를 구분하고 실패 시 partial copy를 만들지 않는다.
## 사용할 개념
C string, capacity, null terminator, non-overlap, status.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o my_strcpy
```
## 실행 방법
```sh
./my_strcpy
```
## 예상 관찰 결과
capacity 4에 `"C17"`은 성공하고 capacity 3은 실패한다.

| source | capacity | 기대 결과 | 확인 경계 |
|---|---:|---|---|
| `""` | 1 | success | terminator only |
| `"C17"` | 4 | success | exact fit |
| `"C17"` | 8 | success | extra space |
| `"C17"` | 3 | failure | one short |
| `"C17"` | 0 | failure | zero capacity |
## 확인 포인트
`capacity == 0`을 `capacity - 1`보다 먼저 검사한다.
## 추가 실습
- ★ failure 뒤 destination unchanged를 검사한다.
- ★★ source length와 필요한 bytes를 표로 만든다.
- ★★★ partial-write contract와 two-pass 설계를 비교한다.
## 완료 기준
모든 capacity boundary에서 OOB 없이 contract를 지킨다.
