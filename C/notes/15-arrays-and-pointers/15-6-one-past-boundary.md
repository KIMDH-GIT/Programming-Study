# 15-6. 같은 배열과 one-past 경계

array의 마지막 element 바로 다음 위치를 가리키는 one-past pointer는 계산·비교할 수 있지만 dereference할 수 없다.

## 1. 학습 목표
- element pointer 범위와 one-past endpoint를 구분한다.
- end pointer를 loop 종료 조건으로 사용한다.
- one-past object가 존재한다고 오해하지 않는다.

## 2. 선수 지식
Step 15-5의 pointer arithmetic과 Part 11의 bounds를 안다.

## 3. 핵심 개념
count 5 array에서 `values+5`는 one-past pointer다. 이는 iteration endpoint로 유용하지만 `*(values+5)`는 valid element access가 아니다. 정의된 pointer arithmetic은 같은 array object의 elements와 one-past 관계 안에 머물러야 한다.

## 4. 문법
```c
int *begin = values;
int *end = values + count;
for (int *current = begin; current < end; ++current) {
    use(*current);
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int values[5] = {10, 20, 30, 40, 50};
    int *current = values;
    int *end = values + 5;

    while (current < end) {
        printf("%d\n", *current);
        ++current;
    }

    printf("reached end: %s\n", current == end ? "yes" : "no");
    return 0;
}
```

## 6. 코드 해석
current는 elements 0~4를 가리킬 때만 dereference된다. 마지막 increment 후 current는 end와 같아 loop가 끝난다. end 자체는 출력 비교에만 사용되고 dereference되지 않는다.

## 7. 내부 동작
[C17 abstract machine] one-past pointer는 array bounds arithmetic과 comparison에 참여할 수 있지만 pointed-to object가 존재하지 않아 unary `*`로 value access할 수 없다. relational comparison은 같은 array object와 one-past 관련 pointers에 대해 사용한다.

## 8. 자주 하는 실수
- one-past 위치에 숨은 element가 있다고 생각한다.
- end pointer를 dereference한다.
- unrelated arrays의 pointers를 `<`로 address 숫자처럼 비교한다.
- array 밖을 여러 positions 넘어가는 pointer를 만든다.

## 9. 필수 실습
begin/end pointers로 array를 순회하고 end 도달을 비교만 한다. [실습 README](../../exercises/15-arrays-and-pointers/15-6/README.md)

## 10. 추가 실습
- ★ empty iteration condition 분석
- ★★ count 1 array
- ★★★ valid element·one-past·invalid 위치 분류

## 11. 확인 문제
1. one-past pointer는 무엇을 가리키는가?
2. 계산 가능한가?
3. dereference 가능한가?
4. endpoint로 어떻게 쓰는가?
5. unrelated pointers의 relational comparison을 일반화할 수 있는가?

## 12. 핵심 정리
one-past pointer는 같은 array 순회의 endpoint이지 dereference 가능한 element pointer가 아니다.

## 13. 다음 Step
[Step 15-7. 포인터 차이와 `ptrdiff_t`](15-7-pointer-difference.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.6, 6.5.8
- [cppreference: Pointer arithmetic](https://en.cppreference.com/w/c/language/operator_arithmetic.html#Pointer_arithmetic)
