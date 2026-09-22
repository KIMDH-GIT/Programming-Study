# 20-6. tag와 union을 함께 쓰기

tagged union은 enum tag와 union storage를 structure에 함께 넣어 현재 의미 있는 variant를 명시하는 programming pattern이다.

## 1. 학습 목표
- enum tag와 union member의 일치 invariant를 유지한다.
- tag를 먼저 검사한 뒤 맞는 member를 읽는다.
- C가 invariant를 자동 검사하지 않음을 설명한다.

## 2. 선수 지식
20-3 enum state, 20-5 union shared storage, Part 19 구조체를 안다.

## 3. 핵심 개념
```c
enum ValueKind { VALUE_INT, VALUE_DOUBLE };
union ValueData { int i; double d; };
struct Value { enum ValueKind kind; union ValueData data; };
```
`kind == VALUE_INT`이면 `data.i`, `kind == VALUE_DOUBLE`이면 `data.d`가 의미 있다는 invariant를 프로그램이 유지한다.

## 4. 문법
named union type과 named structure member를 사용한다. anonymous union이나 pointer ownership을 추가하지 않는다.

## 5. 최소 코드 예제
```c
#include <stdio.h>

enum ValueKind { VALUE_INT, VALUE_DOUBLE };
union ValueData { int i; double d; };
struct Value { enum ValueKind kind; union ValueData data; };

static void print_value(const struct Value *value)
{
    switch (value->kind) {
    case VALUE_INT: printf("%d\n", value->data.i); break;
    case VALUE_DOUBLE: printf("%.1f\n", value->data.d); break;
    default: puts("invalid tag"); break;
    }
}

int main(void)
{
    struct Value a = {VALUE_INT, {.i = 10}};
    struct Value b = {VALUE_DOUBLE, {.d = 3.5}};
    print_value(&a);
    print_value(&b);
    return 0;
}
```

## 6. 코드 해석
각 object initializer가 tag와 matching member를 함께 설정한다. 출력 함수는 const pointer의 tag를 검사한 뒤 해당 member만 읽는다.

## 7. 내부 동작
- **[C17 표준]** enum, struct, union의 개별 access·initialization 규칙을 제공한다.
- tag/member 관계 자체는 언어가 강제하는 기능이 아니라 program invariant다.
- compatible union objects 사이 assignment는 가능하지만 member가 pointer라면 pointed object를 deep-copy하지 않는다.

## 8. 자주 하는 실수
- tag와 다른 member를 읽는다.
- C가 tag/member 일치를 자동 검사한다고 생각한다.
- inactive member 읽기를 일반적인 portable type conversion으로 사용한다.
- raw bytes나 `memcmp`로 semantic equality를 판단한다.

## 9. 필수 실습
정수와 실수 variants를 생성하고 const 출력 함수에서 tag별 member를 읽는다.
[20-6 exercise](../../exercises/20-enum-typedef-union/20-6/README.md)

## 10. 추가 실습
- ★ constructor 함수를 두 개 만든다.
- ★★ invalid tag 경로를 실행 없이 분석한다.
- ★★★ 두 tagged values를 semantic하게 비교한다.

## 11. 확인 문제
1. tagged union invariant는?
2. member보다 tag를 먼저 검사하는 이유는?
3. C compiler가 invariant를 자동 강제하는가?
4. union assignment는 automatic deep copy인가?
5. `memcmp`가 semantic equality가 아닌 이유는?

## 12. 핵심 정리
- enum tag가 union storage의 해석을 설명한다.
- tag와 member를 함께 설정하고 함께 검사한다.
- named union과 const pointer로 의도를 명시한다.

## 13. 다음 Step
[20-7. `sizeof`, `_Alignof`, `offsetof`](20-7-sizeof-alignof-offsetof.md)

## 14. 참고 자료
- N1570 6.5.2.3, 6.5.16.1, 6.7.2.1, 6.7.9. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: union declaration](https://en.cppreference.com/w/c/language/union)
- [cppreference: member access](https://en.cppreference.com/w/c/language/operator_member_access)
