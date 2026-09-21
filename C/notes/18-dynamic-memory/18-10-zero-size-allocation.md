# 18-10. 0 크기 할당의 구현 차이

C17에서 zero-size allocation 결과는 implementation-defined 선택을 포함하므로 정상 교육 예제는 양의 크기를 사용한다.

## 1. 학습 목표
- `malloc(0)` 결과를 단정하지 않는다.
- `realloc(p, 0)`을 portable `free(p)`로 가르치지 않는다.
- zero count를 application boundary에서 처리한다.

## 2. 선수 지식
Step 18-5의 allocation contract와 Step 18-8의 `free`를 안다.

## 3. 핵심 개념
zero size 요청은 null pointer를 반환하거나, object access에 사용하면 안 되는 non-null pointer를 반환할 수 있다. C17은 DR 400을 반영해 `realloc(p, 0)`이 새 object를 할당하지 않고 null pointer를 반환할 때 old object의 deallocation 여부가 implementation-defined임을 명확히 했고, zero-size `realloc` 사용을 obsolescent feature로 분류한다. 명확한 deallocation에는 `free`를 사용한다.

## 4. 문법
```c
if (count == 0) {
    return 0;
}
```
application이 zero count를 별도 의미로 처리하면 모호한 allocation 결과에 의존하지 않는다.

## 5. 최소 코드 예제
```c
#include <stdlib.h>

int main(void)
{
    size_t count = 3;

    if (count == 0) {
        return 0;
    }
    int *values = malloc(count * sizeof *values);
    if (values == NULL) {
        return 1;
    }
    values[0] = 1;
    free(values);
    return 0;
}
```

## 6. 코드 해석
zero count를 allocation 전에 처리한다. allocation 경로는 size > 0만 사용한다.

## 7. 내부 동작
어떤 pointer를 반환할지는 implementation-defined일 수 있지만, returned zero-size pointer로 object access를 해서는 안 된다. allocator 내부가 실제로 몇 bytes를 예약하는지는 C17이 정하지 않는다.

## 8. 자주 하는 실수
- `malloc(0)`은 항상 NULL이라고 말한다.
- non-null이면 1 byte를 쓸 수 있다고 생각한다.
- `realloc(p, 0)`을 모든 구현의 `free(p)`와 동일시한다.

## 9. 필수 실습
count가 0이면 allocation하지 않고 정상 종료하는 분기를 작성한다.
[18-10 exercise](../../exercises/18-dynamic-memory/18-10/README.md)

## 10. 추가 실습
- ★ zero count 정책을 문서화한다.
- ★★ empty input과 allocation failure를 다른 결과로 표현한다.
- ★★★ C17 zero-size 규칙을 표로 정리한다.

## 11. 확인 문제
1. `malloc(0)`은 항상 NULL인가?
2. non-null zero-size pointer를 dereference할 수 있는가?
3. `realloc(p, 0)`을 portable `free`로 사용할 수 있는가?
4. 교육 예제에서 양의 크기를 쓰는 이유는?

## 12. 핵심 정리
- zero-size 결과를 한 가지로 단정하지 않는다.
- zero count는 application logic에서 명시적으로 처리한다.
- 해제에는 `free`를 직접 사용한다.

## 13. 다음 Step
[18-11. `realloc`과 임시 포인터](18-11-realloc-and-temporary-pointer.md)

## 14. 참고 자료
- N1570 7.22.3 Memory management functions; 7.22.3.5 `realloc`. N1570은 **C11 공개 Committee Draft**로 기본 규칙을 제공하지만, C17의 zero-size `realloc` 설명은 DR 400 반영 문구로 확인해야 한다.
- [WG14 N2243: DR 400 `realloc` with size zero](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2243.htm)
- [cppreference: malloc](https://en.cppreference.com/w/c/memory/malloc)
- [cppreference: realloc](https://en.cppreference.com/w/c/memory/realloc)
