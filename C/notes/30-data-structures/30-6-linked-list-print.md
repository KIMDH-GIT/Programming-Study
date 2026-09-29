# 30-6. linked list print
## 1. 학습 목표
- `const Node *`로 단방향 linked list를 변경하지 않고 순회한다.
- 빈 list와 출력 형식을 호출자가 확인할 수 있게 정의한다.
## 2. 선수 지식
Part 14·17의 pointer와 `const`, 30-1~30-5의 `Node`와 연결을 안다.
## 3. 핵심 개념
`head`는 첫 node를 가리키고 마지막 node의 `next`는 `NULL`이다. 빈 list는 `head == NULL`이다. 출력은 값의 순서로 검증한다. 주소의 숫자값은 실행마다 달라질 수 있어 정답의 근거가 아니다.
## 4. 문법
```c
static void list_print(const Node *head);
```
호출자는 살아 있는, 순환하지 않는 `Node` chain을 넘긴다. 함수는 node를 소유하거나 변경하지 않는다. 빈 list의 정확한 출력은 `[]\n`이고, 일반 list는 `[값, 값]\n`이다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

typedef struct Node {
    int value;
    struct Node *next;
} Node;

static void list_print(const Node *head)
{
    const Node *current = head;
    printf("[");
    while (current != NULL) {
        printf("%d", current->value);
        current = current->next;
        if (current != NULL) {
            printf(", ");
        }
    }
    printf("]\n");
}

int main(void)
{
    Node third = {30, NULL};
    Node second = {20, &third};
    Node first = {10, &second};

    list_print(NULL);
    list_print(&first);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o list_print
./list_print
```
## 6. 코드 해석
출력은 첫 줄 `[]`, 둘째 줄 `[10, 20, 30]`이다. 이 예제의 node는 자동 저장 기간 객체이므로 `free`하지 않는다.
## 7. 내부 동작
**[Algorithm]** 현재 node를 출력한 뒤 `next`를 따라가므로 `n`개 node에서 시간 `O(n)`, 추가 공간 `O(1)`이다.

**[C17]** `const Node *`를 통한 멤버 변경은 허용되지 않지만 객체가 ROM에 놓인다는 뜻은 아니다. `first`, `second`, `third`의 lifetime은 `main` 블록이 끝날 때 종료된다. `NULL` 검사 뒤에만 멤버를 읽는다.

**[구현 경계]** allocator의 배치, OS virtual memory, CPU cache의 동작은 이 출력 순서 계약의 근거가 아니다.
## 8. 자주 하는 실수
- `head == NULL`인데 `head->value`를 읽는다.
- 호출자의 유일한 owning `head`를 다음 node로 덮어써 앞 node를 잃는다. 함수의 by-value 지역 복사본을 전진시키는 것 자체는 caller의 `head`를 바꾸지 않는다.
- 마지막 값 뒤에도 구분자를 출력하거나 주소 숫자로 순서를 판단한다.
## 9. 필수 실습
빈 list, 하나, 세 node의 출력 형식을 비교한다. [30-6 exercise](../../exercises/30-data-structures/30-6/README.md)
## 10. 추가 실습
- ★ 음수와 중복 값으로 순서를 검증한다.
- ★★ 출력 함수 호출 전후의 연결 상태가 같은지 확인한다.
- ★★★ 출력 대상 stream을 명시하는 별도 인터페이스를 설계한다.
## 11. 확인 문제
1. 빈 list는 정확히 무엇을 출력하는가?
2. `const Node *`가 막는 변경은 무엇인가?
3. 마지막 구분자를 피하는 조건은 무엇인가?
4. 실행마다 다른 주소를 정답으로 삼지 않는 이유는?
## 12. 핵심 정리
- 살아 있는 node를 `NULL`까지 읽으며 값 순서만 출력한다.
- 읽기 전용 순회는 list 소유권을 가져가지 않는다.
## 13. 다음 Step
[30-7. linked list destroy](30-7-linked-list-destroy.md)
## 14. 참고 자료
- N1570 6.2.4, 6.7.3: 객체 lifetime과 `const` 규칙. N1570은 **C11 공개 Committee Draft**이며 여기에서 쓰는 규칙은 C17에도 적용된다.
