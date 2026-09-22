# 19-2. 구조체 선언·초기화와 `.`

구조체 object는 다른 object처럼 선언·초기화하며 `.` operator로 member object를 선택한다.

## 1. 학습 목표
- positional, partial, designated initialization을 구분한다.
- `object.member`로 member를 읽고 수정한다.
- structure assignment와 equality의 차이를 설명한다.

## 2. 선수 지식
19-1의 structure definition과 Part 7의 assignment를 안다.

## 3. 핵심 개념
`struct Point p = {10, 20};`의 값은 member 이름이 아니라 declaration 순서에 대응한다. `p.x = 30;`은 `p.x`가 지정하는 member object에 일반 assignment를 수행한다.

## 4. 문법
```c
struct Point a = {1, 2};
struct Point b = {10};       /* y는 0으로 초기화 */
struct Point c = {.y = 20};  /* x는 0으로 초기화 */
b = a;                       /* compatible structure assignment */
```
지정되지 않은 member는 aggregate initialization 규칙에 따라 static storage duration object와 같은 방식으로 초기화된다. 우연한 memory 값이 아니다. C99의 designated initializer는 C17에서도 사용할 수 있다.

## 5. 최소 코드 예제
```c
#include <stdio.h>

struct Point {
    int x;
    int y;
};

int main(void)
{
    struct Point a = {10, 20};
    struct Point b = {.y = 5};
    b = a;
    b.x = 30;
    printf("a=(%d,%d) b=(%d,%d)\n", a.x, a.y, b.x, b.y);
    return 0;
}
```

## 6. 코드 해석
`b = a`는 compatible structure value를 대입한다. 이후 `b.x`만 바꾸므로 `a`는 그대로다. 구조체 assignment는 array member도 포함하지만 독립 배열의 `array1 = array2`를 허용하지는 않는다.

## 7. 내부 동작
- **[C17 표준]** structure assignment의 의미는 value/member 값의 assignment다. padding byte까지 반드시 `memcpy`처럼 복사하라고 요구하지 않는다.
- 일반 structure object에는 built-in `==`·`!=`가 없다. equality가 필요하면 의미 있는 members를 비교한다.
- **[compiler]** register, load/store, library call 등 동등한 구현을 선택할 수 있다.

## 8. 자주 하는 실수
- positional initializer가 이름으로 자동 matching된다고 생각한다.
- partial initialization의 나머지가 indeterminate라고 생각한다.
- 구조체 전체 대입이 배열처럼 불가능하다고 생각한다.
- `a == b`가 member별 비교를 한다고 생각한다.
- `memcmp`를 portable value equality로 사용한다. padding과 object representation 때문에 일반 해법이 아니다.

## 9. 필수 실습
두 `struct Point`를 positional/partial 초기화하고 전체 assignment 뒤 한 member만 수정한다.
[19-2 exercise](../../exercises/19-structures/19-2/README.md)

## 10. 추가 실습
- ★ designated initializer의 순서를 바꾼다.
- ★★ partial initialization 결과를 출력한다.
- ★★★ member별 equality 함수를 작성한다.

## 11. 확인 문제
1. `{10, 20}`은 어떤 순서로 대응하는가?
2. `{10}`에서 두 번째 `int` member 값은?
3. `p.x = 30`은 무엇을 대입하는가?
4. compatible structure끼리 전체 assignment가 가능한가?
5. 일반 structure끼리 `==`를 쓸 수 있는가?
6. `memcmp`가 value equality를 보장하지 않는 이유는?

## 12. 핵심 정리
- `.`는 structure object의 member를 선택한다.
- partial/designated initialization의 생략 member는 규칙에 따라 초기화된다.
- structure assignment는 가능하지만 built-in structure equality는 없다.

## 13. 다음 Step
[19-3. `typedef struct`](19-3-typedef-struct.md)

## 14. 참고 자료
- N1570 6.5.16.1, 6.7.9. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: initialization](https://en.cppreference.com/w/c/language/struct_initialization)
- [cppreference: member access](https://en.cppreference.com/w/c/language/operator_member_access)
