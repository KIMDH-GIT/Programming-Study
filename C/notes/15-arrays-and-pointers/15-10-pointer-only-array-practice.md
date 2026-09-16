# 15-10. 포인터만 이용한 배열 실습

pointer traversal과 indirection만으로 array의 합과 최댓값을 계산하며 bounds와 state를 종합할 수 있다.

## 1. 학습 목표
- subscript 없이 array elements를 읽는다.
- one-past endpoint까지 합과 maximum을 계산한다.
- nonempty array 전제를 명시한다.

## 2. 선수 지식
Step 15-8의 forward traversal과 Part 11의 sum·maximum을 사용한다.

## 3. 핵심 개념
array 선언 자체에는 brackets가 필요하지만 처리 loop는 begin/end pointers와 `*current`만 사용한다. maximum은 first element로 초기화하므로 array가 최소 한 element라는 전제가 필요하다. small values로 signed sum overflow를 피한다.

## 4. 문법
```c
int *current = values;
int *end = values + count;
while (current < end) {
    use(*current);
    ++current;
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int values[5] = {-2, 7, 3, 1, 4};
    int *current = values;
    int *end = values + 5;
    int sum = 0;
    int maximum = *current;

    while (current < end) {
        sum += *current;
        if (*current > maximum) {
            maximum = *current;
        }
        ++current;
    }

    printf("sum: %d\n", sum);
    printf("max: %d\n", maximum);
    return 0;
}
```

## 6. 코드 해석
current가 five valid elements를 차례로 가리킨다. 합은 13이고 maximum은 7이다. current가 end가 되면 loop가 끝나 one-past를 읽지 않는다.

## 7. 내부 동작
[C17 abstract machine] all pointer values remain in the same array/one-past domain and only element pointers are dereferenced. array lifetime은 main block 전체에서 유효하다. [compiler] pointer loop와 index loop는 equivalent optimized code로 변환될 수 있으며 성능 우위를 언어가 보장하지 않는다.

## 8. 자주 하는 실수
- empty array 가능성을 무시하고 first element를 읽는다.
- maximum을 0으로 고정해 all-negative case를 틀린다.
- end를 dereference한다.
- pointer loop가 bounds 규칙을 우회한다고 생각한다.

## 9. 필수 실습
pointer-only processing으로 five values의 sum과 maximum을 구한다. [실습 README](../../exercises/15-arrays-and-pointers/15-10/README.md)

## 10. 추가 실습
- ★ minimum 계산
- ★★ negative values only
- ★★★ pointer-only search로 first match 찾기

## 11. 확인 문제
1. maximum 초기값은?
2. 필요한 array 전제는?
3. current가 end일 때 dereference하는가?
4. pointer traversal도 bounds를 지켜야 하는가?
5. pointer loop가 항상 더 빠른가?

## 12. 핵심 정리
pointer-only processing도 same-array bounds, nonempty 전제, lifetime과 arithmetic safety를 그대로 지켜야 한다.

## 13. 다음 Step
[Step 15-11. Part 15 종합 복습](15-11-part-15-review.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.3.2.1, 6.5.3.2, 6.5.6, 6.5.8
- [cppreference: Pointer arithmetic](https://en.cppreference.com/w/c/language/operator_arithmetic.html#Pointer_arithmetic)
