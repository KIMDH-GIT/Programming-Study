# 20-10. endianness 기초

endianness는 multi-byte representation에서 significance가 다른 bytes를 어떤 주소 순서로 배치하는지 설명하는 implementation 특성이다.

## 1. 학습 목표
- little-endian과 big-endian 개념을 구분한다.
- multi-byte object의 native representation을 관찰한다.
- 관찰 결과를 C17 고정 규칙으로 일반화하지 않는다.

## 2. 선수 지식
20-9 object representation과 Part 4 fixed-width integers를 안다.

## 3. 핵심 개념
little-endian은 least-significant byte가 낮은 주소에, big-endian은 most-significant byte가 낮은 주소에 놓이는 흔한 convention이다. C17은 한 가지를 강제하지 않는다.

## 4. 문법
20-9와 같이 character-type pointer로 exact-width integer의 bytes를 관찰한다. `uint16_t`는 implementation이 제공할 때만 존재하며, 이 예제의 “두 byte” 비교는 `CHAR_BIT == 8`일 때만 성립한다.
```c
#if defined(UINT16_MAX) && CHAR_BIT == 8
uint16_t value = 0x0102u;
const unsigned char *bytes = (const unsigned char *)&value;
#endif
```

## 5. 최소 코드 예제
```c
#include <limits.h>
#include <stdint.h>
#include <stdio.h>

int main(void)
{
#if defined(UINT16_MAX) && CHAR_BIT == 8
    uint16_t value = 0x0102u;
    const unsigned char *bytes = (const unsigned char *)&value;

    printf("%02X %02X\n",
           (unsigned int)bytes[0], (unsigned int)bytes[1]);
    if (bytes[0] == 0x02u && bytes[1] == 0x01u) {
        puts("little-endian representation");
    } else if (bytes[0] == 0x01u && bytes[1] == 0x02u) {
        puts("big-endian representation");
    } else {
        puts("other representation");
    }
#else
    puts("this observation requires uint16_t and 8-bit bytes");
#endif
    return 0;
}
```

## 6. 코드 해석
`uint16_t`가 있고 C byte가 8 bits인 구현에서만 두 bytes를 낮은 주소부터 출력한다. 다른 구현에서는 전제 불충족을 알린다. 흔한 little-/big-endian pattern을 구분하되, 어느 결과도 C17이 강제한다고 말하지 않는다.

## 7. 내부 동작
- **[C17 표준]** character type을 통한 representation 관찰을 허용하지만 native endianness는 지정하지 않는다.
- **[ABI/CPU]** object representation의 byte order를 정한다.
- protocol/file format은 C object layout과 별개의 external contract다.
- `uint16_t`는 정확히 16-bit type을 제공할 수 있는 implementation에서만 정의된다. C byte가 16 bits라면 `sizeof(uint16_t)`가 1일 수 있으므로 무조건 `bytes[1]`을 읽으면 안 된다.

## 8. 자주 하는 실수
- C17이 little-endian을 보장한다고 생각한다.
- x86 결과를 ARM·RISC-V 전체에 일반화한다.
- structure/union raw bytes를 portable protocol packet으로 사용한다.
- union inactive member 읽기로 endianness를 “안전하게 변환”한다.

## 9. 필수 실습
`UINT16_MAX`가 정의되고 `CHAR_BIT == 8`인지 확인한 뒤에만 `uint16_t`의 두 bytes를 낮은 주소부터 출력하고 순서를 분류한다.
[20-10 exercise](../../exercises/20-enum-typedef-union/20-10/README.md)

## 10. 추가 실습
- ★ 여러 `uint16_t` values를 관찰한다.
- ★★ 관찰 결과와 C17 보장을 표로 구분한다.
- ★★★ protocol byte order가 별도 contract인 이유를 조사한다.

## 11. 확인 문제
1. C17은 native byte order를 정하는가?
2. little-endian과 big-endian의 차이는?
3. native representation 관찰을 portable serialization로 쓸 수 없는 이유는?
4. `uint16_t`는 모든 구현에 필수인가?
5. raw structure bytes가 portable format이 아닌 이유는?
6. `CHAR_BIT == 16`이면 `sizeof(uint16_t)`가 얼마일 수 있는가?

## 12. 핵심 정리
- endianness는 representation ordering 특성이다.
- byte 관찰은 현재 implementation의 native order를 보여 준다.
- C object layout을 serialization format으로 사용하지 않는다.

## 13. 다음 Step
[20-11. Part 20 종합 복습](20-11-part-20-review.md)

## 14. 참고 자료
- N1570 6.2.6.1, 7.20.1.1. N1570은 **C11 공개 Committee Draft**이며 관련 rules는 C17에서도 유지된다.
- [cppreference: fixed-width integer types](https://en.cppreference.com/w/c/types/integer)
- [IETF RFC 1700: Data Notations](https://www.rfc-editor.org/rfc/rfc1700)
