# 30-9 실습: `pop`과 빈 stack
이론: [note](../../../notes/30-data-structures/30-9-pop-and-empty-stack.md)
## 실습 목적
빈 stack을 status로 구별하고 LIFO pop 뒤에도 불변식을 지킨다.
## 작성할 파일
- `main.c`
## 해야 할 일
`typedef struct { Node *top; size_t size; } Stack;`을 사용한다. `stack_pop(Stack *stack, int *value)`는 성공 시 1과 값을 주고, 빈 stack 또는 NULL 인자는 0과 변경 없는 상태를 준다. 여러 값을 push하고 모두 pop한 뒤 다시 push·pop한다. 정리 함수의 인자는 NULL이 아니며 ownership과 chain 불변식을 만족하는 살아 있는 writable `Stack`을 가리켜야 하고, 유효한 빈 stack은 허용한다. 남은 소유 node는 모두 정리한다.
## 사용할 개념
`Node` chain, output pointer, status, `free`, LIFO.
## 컴파일 방법
```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o stack_pop
```
## 실행 방법
```sh
./stack_pop
```
## 예상 관찰 결과
10, 20, -1을 push하면 다음과 같이 출력된다.
```text
pop=-1 size=2
pop=20 size=1
pop=10 size=0
empty=0 value=99
again=7 size=0
```

| 동작 | status / output | top / size | 경계 |
|---|---|---|---|
| 빈 상태 pop | 0 / output 유지 | `NULL` / 0 | 비어 있음 |
| NULL stack 또는 NULL output | 0 / 유효한 output 유지 | 변경 없음 | 인자 |
| -1 pop | 1 / -1 | 다음 node / 2 | 값은 sentinel 아님 |
| 마지막 pop | 1 / 10 | `NULL` / 0 | 마지막 node |
| 다시 push 7, pop | 1 / 7 | `NULL` / 0 | 재사용 |
## 확인 포인트
`next`와 값을 `free` 전 보관한다. non-NULL output은 호출과 반환까지 lifetime이 유지되는 writable `int` 객체를 가리키며 `Stack` 객체나 stack이 소유한 어떤 node와도 겹치지 않는다. `size == 0` iff `top == NULL`; chain은 순환 없이 끝나고 `size`는 `top`에서 도달 가능한 live node 수와 같다. non-NULL stack 인자는 모든 연산 진입 전에 이 불변식을 만족한다.
## 추가 실습
- ★ `-1`이 정상 값임을 확인한다.
- ★★ NULL 및 빈 상태에서 변경이 없음을 확인한다.
- ★★★ 모두 꺼낸 후 다시 push·pop한다.
## 완료 기준
LIFO 출력과 size가 표에 맞고 실패 시 상태·output은 유지되며 소유 node가 남지 않는다.
