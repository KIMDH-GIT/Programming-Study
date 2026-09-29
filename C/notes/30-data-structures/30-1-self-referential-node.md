# 30-1. 자기 참조 구조체 `Node`
## 1. 학습 목표
- `Node`와 다음 node를 가리키는 포인터를 선언한다.
- 자료구조, 추상 자료형(ADT), C17 표현을 구분한다.
- 빈 목록과 끝을 `NULL`로 나타낸다.
## 2. 선수 지식
구조체, 포인터, `NULL`, 객체의 lifetime을 안다.
## 3. 핵심 개념
**자료구조**는 값을 연결해 저장하는 구체적인 구성이고, **ADT**는 검색·삽입·삭제 같은 연산의 외부 계약이다. **C17 표현**은 여기서 `Node` 객체의 `value`와 `next`, 첫 node를 가리키는 `Node *head`다. 이 Part의 목록은 순환하지 않고 마지막 `next`가 `NULL`이며 별도의 sentinel node가 없다. 빈 목록은 `head == NULL`이다.

`struct Node *`는 구조체 본문에서 아직 완성되지 않은 타입을 가리키는 포인터로 사용할 수 있다. `Node`라는 typedef 이름과 `sizeof(Node)`는 선언이 끝나 타입이 완성된 뒤에 사용한다. 구조체 안에 `struct Node next;`처럼 미완성 객체 자체를 넣을 수는 없다.
## 4. 문법
```c
typedef struct Node {
    int value;
    struct Node *next;
} Node;
```
`next`는 다음 객체의 주소 또는 `NULL`이다. 포인터의 바이트 수나 객체들이 메모리에 연속 또는 분산 배치된다는 가정은 하지 않는다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

typedef struct Node {
    int value;
    struct Node *next;
} Node;

int main(void)
{
    Node last = {20, NULL};
    Node first = {10, &last};
    Node *head = &first;

    printf("empty=%d first=%d last=%d end=%d\n",
           head == NULL, head->value, head->next->value,
           head->next->next == NULL);
    return 0;
}
```
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o node
./node
```
## 6. 코드 해석
출력은 `empty=0 first=10 last=20 end=1`이다. `first.next`는 `last`를 가리키고 `last.next`는 끝을 표시한다. `first`와 `last`는 `main`이 끝날 때까지 살아 있는 자동 저장 기간 객체다.
## 7. 내부 동작
**[자료구조]** `head`에서 `next`를 따르면 순서대로 두 값을 만난다. 유효한 목록에서는 각 node를 최대 한 번 방문하고 `NULL`에서 끝난다.

**[ADT]** 앞으로의 연산은 이 연결 관계에 대해 정의한다. 특정 메모리 배치는 계약이 아니다.

**[C17]** 아직 미완성인 `struct Node`의 포인터 멤버는 선언할 수 있지만 그 객체를 값 멤버로 내장하거나 완성 전에 크기를 구하지 않는다. 포인터를 역참조할 때는 가리키는 객체가 살아 있어야 한다.
## 8. 자주 하는 실수
- `typedef` 이름을 구조체 본문에서 이미 선언된 것처럼 쓴다.
- 끝에서 `NULL`을 확인하지 않고 `next`를 역참조한다.
- 두 node가 메모리에서 이웃한다고 가정한다.
## 9. 필수 실습
빈 목록과 하나·여러 node의 연결 및 종료 조건을 확인한다. [30-1 exercise](../../exercises/30-data-structures/30-1/README.md)
## 10. 추가 실습
- ★ 한 node만 있는 목록을 표시한다.
- ★★ 세 node를 연결하고 각 주소 대신 값만 확인한다.
- ★★★ 잘못된 종료 링크가 순회를 끝내지 못하는 이유를 설명한다.
## 11. 확인 문제
1. 본문 안에서 `struct Node *`가 가능한 이유는?
2. `Node` typedef와 `sizeof(Node)`는 언제 사용할 수 있는가?
3. 빈 목록과 마지막 node의 끝은 각각 어떻게 표현하는가?
4. 자료구조와 ADT의 차이는 무엇인가?
5. 포인터 값만으로 메모리 배치를 추측할 수 없는 이유는?
## 12. 핵심 정리
- `next`는 살아 있는 다음 node 또는 `NULL`이다.
- 연결의 의미와 C17의 객체 표현을 분리한다.
## 13. 다음 Step
[30-2. node 생성과 소유권](30-2-node-creation-and-ownership.md)
## 14. 참고 자료
- N1570 6.2.5, 6.7.2.1. N1570은 **C11 공개 Committee Draft**이며 관련 미완성 타입·구조체 규칙은 C17에서도 유지된다.
- [cppreference: struct declaration](https://en.cppreference.com/w/c/language/struct)
