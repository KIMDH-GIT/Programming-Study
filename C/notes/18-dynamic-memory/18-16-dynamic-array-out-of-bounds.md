# 18-16. 동적 배열 out-of-bounds

allocated storage도 element count가 정한 범위 안에서만 array-like access할 수 있다.

## 1. 학습 목표
- `i < count` 경계를 유지한다.
- one-past pointer와 dereference 가능 범위를 구분한다.
- allocated byte count와 logical element count를 함께 관리한다.

## 2. 선수 지식
Part 15의 pointer arithmetic과 Step 18-6의 dynamic array pattern을 안다.

## 3. 핵심 개념
`count` elements를 위한 storage에서 valid subscripts는 `0`부터 `count - 1`까지다. `values + count`는 one-past 계산에 사용할 수 있지만 dereference할 수 없다.

## 4. 문법
```c
for (size_t i = 0; i < count; ++i) {
    values[i] = 0;
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    size_t count = 4;
    int *values = malloc(count * sizeof *values);

    if (values == NULL) {
        return 1;
    }
    for (size_t i = 0; i < count; ++i) {
        values[i] = (int)(i * 2);
    }
    printf("%d\n", values[count - 1]);
    free(values);
    return 0;
}
```

## 6. 코드 해석
loop condition이 모든 access를 allocated range 안에 둔다. 마지막 valid index만 읽고 base pointer를 해제한다.

## 7. 내부 동작
out-of-bounds access는 UB다. allocator metadata나 인접 allocation이 실제로 어디 있는지는 구현 세부이며, 접근이 우연히 성공해 보여도 valid하지 않다.

## 8. 자주 하는 실수
- `i <= count`를 사용한다.
- allocation byte 수를 element count로 착각한다.
- `sizeof(values)`로 count를 복원한다.

## 9. 필수 실습
count elements를 채우고 첫·마지막 원소만 출력한다.
[18-16 exercise](../../exercises/18-dynamic-memory/18-16/README.md)

## 10. 추가 실습
- ★ index 범위를 표로 쓴다.
- ★★ pointer loop를 one-past 비교로 작성한다.
- ★★★ shrink 후 새 count로만 접근한다.

## 11. 확인 문제
1. valid 마지막 index는?
2. one-past pointer를 dereference할 수 있는가?
3. `sizeof(values)`가 count를 주는가?
4. out-of-bounds가 항상 segmentation fault인가?

## 12. 핵심 정리
- pointer와 count를 함께 유지한다.
- 모든 access는 `i < count`를 지킨다.
- one-past는 비교용이지 access 대상이 아니다.

## 13. 다음 Step
[18-17. 검사 실행 예제로 메모리 오류 관찰](18-17-memory-error-checking.md)

## 14. 참고 자료
- N1570 6.5.6 Additive operators; 6.5.2.1 Array subscripting. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: pointer arithmetic](https://en.cppreference.com/w/c/language/operator_arithmetic)
- [GCC: AddressSanitizer](https://gcc.gnu.org/onlinedocs/gcc/Instrumentation-Options.html)
