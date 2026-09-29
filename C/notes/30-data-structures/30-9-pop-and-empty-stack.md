# 30-9. `pop`과 빈 stack
## 1. 학습 목표
- status와 output pointer로 빈 stack을 값과 구별한다.
- top을 떼어내며 LIFO와 size 불변식을 유지한다.
## 2. 선수 지식
30-8의 linked stack 표현, `push`, 소유권을 안다.
## 3. 핵심 개념
`pop`은 마지막에 넣은 값을 먼저 반환한다. `-1`도 유효한 `int` 값이므로 실패를 값 sentinel로 표현하지 않는다. `size == 0` iff `top == NULL`; nonempty top은 stack이 소유한 첫 node이고 chain은 acyclic·`NULL`-terminated이다. `size`는 `top`에서 도달 가능한 live node 수와 같으며, non-NULL stack 인자는 모든 연산 진입 전에 이 불변식을 만족해야 한다. 이는 compiler/ABI/runtime call stack과 별개의 LIFO ADT다.
## 4. 문법
```c
static int stack_pop(Stack *stack, int *value);
```
`stack == NULL`, `value == NULL`, 또는 빈 stack이면 0을 반환하고 stack 및 유효한 output object를 변경하지 않는다. 성공하면 살아 있는 writable `int` object에 값을 기록하고 1을 반환한다. output은 stack/node 저장소와 겹치지 않는다는 caller precondition이며 invalid non-NULL pointer는 검사할 수 없다.
## 5. 최소 코드 예제
```c
#include <stdint.h>
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int value;
    struct Node *next;
} Node;

typedef struct {
    Node *top;
    size_t size;
} Stack;

static int stack_push(Stack *stack, int value)
{
    if (stack == NULL || stack->size == SIZE_MAX) {
        return 0;
    }
    Node *node = malloc(sizeof *node);
    if (node == NULL) {
        return 0;
    }
    node->value = value;
    node->next = stack->top;
    stack->top = node;
    ++stack->size;
    return 1;
}

static int stack_pop(Stack *stack, int *value)
{
    if (stack == NULL || value == NULL || stack->top == NULL) {
        return 0;
    }
    Node *old_top = stack->top;
    Node *next = old_top->next;
    int result = old_top->value;
    stack->top = next;
    --stack->size;
    *value = result;
    free(old_top);
    return 1;
}

/* Precondition: stack is non-NULL and points to a live writable Stack
   satisfying its ownership and chain invariant. An empty stack is allowed. */
static void stack_destroy(Stack *stack)
{
    Node *current = stack->top;
    while (current != NULL) {
        Node *next = current->next;
        free(current);
        current = next;
    }
    stack->top = NULL;
    stack->size = 0;
}

int main(void)
{
    Stack stack = {NULL, 0};
    int value = 99;
    if (!stack_push(&stack, 10) || !stack_push(&stack, 20) ||
        !stack_push(&stack, -1)) {
        stack_destroy(&stack);
        return 1;
    }
    while (stack_pop(&stack, &value)) {
        printf("pop=%d size=%zu\n", value, stack.size);
    }
    value = 99;
    int empty_status = stack_pop(&stack, &value);
    printf("empty=%d value=%d\n", empty_status, value);
    if (!stack_push(&stack, 7) || !stack_pop(&stack, &value)) {
        stack_destroy(&stack);
        return 1;
    }
    printf("again=%d size=%zu\n", value, stack.size);
    stack_destroy(&stack);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o stack_pop
./stack_pop
```
## 6. 코드 해석
순서대로 `pop=-1 size=2`, `pop=20 size=1`, `pop=10 size=0`, `empty=0 value=99`, `again=7 size=0`이 출력된다. 실패 뒤 value는 이전 값 그대로다.
## 7. 내부 동작
**[Algorithm]** `pop`은 현재 원소 수 `n`과 관계없이 top 하나만 처리해 시간 `O(1)`, 추가 공간 `O(1)`이다. 마지막 node를 제거하면 top은 `NULL`, size는 0이다.

**[C17]** `old_top->next`와 값을 `free` 전에 보관한다. 성공 시 top과 size를 갱신한 뒤 node lifetime을 끝낸다. output은 살아 있는 별도 객체여야 하며 실패 시 쓰지 않는다.

**[구현 경계]** node lifetime은 allocator 내부의 재사용 시점, OS virtual memory 반환, CPU cache 상태와 구별된다.
## 8. 자주 하는 실수
- `-1`을 빈 stack의 특별한 결과로 취급한다.
- `free(old_top)` 뒤 멤버를 읽거나 마지막 node 제거 뒤 size를 줄이지 않는다.
- 빈 stack이나 NULL output을 역참조한다.
## 9. 필수 실습
여러 번 push, 모두 pop, 빈 상태 pop, 다시 push·pop을 수행한다. [30-9 exercise](../../exercises/30-data-structures/30-9/README.md)
## 10. 추가 실습
- ★ `-1` 값도 정상적으로 pop한다.
- ★★ NULL stack과 NULL output에서 변경 없는 실패를 확인한다.
- ★★★ 전부 pop한 후 다시 push하여 불변식을 검증한다.
## 11. 확인 문제
1. `-1`을 실패 sentinel로 쓰면 무엇이 깨지는가?
2. `free` 전에 저장해야 할 정보는 무엇인가?
3. 마지막 pop 후 top과 size는 무엇인가?
4. 빈 pop은 output을 변경하는가?
5. 모든 값을 꺼낸 뒤 push는 가능한가?
## 12. 핵심 정리
- pop은 status와 별도 output으로 모든 `int` 값을 표현한다.
- 실패 때 상태를 보존하고 성공 때 top·size를 함께 갱신한다.
## 13. 다음 Step
[30-10. `peek`](30-10-peek.md)
## 14. 참고 자료
- N1570 6.2.4, 7.22.3.3: object lifetime과 `free`. N1570은 **C11 공개 Committee Draft**이며 여기의 규칙은 C17에도 적용된다.
