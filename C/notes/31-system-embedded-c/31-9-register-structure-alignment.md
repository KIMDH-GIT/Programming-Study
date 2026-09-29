# 31-9. register 자료구조의 alignment
## 1. 학습 목표
- type alignment requirement와 object address를 구분한다.
- struct member alignment를 `offsetof`와 `_Alignof`로 관찰한다.
- CPU의 misaligned access 동작을 C validity와 동일시하지 않는다.
## 2. 선수 지식
Part 19의 struct, Part 14의 pointer, 31-4의 register mock을 안다.
## 3. 핵심 개념
alignment는 특정 type의 object가 가질 수 있는 address 제약이다. `_Alignof(type)`은 C11/C17 operator이고 `<stdalign.h>`의 `alignof` macro와 구분할 수 있다.

register block을 struct로 표현할 때 member alignment와 padding이 생길 수 있다. 그러나 실제 device layout 일치는 C struct만으로 자동 보장되지 않으며 ABI/compiler/device 문서를 함께 확인해야 한다.
## 4. 문법
```c
_Alignof(unsigned)
offsetof(RegisterBlock, value)
```
`offsetof`는 `<stddef.h>`의 표준 macro를 사용하며 NULL pointer 기반 homemade trick을 쓰지 않는다.
## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

typedef struct {
    unsigned char tag;
    unsigned value;
} RegisterBlock;

int main(void)
{
    size_t offset = offsetof(RegisterBlock, value);
    size_t alignment = _Alignof(unsigned);
    printf("aligned_offset=%d object_alignment_ok=%d\n",
           offset % alignment == 0u,
           _Alignof(RegisterBlock) >= alignment);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o alignment
./alignment
```
## 6. 코드 해석
정의된 C layout에서 두 조건은 참이므로 출력은 `aligned_offset=1 object_alignment_ok=1`이다. 구체적인 offset 숫자는 implementation/ABI에 따라 다를 수 있다.
## 7. 내부 동작
**[ISO C17 portable]** member 선언 순서는 보존되고 각 member는 자신의 alignment 요구를 만족한다.

**[ABI-specific]** 구체적인 alignment 값, padding 수, 전체 `sizeof`는 ABI와 implementation에 의존한다.

**[CPU / device]** CPU가 misaligned load를 지원하더라도 잘못 정렬된 pointer로 C object를 접근하는 것이 자동으로 valid해지지 않는다.
## 8. 자주 하는 실수
- CPU가 misaligned access를 허용하므로 C에서도 항상 valid라고 말한다.
- struct가 padding 없이 device register에 정확히 맞는다고 가정한다.
- 모든 compiler/ABI에서 offset을 같은 숫자로 고정한다.
- homemade NULL-pointer `offsetof` macro를 사용한다.
## 9. 필수 실습
alignment 배수 조건만 검사하고 구체 숫자는 host observation으로 기록한다. [31-9 exercise](../../exercises/31-system-embedded-c/31-9/README.md)
## 10. 추가 실습
- ★ member 순서를 바꾸어 size/offset을 관찰한다.
- ★★ `_Alignof`와 `offsetof` 결과를 표로 만든다.
- ★★★ ABI 문서와 device register offset을 비교하는 절차를 작성한다.
## 11. 확인 문제
1. type alignment requirement란 무엇인가?
2. member offset과 CPU access behavior가 다른 층인 이유는?
3. concrete padding 수를 C17이 고정하지 않는 이유는?
4. `_Alignof`는 어느 표준의 기능인가?
5. actual device layout에는 어떤 추가 문서가 필요한가?
## 12. 핵심 정리
- alignment, padding, CPU behavior, device layout을 분리한다.
- 표준 `_Alignof`와 `offsetof`를 사용한다.
- 숫자 관찰은 현재 implementation 결과로만 보고한다.
## 13. 다음 Step
[31-10. padding과 member offset](31-10-padding-and-member-offset.md)
## 14. 참고 자료
- WG14 N1570 6.2.8, 6.5.3.4, 7.19. N1570은 C11 공개 Committee Draft이며 alignment/offsetof 규칙은 C17에서도 유지된다: https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf
