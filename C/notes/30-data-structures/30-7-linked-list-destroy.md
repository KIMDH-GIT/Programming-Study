# 30-7. linked list destroy
## 1. 학습 목표
- `next`를 `free` 전에 보관하여 모든 node를 정확히 한 번 해제한다.
- 호출자 `head`를 `NULL`로 되돌려 반복 destroy를 안전하게 한다.
## 2. 선수 지식
18장의 `malloc`·`free`, 30-6의 `Node`와 순회를 안다.
## 3. 핵심 개념
동적으로 할당된 chain을 소유한 호출자는 더 이상 쓰지 않을 때 모든 node의 소유권을 끝내야 한다. node를 잃고 해제하지 못하면 memory leak이고, 해제된 node를 가리키던 pointer를 사용하면 lifetime이 끝난 객체에 접근하게 된다. 여기서는 호출자의 head 자체도 `NULL`로 만든다. 별도 복사해 둔 pointer가 자동으로 `NULL`이 되는 것은 아니며, 대상 lifetime이 끝나면 그 pointer value는 indeterminate가 되므로 사용하면 안 된다.
## 4. 문법
```c
static void list_destroy(Node **head);
```
`head == NULL`은 아무 일도 하지 않는다. 그 외에는 살아 있고 순환하지 않는, `malloc`으로 각각 얻은 node만으로 된 chain의 유일한 소유자 `*head`를 받는다. 해제 후 `*head == NULL`이며 같은 head로 다시 호출해도 안전하다.
## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int value;
    struct Node *next;
} Node;

static void list_destroy(Node **head)
{
    if (head == NULL) {
        return;
    }
    Node *current = *head;
    while (current != NULL) {
        Node *next = current->next;
        free(current);
        current = next;
    }
    *head = NULL;
}

int main(void)
{
    Node *head = NULL;
    list_destroy(&head);
    for (int value = 1; value <= 3; ++value) {
        Node *node = malloc(sizeof *node);
        if (node == NULL) {
            list_destroy(&head);
            return 1;
        }
        node->value = value;
        node->next = head;
        head = node;
    }
    printf("before=%d,%d,%d\n",
           head->value, head->next->value, head->next->next->value);
    list_destroy(&head);
    list_destroy(&head);
    printf("after=%d\n", head == NULL);

    Node *single = malloc(sizeof *single);
    if (single == NULL) {
        return 1;
    }
    single->value = 7;
    single->next = NULL;
    list_destroy(&single);
    printf("single=%d\n", single == NULL);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o list_destroy
./list_destroy
```
## 6. 코드 해석
정상 할당 시 출력은 `before=3,2,1`, `after=1`, `single=1` 순이다. 할당 실패 시 그때까지 소유한 node를 정리하고 종료 상태 1을 반환한다.
## 7. 내부 동작
**[Algorithm]** 한 번에 한 node를 떼어내며 `n`개 node에 시간 `O(n)`, 추가 공간 `O(1)`이다.

**[C17]** `free` 후 그 node의 lifetime은 끝나므로 `current->next`를 읽을 수 없다. 해제 전에 저장한 `next`로만 진행한다. `*head = NULL`은 호출자의 pointer object를 갱신하며 다른 alias를 고쳐 주지는 않는다.

**[구현 경계]** C17의 object lifetime과 pointer 사용 규칙은 allocator 내부의 free-list, OS virtual memory 회수 시점, CPU cache의 잔존 값과 별개다.
## 8. 자주 하는 실수
- `free(current)` 뒤 `current->next`를 읽는다.
- head만 해제하여 뒤 node를 누수시키거나 같은 node를 두 번 해제한다.
- `Node *head`만 전달하여 호출자 pointer가 그대로 남게 한다.
## 9. 필수 실습
빈·단일·여러 node와 연속 두 번 destroy를 확인한다. [30-7 exercise](../../exercises/30-data-structures/30-7/README.md)
## 10. 추가 실습
- ★ node가 하나뿐인 경우를 다시 확인한다.
- ★★ 부분 할당 실패 시 생성된 node를 정리한다.
- ★★★ 소유하지 않는 alias가 남은 경우의 사용 금지 계약을 기술한다.
## 11. 확인 문제
1. `next`를 언제 저장해야 하는가?
2. memory leak과 dangling pointer 사용은 어떻게 다른가?
3. `Node **head`를 쓰는 이유는?
4. 두 번째 destroy가 안전한 이유는?
## 12. 핵심 정리
- 각 node를 정확히 한 번 해제하고 head를 `NULL`로 만든다.
- 해제된 객체에 접근하지 않으며 소유권은 destroy에서 종료된다.
## 13. 다음 Step
[30-8. stack 불변식과 `push`](30-8-stack-invariant-and-push.md)
## 14. 참고 자료
- N1570 6.2.4, 7.22.3.3: 객체 lifetime과 `free`. N1570은 **C11 공개 Committee Draft**이며 여기의 규칙은 C17에도 적용된다.
