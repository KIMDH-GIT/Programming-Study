# 18-4. 원소 수와 할당 크기 overflow 검증

`count * sizeof *p`가 먼저 overflow하면 allocator는 원래 의도보다 작은 크기만 요청받을 수 있다.

## 1. 학습 목표
- `size_t` multiplication overflow 위험을 설명한다.
- 나눗셈 기반 사전 검사를 작성한다.
- allocation failure와 size overflow를 구분한다.

## 2. 선수 지식
Part 3의 unsigned arithmetic과 `<stdint.h>`의 `SIZE_MAX`를 안다.

## 3. 핵심 개념
`size_t`는 unsigned integer type이므로 곱셈은 범위를 넘으면 modulo 연산으로 wrap된다. overflow된 작은 크기의 allocation이 성공할 수도 있어 `malloc` 실패 검사만으로 충분하지 않다.

## 4. 문법
```c
if (count > SIZE_MAX / sizeof *values) {
    /* too large */
}
```
`SIZE_MAX`는 C17 `<stdint.h>`가 `size_t`의 최댓값으로 제공하는 표준 limit macro다.

## 5. 최소 코드 예제
```c
#include <stdint.h>
#include <stdlib.h>

int main(void)
{
    size_t count = 10;

    if (count > SIZE_MAX / sizeof(int)) {
        return 1;
    }
    int *values = malloc(count * sizeof *values);
    if (values == NULL) {
        return 1;
    }
    values[0] = 7;
    free(values);
    return 0;
}
```

## 6. 코드 해석
division check가 multiplication 전에 수행된다. 검사를 통과한 뒤 allocation failure를 별도로 검사한다.

## 7. 내부 동작
**[C17]** unsigned overflow는 modulo arithmetic이다. **[allocator]** allocator는 이미 계산된 byte count만 받으므로 원래 element count를 복원해 검증하지 않는다.

## 8. 자주 하는 실수
- overflow면 `malloc`이 반드시 NULL을 반환한다고 생각한다.
- signed 음수를 `size_t`로 변환해 count로 사용한다.
- multiplication 뒤 결과가 작아졌는지만 확인한다.

## 9. 필수 실습
고정된 작은 `count`에 overflow guard를 적용하고 한 element를 저장한 뒤 해제한다.
[18-4 exercise](../../exercises/18-dynamic-memory/18-4/README.md)

## 10. 추가 실습
- ★ guard 식을 자연어로 설명한다.
- ★★ `double` element로 바꾼다.
- ★★★ 사용자 입력 검증 시 필요한 네 단계를 목록으로 만든다.

## 11. 확인 문제
1. `size_t` 곱셈 overflow는 어떤 결과를 내는가?
2. allocator가 overflow를 항상 알아낼 수 없는 이유는?
3. overflow 검사와 allocation failure 검사는 왜 둘 다 필요한가?
4. 음수 입력을 바로 `size_t`로 바꾸면 왜 위험한가?

## 12. 핵심 정리
- 크기 곱셈은 allocation 전에 검사한다.
- overflow와 allocation failure는 서로 다른 실패다.
- count와 element size를 함께 검증한다.

## 13. 다음 Step
[18-5. `void *`와 `malloc`](18-5-void-pointer-and-malloc.md)

## 14. 참고 자료
- N1570 6.2.5 Types; 6.2.6.2 Integer types; 7.20.3 Limit of size_t. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: integer types](https://en.cppreference.com/w/c/types/integer)
- [CERT C: integer multiplication](https://wiki.sei.cmu.edu/confluence/display/c/INT30-C.+Ensure+that+unsigned+integer+operations+do+not+wrap)
