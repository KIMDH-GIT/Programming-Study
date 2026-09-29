# 30-3. linked list 순회·검색
## 1. 학습 목표
- `const Node *`로 목록을 변경하지 않고 순회한다.
- 빈 목록 및 첫·중간·마지막·미발견을 검색한다.
- 노드 수에 따른 실행 시간을 설명한다.
## 2. 선수 지식
30-1의 연결 표현과 30-2의 객체 lifetime을 안다.
## 3. 핵심 개념
**자료구조**는 `next` 연결이고, **ADT**의 검색 계약은 찾은 node를 반환하거나 없으면 `NULL`을 반환하는 것이다. **C17 표현**에서는 `const Node *`를 사용해 함수가 node의 값을 수정하지 않게 한다. 반환된 포인터는 새 소유권을 만들지 않으며 원래 node가 살아 있는 동안에만 유효하다. 목록은 비순환, NULL 종료, sentinel 없음이 호출 전제다.
## 4. 문법
```c
static const Node *find(const Node *head, int target);
```
구조체 본문에는 미완성 타입 포인터 `struct Node *next`를 선언하고, typedef `Node`와 `sizeof(Node)`는 선언 완료 뒤에만 사용할 수 있다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

typedef struct Node {
    int value;
    struct Node *next;
} Node;

static const Node *find(const Node *head, int target)
{
    for (const Node *current = head; current != NULL;
         current = current->next) {
        if (current->value == target) {
            return current;
        }
    }
    return NULL;
}

int main(void)
{
    Node last = {30, NULL};
    Node middle = {20, &last};
    Node first = {10, &middle};

    printf("empty=%d first=%d middle=%d last=%d absent=%d\n",
           find(NULL, 10) != NULL, find(&first, 10) != NULL,
           find(&first, 20) != NULL, find(&first, 30) != NULL,
           find(&first, 99) != NULL);
    return 0;
}
```
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o search
./search
```
## 6. 코드 해석
출력은 `empty=0 first=1 middle=1 last=1 absent=0`이다. `current != NULL`을 확인한 다음에만 `current->value`와 `current->next`를 읽는다.
## 7. 내부 동작
**[자료구조]** 첫 node부터 `next`를 따라가며 일치 시 반환하고 끝까지 없으면 `NULL`을 반환한다.

**[ADT]** 성공 결과는 기존 node의 읽기 전용 참조이며 소유권 이전이 아니다. 현재 node 수를 `n`이라 하면 최악의 순회·검색 시간은 `O(n)`, 보조 공간은 `O(1)`이다.

**[C17]** `const`는 이 포인터를 통한 변경을 막는다. node의 lifetime이 끝나면 검색 결과도 무효다. 포인터 추적과 cache 효과는 CPU 구현 성능 영역이지 C17이 보장하는 시간이나 저장 위치가 아니다.
## 8. 자주 하는 실수
- `current == NULL` 상태에서 `current->next`를 읽는다.
- 검색 결과를 소유한 것으로 여기고 `free`한다.
- 주소가 규칙적으로 증가한다고 가정하고 산술로 다음 node를 찾는다.
## 9. 필수 실습
빈 목록과 첫·중간·마지막·없는 값을 검사한다. [30-3 exercise](../../exercises/30-data-structures/30-3/README.md)
## 10. 추가 실습
- ★ 방문한 node의 수를 센다.
- ★★ 한 node 목록에서 일치·미발견을 비교한다.
- ★★★ 중복 값은 처음 만난 node를 반환함을 설명한다.
## 11. 확인 문제
1. 빈 목록에서는 loop가 몇 번 실행되는가?
2. 미발견 결과를 무엇으로 표현하는가?
3. 반환된 포인터는 언제 유효한가?
4. `O(n)`에서 `n`은 무엇인가?
5. cache 효과가 C17의 규칙이 아닌 이유는?
## 12. 핵심 정리
- 읽기 전용 탐색은 끝의 `NULL`까지 진행한다.
- 반환 포인터는 새 소유권이 아니며 최악의 시간은 `O(n)`이다.
## 13. 다음 Step
[30-4. linked list insert](30-4-linked-list-insert.md)
## 14. 참고 자료
- N1570 6.5.3.2, 6.7.2.1. N1570은 **C11 공개 Committee Draft**이며 관련 포인터·구조체 규칙은 C17에서도 유지된다.
- Sedgewick and Wayne, *Algorithms*, 4th ed., linked lists.
