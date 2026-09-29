# 31-8. endianness와 명시적 byte 조립
## 1. 학습 목표
- endianness를 multi-byte object representation의 byte order로 설명한다.
- bit order와 byte order를 구분한다.
- protocol bytes를 명시적 shift/OR로 조립한다.
## 2. 선수 지식
Part 13의 `unsigned char`, Part 21의 shift, 31-6의 fixed-width type을 안다.
## 3. 핵심 개념
Endianness는 multi-byte value의 byte들이 낮은 주소부터 어떤 순서로 놓이는지에 관한 implementation/ABI 특성이다. bit numbering이 반대라는 뜻이 아니다. ISO C17은 모든 implementation의 byte order를 하나로 고정하지 않는다.

object representation은 character type lvalue로 관찰할 수 있다. 그러나 raw struct/object bytes를 file, network packet, device format으로 그대로 쓰는 것은 portable serialization이 아니다. external format은 byte 순서를 명시하고 byte 단위로 조립한다.
## 4. 문법
```c
static uint32_t read_le32(const unsigned char bytes[4]);
static uint32_t read_be32(const unsigned char bytes[4]);
```
## 5. 최소 코드 예제
```c
#include <inttypes.h>
#include <stdint.h>
#include <stdio.h>

static uint32_t read_le32(const unsigned char bytes[4])
{
    return (uint32_t)bytes[0] |
           ((uint32_t)bytes[1] << 8) |
           ((uint32_t)bytes[2] << 16) |
           ((uint32_t)bytes[3] << 24);
}

static uint32_t read_be32(const unsigned char bytes[4])
{
    return ((uint32_t)bytes[0] << 24) |
           ((uint32_t)bytes[1] << 16) |
           ((uint32_t)bytes[2] << 8) |
           (uint32_t)bytes[3];
}

int main(void)
{
    const unsigned char bytes[4] = {0x12u, 0x34u, 0x56u, 0x78u};
    printf("le=%08" PRIX32 " be=%08" PRIX32 "\n",
           read_le32(bytes), read_be32(bytes));
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o byte_order
./byte_order
```
## 6. 코드 해석
출력은 `le=78563412 be=12345678`이다. host endianness와 무관하게 external byte order를 code로 명시했기 때문에 deterministic하다.
## 7. 내부 동작
각 `unsigned char`를 `uint32_t`로 변환한 뒤 유효한 0, 8, 16, 24 count로 shift한다. 이 예제는 object memory를 다른 pointer type으로 재해석하지 않는다.

현재 x86_64 host가 little-endian이어도 “C는 little-endian”이라는 결론은 성립하지 않는다.
## 8. 자주 하는 실수
- little-endian을 bit 순서가 반대라고 설명한다.
- host byte order를 C17 보장으로 말한다.
- network/file bytes를 struct pointer로 cast해 읽는다.
- signed `char`를 shift해 sign extension 문제를 만든다.
## 9. 필수 실습
같은 4 bytes를 little/big order로 각각 조립해 exact 결과를 확인한다. [31-8 exercise](../../exercises/31-system-embedded-c/31-8/README.md)
## 10. 추가 실습
- ★ 16-bit little-endian 조립 함수를 만든다.
- ★★ value를 big-endian bytes로 분해한다.
- ★★★ host object bytes 관찰과 protocol parsing을 비교한다.
## 11. 확인 문제
1. endianness는 무엇의 순서인가?
2. bit order와 다른 이유는?
3. explicit byte assembly가 host-independent인 이유는?
4. character type 접근은 무엇을 관찰할 수 있는가?
5. raw struct write가 portable serialization이 아닌 이유는?
## 12. 핵심 정리
- endianness는 byte order다.
- external representation은 명시적으로 조립한다.
- host observation과 ISO C 보장을 분리한다.
## 13. 다음 Step
[31-9. register 자료구조의 alignment](31-9-register-structure-alignment.md)
## 14. 참고 자료
- WG14 N1570 6.2.6.1, 6.5 paragraph 7. N1570은 C11 공개 Committee Draft이며 object representation 접근 규칙은 C17에서도 유지된다: https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf
