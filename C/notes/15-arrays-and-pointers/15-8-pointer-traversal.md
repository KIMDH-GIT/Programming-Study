# 15-8. 포인터로 배열 순회

current pointer를 first element에서 one-past endpoint까지 증가시키면 subscript 없이 array elements를 순회할 수 있다.

## 1. 학습 목표
- begin·current·end pointers로 forward traversal을 작성한다.
- `++pointer`와 `++*pointer`를 구분한다.
- same-array relational comparison 전제를 유지한다.

## 2. 선수 지식
Step 15-5의 increment와 Step 15-6의 one-past endpoint를 안다.

## 3. 핵심 개념
`++current`는 pointer object에 저장된 value를 next element pointer로 바꾼다. `++*current`는 pointed-to int value를 증가시키므로 전혀 다르다. `*current++` 같은 compact expression은 precedence 때문에 `*(current++)`이지만 입문 code에서는 분리해 명확히 쓴다.

## 4. 문법
```c
for (int *current = values; current < end; ++current) {
    printf("%d\n", *current);
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int values[5] = {10, 20, 30, 40, 50};
    int *end = values + 5;

    for (int *current = values; current < end; ++current) {
        printf("%d\n", *current);
    }
    return 0;
}
```

## 6. 코드 해석
current는 each element pointer를 차례로 저장한다. body에서는 valid current만 dereference하며 마지막 increment로 end가 되면 condition이 false라 one-past를 읽지 않는다.

## 7. 내부 동작
[C17 abstract machine] increment는 current를 same array의 next element로 이동시키며 comparison은 same array/one-past pointers 사이에서 정의된다. [compiler/CPU] source pointer loop가 index loop와 같은 optimized code가 될 수도 있으며 C는 특정 form의 superiority를 보장하지 않는다.

## 8. 자주 하는 실수
- `++current`와 `++*current`를 혼동한다.
- one-past endpoint를 dereference한다.
- compact `*current++`를 설명 없이 사용한다.
- pointer loop가 index loop보다 항상 빠르다고 단정한다.

## 9. 필수 실습
begin/end pointer loop로 모든 array values를 출력한다. [실습 README](../../exercises/15-arrays-and-pointers/15-8/README.md)

## 10. 추가 실습
- ★ pointer loop로 sum 계산
- ★★ each pointed value 수정
- ★★★ index loop와 상태 추적 비교

## 11. 확인 문제
1. `++current`가 바꾸는 것은?
2. `++*current`가 바꾸는 것은?
3. loop endpoint는?
4. end를 dereference하는가?
5. pointer loop가 항상 더 빠른가?

## 12. 핵심 정리
pointer traversal은 same-array begin부터 one-past end까지 pointer value를 증가시키며 valid elements만 dereference한다.

## 13. 다음 Step
[Step 15-9. 시작 전 포인터를 만들지 않는 역순 순회](15-9-safe-reverse-traversal.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.6, 6.5.8
- [cppreference: Pointer arithmetic](https://en.cppreference.com/w/c/language/operator_arithmetic.html#Pointer_arithmetic)
