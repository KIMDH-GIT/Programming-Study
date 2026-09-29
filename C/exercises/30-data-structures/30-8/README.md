# 30-8 실습: stack 불변식과 `push`
이론: [note](../../../notes/30-data-structures/30-8-stack-invariant-and-push.md)
## 실습 목적
`Node *top`과 `size_t size`를 함께 관리하는 linked stack을 만든다.
## 작성할 파일
- `main.c`
## 해야 할 일
`typedef struct { Node *top; size_t size; } Stack;`을 사용한다. 빈 stack을 초기화하고 `stack_push`에서 `malloc(sizeof *node)` 실패와 표현 불가능한 크기를 변경 전에 거부한다. `stack_destroy`의 인자는 NULL이 아니며 ownership과 chain 불변식을 만족하는 살아 있는 writable `Stack`을 가리켜야 하고, 유효한 빈 stack은 허용한다. 종료 전 모든 node를 해제한다.
## 사용할 개념
LIFO ADT, `NULL`-terminated acyclic chain, `SIZE_MAX`, ownership.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o stack_push
```
## 실행 방법
```sh
./stack_push
```
## 예상 관찰 결과
성공한 push 두 번과 정리 후 정확한 출력은 다음과 같다.
```text
empty=1 size=0
top=20 size=2
cleared=1 size=0
```

| 동작 | top 값 | size | 경계 |
|---|---|---:|---|
| 초기 상태 | `NULL` | 0 | 빈 stack |
| push 10 | 10 | 1 | 첫 node |
| push 20 | 20 | 2 | 앞에 연결 |
| 할당 실패 또는 `SIZE_MAX` | 이전 값 유지 | 이전 값 유지 | 변경 없는 실패 |
| destroy | `NULL` | 0 | 소유권 종료 |
## 확인 포인트
`size == 0` iff `top == NULL`이고 nonempty top이 소유한 첫 node이며 chain은 순환 없이 끝난다. `size`는 `top`에서 도달 가능한 live node 수와 같고, non-NULL stack 인자는 모든 연산 진입 전에 이 불변식을 만족한다. 이 stack은 runtime call stack이 아니다.
## 추가 실습
- ★ 두 번 push한 뒤 top과 size를 확인한다.
- ★★ 실패 전후의 불변식과 상태 보존을 설명한다.
- ★★★ 크기가 표현되지 않을 때의 거부 조건을 설명한다.
## 완료 기준
성공 시 LIFO 순서로 top이 갱신되고 실패 시 top·size가 그대로이며 node가 모두 정리된다.
