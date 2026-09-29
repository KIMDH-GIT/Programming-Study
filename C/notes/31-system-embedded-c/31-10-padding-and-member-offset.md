# 31-10. padding과 member offset
## 1. 학습 목표
- member order, offset, padding, total size를 구분한다.
- portable invariant와 host-specific number를 나눈다.
- raw struct representation을 external format으로 사용하지 않는다.
## 2. 선수 지식
Part 19의 struct layout과 31-9의 alignment를 안다.
## 3. 핵심 개념
struct의 non-bit-field member는 선언 순서대로 뒤쪽 address에 배치되지만 member 사이와 마지막에는 unnamed padding이 있을 수 있다. 구체적인 padding byte 수, member offset, total `sizeof`는 모든 implementation에서 같지 않다.

따라서 C struct memory를 network packet, file format, device register block의 portable serialization로 그대로 쓰면 안 된다. external format의 byte order와 field position을 명시적으로 처리한다.
## 4. 문법
```c
offsetof(Layout, tag)
offsetof(Layout, value)
sizeof(Layout)
```
## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

typedef struct {
    unsigned char tag;
    unsigned value;
    unsigned char flag;
} Layout;

int main(void)
{
    size_t tag = offsetof(Layout, tag);
    size_t value = offsetof(Layout, value);
    size_t flag = offsetof(Layout, flag);
    int ordered = tag < value && value < flag;
    int size_covers_last = sizeof(Layout) >= flag + sizeof(unsigned char);
    printf("ordered=%d size_covers_last=%d\n", ordered, size_covers_last);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o padding
./padding
```
## 6. 코드 해석
표준이 보장하는 순서와 object extent를 검사하므로 출력은 `ordered=1 size_covers_last=1`이다. exact offsets는 출력 계약에 넣지 않는다.
## 7. 내부 동작
padding byte는 member가 아니며 그 값에 의미 있는 field를 저장했다고 가정할 수 없다. struct assignment는 member values를 복사하지만 raw bytes가 external representation과 같다는 뜻은 아니다.

**[ABI-specific]** alignment policy가 concrete offset과 size를 정한다.

**[device/SoC-specific]** register offset은 datasheet가 정한다. 필요하면 명시적 reserved field와 compile-time checks를 사용하되 target contract를 함께 기록한다.
## 8. 자주 하는 실수
- C struct에는 padding이 없다고 말한다.
- 모든 compiler에서 같은 layout이라고 단정한다.
- `sizeof`만 맞으면 모든 member offset도 맞다고 생각한다.
- struct bytes를 portable file/device format으로 저장한다.
## 9. 필수 실습
portable order/extent condition과 host-specific offset 관찰을 분리한다. [31-10 exercise](../../exercises/31-system-embedded-c/31-10/README.md)
## 10. 추가 실습
- ★ member 순서를 바꾸고 size를 관찰한다.
- ★★ 각 offset이 member alignment 배수인지 확인한다.
- ★★★ external 6-byte format을 explicit bytes로 encode한다.
## 11. 확인 문제
1. member order에서 C가 보장하는 것은?
2. padding이 생기는 이유는?
3. exact offset을 portable하게 고정할 수 없는 이유는?
4. raw struct가 serialization이 아닌 이유는?
5. device offset 검증에는 무엇이 필요한가?
## 12. 핵심 정리
- 선언 순서와 concrete layout number를 구분한다.
- padding과 offset은 implementation/ABI 영향을 받는다.
- external representation은 explicit encoding을 사용한다.
## 13. 다음 Step
[31-11. pointer casting·alignment·aliasing](31-11-pointer-casting-alignment-and-aliasing.md)
## 14. 참고 자료
- WG14 N1570 6.2.6.1, 6.7.2.1, 7.19. N1570은 C11 공개 Committee Draft이며 struct layout 규칙은 C17에서도 유지된다: https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf
