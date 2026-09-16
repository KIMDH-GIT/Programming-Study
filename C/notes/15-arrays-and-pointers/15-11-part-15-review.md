# 15-11. Part 15 종합 복습

Part 15에서는 array expression conversion, pointer arithmetic, one-past, difference와 pointer traversal을 학습했다.

## 1. 학습 목표
- array object와 pointer object를 끝까지 구분한다.
- same-array pointer bounds와 `sizeof` context를 종합한다.
- function parameter 관련 규칙을 다음 Part와 정확히 연결한다.

## 2. 선수 지식
Step 15-1부터 15-10까지와 Part 11~14의 arrays·pointers를 사용한다.

## 3. 핵심 개념
array object는 pointer object가 아니다. array expression은 여러 context에서 first element pointer로 변환된다. pointer arithmetic은 same array elements와 one-past 범위에서 element 단위로 정의된다. subscript는 pointer addition과 indirection으로 정의되지만 bounds를 우회하지 않는다.

## 4. 문법
```c
int values[5] = {10, 20, 30, 40, 50};
int *begin = values;
int *end = values + 5;
for (int *current = begin; current < end; ++current) {
    printf("%d\n", *current);
}
```

## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

int main(void)
{
    int values[5] = {10, 20, 30, 40, 50};
    int *begin = values;
    int *end = values + 5;

    printf("array bytes: %zu\n", sizeof(values));
    printf("pointer bytes: %zu\n", sizeof(begin));
    printf("count by distance: %td\n", end - begin);

    for (int *current = begin; current < end; ++current) {
        printf("%d\n", *current);
    }
    return 0;
}
```

## 6. 코드 해석
actual array `sizeof`와 pointer `sizeof`의 의미를 분리한다. end-begin은 element count 5이고 loop는 only valid element pointers를 dereference한다.

## 7. 내부 동작
[C17 abstract machine] conversion exceptions, subscript definition, additive operators, pointer difference와 comparison이 적용된다. function parameter의 array notation은 Part 16에서 pointer parameter로 adjustment되고 count를 별도로 전달하는 이유를 배운다. C는 여전히 pointer value를 pass-by-value한다. 2차원 array conversion은 first row pointer이며 `int **`가 아니다.

## 8. 자주 하는 실수
- 배열은 pointer라고 말한다.
- `sizeof(array)`와 `sizeof(pointer)`를 혼동한다.
- p+1을 raw address +1이라고 설명한다.
- one-past를 dereference한다.
- array argument가 call by reference로 전달된다고 말한다.

## 9. 필수 실습
array/pointer sizes, pointer distance, pointer traversal을 한 프로그램에서 확인한다. [실습 README](../../exercises/15-arrays-and-pointers/15-11/README.md)

## 10. 추가 실습
- ★ subscript와 pointer values 비교
- ★★ safe reverse traversal
- ★★★ array object·converted expression·parameter adjustment 점검표

## 11. 확인 문제
1. array object와 pointer object가 같은가?
2. conversion이 일어나지 않는 대표 context는?
3. `arr[i]`의 정의상 form은?
4. one-past를 dereference할 수 있는가?
5. pointer difference result type은?
6. array parameter `sizeof`가 caller array size인가?
7. 2차원 array가 `int **`인가?

## 12. 핵심 정리
array-to-pointer conversion은 expression 규칙이며 pointer arithmetic·comparison·dereference는 same-array bounds와 lifetime 안에서 사용한다.

## 13. 다음 Step
Step 16-1. 값 매개변수로 원본을 바꾸지 못하는 이유

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.3.2.1, 6.5.2.1, 6.5.3.4, 6.5.6, 6.5.8, 6.7.6.3
- [cppreference: Arrays](https://en.cppreference.com/w/c/language/array.html)
- [cppreference: Pointer arithmetic](https://en.cppreference.com/w/c/language/operator_arithmetic.html#Pointer_arithmetic)
