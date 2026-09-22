# 20-2. `typedef`

`typedef` declaration은 기존 type에 새 typedef name을 붙인다. 새 runtime object나 별도의 distinct type을 만들지 않는다.

## 1. 학습 목표
- typedef name과 tag를 구분한다.
- enum, struct, scalar type에 alias를 선언한다.
- alias가 type semantics를 바꾸지 않음을 설명한다.

## 2. 선수 지식
19-3의 `typedef struct`, 20-1의 enum tag를 안다.

## 3. 핵심 개념
```c
typedef enum Direction Direction;
```
`enum Direction`은 tag가 붙은 type specifier이고 `Direction`은 ordinary identifier namespace의 typedef name이다.

## 4. 문법
```c
enum Direction { NORTH, SOUTH };
typedef enum Direction Direction;

typedef unsigned long Counter;
```
tag 없는 `typedef enum { ... } Direction;`도 가능하지만 tag가 없으므로 이후 `enum Direction` 표기는 존재하지 않는다.

## 5. 최소 코드 예제
```c
#include <stdio.h>

enum Direction { NORTH, SOUTH };
typedef enum Direction Direction;
typedef unsigned long Counter;

int main(void)
{
    Direction direction = SOUTH;
    Counter moves = 3UL;
    printf("%d %lu\n", (int)direction, moves);
    return 0;
}
```

## 6. 코드 해석
`Direction`은 `enum Direction`의 synonym이고 `Counter`는 `unsigned long`의 synonym이다. alias 이전과 이후 type identity는 같다.

## 7. 내부 동작
- **[C17 표준]** typedef name은 ordinary identifier namespace에, enum/struct/union tag는 tag namespace에 속한다.
- typedef declaration 자체는 storage나 machine instruction을 생성하지 않는다.
- enumerator identifiers도 ordinary identifiers이므로 같은 scope의 typedef name과 충돌할 수 있다.

## 8. 자주 하는 실수
- typedef가 새로운 incompatible type을 만든다고 생각한다.
- `typedef enum` 전체를 enum을 만드는 하나의 특수 문법이라고 생각한다.
- tag 없는 enum에 `enum Direction`을 사용할 수 있다고 생각한다.
- alias로 pointer·ownership 의미를 숨긴다.

## 9. 필수 실습
named enum에 typedef alias를 붙이고 tag 표기와 alias 표기를 함께 사용한다.
[20-2 exercise](../../exercises/20-enum-typedef-union/20-2/README.md)

## 10. 추가 실습
- ★ scalar alias를 만든다.
- ★★ tag 없는 enum typedef와 named enum typedef를 비교한다.
- ★★★ 각 이름이 속한 namespace를 표로 정리한다.

## 11. 확인 문제
1. typedef는 distinct type을 만드는가?
2. tag와 typedef name은 같은 namespace인가?
3. tag 없는 enum typedef에서 `enum Direction`을 쓸 수 있는가?
4. typedef가 runtime cost를 만드는가?

## 12. 핵심 정리
- typedef는 기존 type의 alias다.
- enum definition과 typedef declaration은 서로 다른 역할이다.
- tag와 ordinary identifier namespace를 구분한다.

## 13. 다음 Step
[20-3. enum 기반 state 표현](20-3-enum-based-state.md)

## 14. 참고 자료
- N1570 6.2.3, 6.7.8. N1570은 **C11 공개 Committee Draft**이며 관련 typedef 규칙은 C17에서도 유지된다.
- [cppreference: typedef declaration](https://en.cppreference.com/w/c/language/typedef)
- [cppreference: name spaces](https://en.cppreference.com/w/c/language/name_space)
