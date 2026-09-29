# 30-10 실습: `peek`
이론: [note](../../../notes/30-data-structures/30-10-peek.md)
## 실습 목적
linked stack의 top 값을 읽고도 소유권·연결·size를 유지한다.
## 작성할 파일
- `main.c`
## 해야 할 일
`typedef struct { Node *top; size_t size; } Stack;`을 사용한다. `stack_peek(const Stack *stack, int *value)`는 빈 stack과 NULL 인자에서 0과 변경 없는 output을, 성공 시 1과 top 값을 준다. non-NULL output은 호출과 반환까지 lifetime이 유지되는 writable `int` 객체를 가리키며 `Stack` 객체나 stack이 소유한 어떤 node와도 겹치지 않는다. 두 번 연속 호출해 top과 size가 그대로인지 확인한다. 정리 함수의 인자는 NULL이 아니며 ownership과 chain 불변식을 만족하는 살아 있는 writable `Stack`을 가리켜야 하고, 유효한 빈 stack은 허용한다. 종료 전에 모든 node를 해제한다.
## 사용할 개념
LIFO ADT, `const`, output pointer, ownership, `NULL`.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o stack_peek
```
## 실행 방법
```sh
./stack_peek
```
## 예상 관찰 결과
빈 상태 확인 후 -1과 8을 push하면 다음을 출력한다.
```text
empty=0 value=99
peek=1 value=8 size=2
again=1 value=8 unchanged=1
```

| 입력 상태 | status / output | top / size | 경계 |
|---|---|---|---|
| 빈 stack | 0 / 기존 99 유지 | `NULL` / 0 | 없음 |
| NULL stack 또는 NULL output | 0 / 유효한 output 유지 | 변경 없음 | NULL 인자 |
| 한 node `-1` | 1 / -1 | 같은 node / 1 | 값 sentinel 금지 |
| `-1 -> 8` push 후 두 번 peek | 각각 1 / 8 | 같은 top / 2 | 반복 읽기 |
| destroy | - | `NULL` / 0 | 소유권 종료 |
## 확인 포인트
`size == 0` iff `top == NULL`이고 chain은 acyclic·`NULL`-terminated이다. `size`는 `top`에서 도달 가능한 live node 수와 같고, non-NULL stack 인자는 모든 연산 진입 전에 이 불변식을 만족한다. `const Stack *`는 ROM 배치를 뜻하지 않으며 node까지 deep const로 만들지 않는다. 함수 구현은 어느 node도 변경하지 않는다.
## 추가 실습
- ★ 단일 node를 반복 peek한다.
- ★★ NULL 인자와 빈 stack의 output 보존을 확인한다.
- ★★★ 호출 전후 top과 size를 비교한다.
## 완료 기준
빈·NULL에서 안전하게 실패하고 반복 성공에서 같은 값과 변경 없는 top·size를 확인하며 node가 모두 정리된다.
