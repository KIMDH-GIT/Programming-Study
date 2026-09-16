# 15-7. 포인터 차이와 `ptrdiff_t`

같은 array object의 두 element pointers를 빼면 byte 수가 아니라 element positions의 차이를 얻는다.

## 1. 학습 목표
- pointer subtraction의 same-array 전제를 설명한다.
- result type `ptrdiff_t`를 사용한다.
- `%td`로 portable하게 출력한다.

## 2. 선수 지식
Step 15-5의 element units와 Step 15-6의 one-past를 사용한다.

## 3. 핵심 개념
`&values[4] - &values[1]`은 positions 4와 1의 차이 3이다. `ptrdiff_t`는 두 pointers를 뺀 result를 표현하는 signed integer type이다. 서로 unrelated arrays의 pointers를 빼는 것은 정의된 byte-distance 계산법이 아니다.

## 4. 문법
```c
#include <stddef.h>
ptrdiff_t distance = later - earlier;
printf("%td\n", distance);
```

## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

int main(void)
{
    int values[5] = {10, 20, 30, 40, 50};
    int *first = &values[1];
    int *last = &values[4];
    ptrdiff_t distance = last - first;

    printf("distance: %td\n", distance);
    printf("reverse: %td\n", first - last);
    return 0;
}
```

## 6. 코드 해석
last는 index 4, first는 index 1이라 distance는 3이다. 반대 방향 subtraction은 -3이다. result는 bytes가 아니라 int element positions다.

## 7. 내부 동작
[C17 abstract machine] same array 또는 one-past 관련 pointers의 subtraction result는 subscript difference이며 type은 `ptrdiff_t`. result가 `ptrdiff_t`로 표현되지 않으면 behavior가 정의되지 않는다. [ABI] type width는 implementation이 정한다.

## 8. 자주 하는 실수
- pointer difference를 byte difference라고 생각한다.
- unrelated objects의 pointers를 뺀다.
- result를 `int`로 고정한다.
- `%d`로 `ptrdiff_t`를 출력한다.

## 9. 필수 실습
한 array 안의 여러 element pointers 사이 distances를 `%td`로 출력한다. [실습 README](../../exercises/15-arrays-and-pointers/15-7/README.md)

## 10. 추가 실습
- ★ first와 one-past distance
- ★★ negative distance
- ★★★ valid/invalid subtraction 분류

## 11. 확인 문제
1. pointer subtraction의 result 단위는?
2. result type은?
3. 출력 specifier는?
4. unrelated pointers subtraction은 가능한가?
5. reverse subtraction result는 왜 negative인가?

## 12. 핵심 정리
pointer difference는 같은 array 내 element position 차이며 `ptrdiff_t`와 `%td`를 사용한다.

## 13. 다음 Step
[Step 15-8. 포인터로 배열 순회](15-8-pointer-traversal.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.6, 7.19
- [cppreference: ptrdiff_t](https://en.cppreference.com/w/c/types/ptrdiff_t.html)
