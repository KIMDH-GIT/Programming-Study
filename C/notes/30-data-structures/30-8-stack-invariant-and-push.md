# 30-8. stack 불변식과 `push`
## 1. 학습 목표
- linked stack의 LIFO 규칙과 표현 불변식을 구별한다.
- 할당 실패에도 구조를 유지하는 `stack_push`를 작성한다.
## 2. 선수 지식
30-7의 node 소유권과 안전한 정리, `size_t`와 `malloc`을 안다.
## 3. 핵심 개념
stack은 마지막에 넣은 값을 먼저 꺼내는 LIFO 추상 자료형(ADT)이다. 여기서는 `top`부터 시작하는 단방향 node chain으로 표현한다. `size == 0` iff `top == NULL`, nonempty `top`은 stack이 소유한 첫 node이며 chain은 순환 없이 `NULL`로 끝난다. `size`는 `top`에서 도달 가능한 live node 수와 같으며, non-NULL stack 인자는 모든 연산 진입 전에 이 불변식을 만족해야 한다. 이 자료구조 stack은 compiler/ABI/runtime이 함수 호출을 처리하는 call stack과 다르다.
## 4. 문법
```c
typedef struct {
    Node *top;
    size_t size;
} Stack;
static int stack_push(Stack *stack, int value);
```
초기화는 `(Stack){NULL, 0}`이다. `stack == NULL`이면 status 0. 유효한 불변식을 만족하는 stack에서 `size == SIZE_MAX`면 크기를 표현할 수 없으므로 변경 없이 0. 할당 실패도 변경 없이 0, 성공은 1이다. 다른 alias는 node를 소유하지 않는다.
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
    printf("empty=%d size=%zu\n", stack.top == NULL, stack.size);
    if (!stack_push(&stack, 10) || !stack_push(&stack, 20)) {
        stack_destroy(&stack);
        return 1;
    }
    printf("top=%d size=%zu\n", stack.top->value, stack.size);
    stack_destroy(&stack);
    printf("cleared=%d size=%zu\n", stack.top == NULL, stack.size);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o stack_push
./stack_push
```
## 6. 코드 해석
출력은 `empty=1 size=0`, `top=20 size=2`, `cleared=1 size=0` 순이다. 실패 경로까지 현재 소유한 node를 모두 해제한다.
## 7. 내부 동작
**[Algorithm]** 새 node를 기존 top 앞에 연결하므로 `push`는 현재 원소 수 `n`에 관계없이 시간 `O(1)`, node 하나의 추가 공간 `O(1)`이다. 성공 전후에 `size == 0` iff `top == NULL`이다.

**[C17]** `malloc(sizeof *node)`가 유효한 객체를 반환한 뒤에만 멤버를 쓴다. `size == SIZE_MAX`를 먼저 확인하여 `size_t` 증가가 표현 범위를 넘지 않게 한다. `free` 뒤에는 node lifetime이 끝난다.

**[구현 경계]** allocation 내부 구현, OS virtual memory와 CPU cache, 함수 호출의 ABI stack frame은 이 linked stack의 규칙과 별개다.
## 8. 자주 하는 실수
- `malloc` 실패 전에 `top` 또는 `size`를 변경한다.
- `top`만 갱신하고 `size`를 갱신하지 않는다.
- 이 linked stack을 실행 환경의 call stack과 동일시한다.
## 9. 필수 실습
빈 stack에서 두 번 push한 후 정리한다. [30-8 exercise](../../exercises/30-data-structures/30-8/README.md)
## 10. 추가 실습
- ★ push 후 top과 size를 확인한다.
- ★★ 할당 실패 때 상태가 유지되는 이유를 설명한다.
- ★★★ `SIZE_MAX`에서 거부해야 하는 이유를 서술한다.
## 11. 확인 문제
1. LIFO는 어떤 연산 순서를 뜻하는가?
2. 빈 stack의 `top`과 `size`는 각각 무엇인가?
3. 실패 경로에서 무엇이 바뀌지 않아야 하는가?
4. `push`의 `n`과 시간 복잡도는 무엇인가?
5. call stack과 이 자료구조는 왜 별개인가?
## 12. 핵심 정리
- 새 node는 항상 기존 top 앞에 붙인다.
- 성공 시에만 top과 size가 함께 바뀌며 실패 시에는 둘 다 그대로다.
## 13. 다음 Step
[30-9. `pop`과 빈 stack](30-9-pop-and-empty-stack.md)
## 14. 참고 자료
- N1570 6.2.4, 7.22.3.4: 객체 lifetime과 `malloc`. N1570은 **C11 공개 Committee Draft**이며 여기의 규칙은 C17에도 적용된다.
