# 30-6 실습: linked list print
이론: [note](../../../notes/30-data-structures/30-6-linked-list-print.md)
## 실습 목적
`const Node *`를 따라 값만 출력하고 빈 list의 출력을 고정한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`int value`와 `Node *next`를 가진 node chain을 만들고 `list_print(const Node *head)`로 `[]\n` 또는 `[값, 값]\n`을 출력한다. 자동 저장 기간 node를 사용한다면 `free`하지 않는다.
## 사용할 개념
`NULL`, `const`, read-only traversal, node lifetime.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o list_print
```
## 실행 방법
```sh
./list_print
```
## 예상 관찰 결과
빈 list와 `{10, 20, 30}` 순서로 호출하면 정확히 두 줄 `[]`와 `[10, 20, 30]`이 나온다.

| 입력 | 정확한 출력 | 경계 |
|---|---|---|
| `NULL` | `[]` | 빈 list |
| `7` | `[7]` | 하나, 뒤 쉼표 없음 |
| `10 -> 20 -> 30` | `[10, 20, 30]` | 여러 개, 순서 보존 |
| `-1 -> -1` | `[-1, -1]` | 음수와 중복 |
## 확인 포인트
pointer 주소 숫자가 아니라 값 순서로 비교한다. 함수가 head나 node의 `next`를 변경하지 않는지 확인한다.
## 추가 실습
- ★ 음수와 중복 값의 정확한 출력을 비교한다.
- ★★ 호출 전후 head와 연결 관계를 확인한다.
- ★★★ `FILE *`로 출력 대상을 지정하는 함수 계약을 작성한다.
## 완료 기준
빈 list와 단일·다중 node의 출력이 표와 일치하고 원래 chain이 보존된다.
