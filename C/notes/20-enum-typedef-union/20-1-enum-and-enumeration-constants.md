# 20-1. `enum`과 열거 상수

열거형은 서로 관련된 정수 상수에 이름을 주고 하나의 enumeration type으로 묶는다.

## 1. 학습 목표
- enumeration tag, enumerator, enumeration object를 구분한다.
- 생략·명시된 enumerator value 계산 규칙을 설명한다.
- C17 enum과 C++ enum 규칙을 섞지 않는다.

## 2. 선수 지식
Part 2의 정수형, Part 7의 상수 식, Part 19의 tag를 안다.

## 3. 핵심 개념
```c
enum Direction {
    NORTH,
    EAST,
    SOUTH = 10,
    WEST
};
```
`enum`은 keyword, `Direction`은 enumeration tag, `NORTH` 등은 enumerators다. C17에서 각 enumeration constant identifier의 type은 `int`다. `enum Direction`은 별도의 enumerated type이며 “그냥 int의 다른 이름”이 아니다.

## 4. 문법
첫 enumerator에 값이 없으면 0, 이후 생략 값은 직전 enumerator 값에 1을 더한다. 따라서 위 값은 0, 1, 10, 11이다. 값은 연속일 필요가 없고 서로 같은 값도 가질 수 있지만, 같은 `switch` 안에서 같은 값의 `case` label을 중복할 수는 없다.

## 5. 최소 코드 예제
```c
#include <stdio.h>

enum Direction {
    NORTH,
    EAST,
    SOUTH = 10,
    WEST
};

int main(void)
{
    enum Direction direction = WEST;
    printf("%d %d %d\n", NORTH, SOUTH, (int)direction);
    return 0;
}
```

## 6. 코드 해석
`direction`은 `enum Direction` object이고 `WEST`는 type이 `int`인 enumeration constant다. `SOUTH`는 10, 다음 생략 값 `WEST`는 11이다.

## 7. 내부 동작
- **[C17 표준]** 각 distinct enumeration type은 `char`, signed integer type 또는 unsigned integer type 중 하나와 compatible하다. 선택은 implementation-defined이며 모든 enumerator 값을 표현해야 한다.
- 모든 enumerator 값은 C17에서 `int`로 표현 가능해야 한다.
- **[compiler/ABI]** enum object의 compatible type, 크기와 representation을 선택한다. `sizeof(enum Direction)`이 항상 4라는 보장은 없다.

## 8. 자주 하는 실수
- enumerator의 type이 항상 해당 enum type이라고 말한다.
- 모든 enum이 반드시 signed `int`와 compatible하다고 말한다.
- enum object에는 enumerator로 이름 붙인 값만 저장될 수 있다고 단정한다.
- C++의 `enum class`, `Direction::NORTH`, fixed underlying type syntax를 C17에서 사용한다.

## 9. 필수 실습
기본값과 명시값이 섞인 enum을 정의하고 각 값과 enum object를 출력한다.
[20-1 exercise](../../exercises/20-enum-typedef-union/20-1/README.md)

## 10. 추가 실습
- ★ 같은 값을 가진 두 enumerator를 정의한다.
- ★★ 비연속 값의 표를 만든다.
- ★★★ 현재 구현에서 `sizeof(enum Direction)`을 관찰하되 표준 보장과 구분한다.

## 11. 확인 문제
1. `Direction`과 `NORTH`는 각각 무엇인가?
2. C17에서 enumeration constant의 type은?
3. 생략된 첫 값과 다음 값은 어떻게 정해지는가?
4. enum object 크기가 항상 4 byte인가?
5. `Direction::NORTH`가 C17 문법이 아닌 이유는?

## 12. 핵심 정리
- enum은 실제 enumeration type을 선언한다.
- enumerator는 C17에서 type이 `int`인 named integer constant다.
- compatible integer type과 object representation은 구현 선택이다.

## 13. 다음 Step
[20-2. `typedef`](20-2-typedef.md)

## 14. 참고 자료
- N1570 6.2.5, 6.4.4, 6.7.2.2. N1570은 **C11 공개 Committee Draft**이며 관련 enumeration 규칙은 C17에서도 유지된다.
- [cppreference: enum declaration](https://en.cppreference.com/w/c/language/enum)
- [GCC: Code Gen Options (`-fshort-enums`)](https://gcc.gnu.org/onlinedocs/gcc/Code-Gen-Options.html)
