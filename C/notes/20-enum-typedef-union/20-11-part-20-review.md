# 20-11. Part 20 종합 복습

Part 20은 enum으로 의미 있는 상태를 만들고, typedef로 type 이름을 정리하며, union과 layout·representation을 C17 경계 안에서 다룬다.

## 1. 학습 목표
- enum, typedef, union의 역할을 종합한다.
- tagged union invariant를 안전하게 유지한다.
- semantic value, layout, bytes, endianness를 구분한다.

## 2. 선수 지식
20-1부터 20-10까지를 학습했다.

## 3. 핵심 개념
enum은 named integer constants와 enumeration type을 제공하고, typedef는 alias를 제공하며, union은 shared storage를 제공한다. tagged union은 이 요소들을 structure로 조합한 program convention이다.

## 4. 문법
```c
typedef enum ValueKind { VALUE_INT, VALUE_DOUBLE } ValueKind;
union ValueData { int i; double d; };
struct Value { ValueKind kind; union ValueData data; };
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

typedef enum ValueKind { VALUE_INT, VALUE_DOUBLE } ValueKind;
union ValueData { int i; double d; };
struct Value { ValueKind kind; union ValueData data; };

static void print_value(const struct Value *value)
{
    switch (value->kind) {
    case VALUE_INT: printf("int:%d\n", value->data.i); break;
    case VALUE_DOUBLE: printf("double:%.1f\n", value->data.d); break;
    default: puts("invalid"); break;
    }
}

int main(void)
{
    struct Value value = {VALUE_DOUBLE, {.d = 2.5}};
    print_value(&value);
    return 0;
}
```

## 6. 코드 해석
typedef alias를 사용해 enum tag를 간결하게 쓰고, tagged union object를 일관되게 초기화한 뒤 matching member를 출력한다.

## 7. 내부 동작
- **[C17 표준]** enumerators는 `int` constants이며 enum은 implementation-selected integer type과 compatible하다.
- union members는 storage를 공유하고, struct/union에는 padding이 있을 수 있다.
- character type으로 bytes를 관찰할 수 있지만 bytes가 semantic equality나 portable serialization을 의미하지는 않는다.
- **[compiler/ABI/CPU]** exact size, alignment, offsets, representation, endianness를 결정한다.
inactive union member를 읽는 type-punning은 object representation 재해석이며 portable numeric conversion이 아니고 trap representation 위험도 있어 실행 실습에서 제외한다.

## 8. 자주 하는 실수
- enum을 `int`와 완전히 동일시하거나 항상 4 bytes라고 한다.
- C++ enum class·scoped syntax·C23 fixed underlying type 문법을 C17에 쓴다.
- union values가 동시에 보존된다고 생각한다.
- tag/member invariant를 compiler가 자동 보장한다고 생각한다.
- raw `memcmp`와 raw file write를 semantic comparison·serialization으로 사용한다.

## 9. 필수 실습
enum state, typedef alias, tagged union, layout과 native byte order 관찰을 한 프로그램의 독립 함수들로 복습한다.
[20-11 exercise](../../exercises/20-enum-typedef-union/20-11/README.md)

## 10. 추가 실습
- ★ 핵심 용어 표를 만든다.
- ★★ C17 보장과 구현 선택을 분류한다.
- ★★★ tagged union API contract를 작성한다.

## 11. 확인 문제
1. enumeration constant의 C17 type은?
2. typedef와 distinct type의 차이는?
3. struct와 union storage 차이는?
4. tagged union invariant는?
5. `sizeof`·`_Alignof`·`offsetof` 차이는?
6. `unsigned char` byte 관찰의 한계는?
7. native byte-order 관찰을 C17 보장으로 일반화할 수 있는가?

## 12. 핵심 정리
- named values, type aliases, shared storage를 목적에 맞게 구분한다.
- tag를 확인한 뒤 union member를 읽는다.
- C17 semantics와 implementation representation을 분리한다.

## 13. 다음 Step
커리큘럼의 다음 Step은 **21-1. `&`**다. Part 21 파일은 만들지 않는다.

## 14. 참고 자료
- N1570 6.2.6.1, 6.2.8, 6.5.2.3, 6.5.3.4, 6.7.2, 6.7.8, 6.7.9, 7.19. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: enum](https://en.cppreference.com/w/c/language/enum)
- [cppreference: union](https://en.cppreference.com/w/c/language/union)
- [cppreference: object](https://en.cppreference.com/w/c/language/object)
