# 31-11. pointer casting·alignment·aliasing
## 1. 학습 목표
- pointer conversion의 alignment requirement를 설명한다.
- 같은 address와 허용되는 lvalue access를 구분한다.
- `memcpy`와 character-type byte inspection을 안전하게 사용한다.
## 2. 선수 지식
Part 14의 pointer, 31-8의 object representation, 31-9의 alignment를 안다.
## 3. 핵심 개념
pointer를 다른 object type pointer로 cast할 수 있다는 사실만으로 dereference가 valid해지지 않는다. 결과 pointer는 target type에 올바르게 정렬되어야 하고, object의 effective type/compatible type access 규칙도 만족해야 한다.

두 pointer가 같은 numeric address를 표현하는 것과 어떤 lvalue type으로 object를 접근할 수 있는지는 다른 문제다. character type lvalue는 object representation을 검사할 수 있고, `memcpy`는 alignment가 다른 typed dereference를 만들지 않고 bytes를 복사한다.
## 4. 문법
```c
void *raw = &value;
uint32_t *same_type = raw;
memcpy(bytes, &value, sizeof value);
```
## 5. 최소 코드 예제
```c
#include <stdint.h>
#include <stdio.h>
#include <string.h>

int main(void)
{
    uint32_t value = UINT32_C(0x12345678);
    void *raw = &value;
    uint32_t *same_type = raw;
    unsigned char bytes[sizeof value];

    *same_type ^= UINT32_C(0x000000FF);
    memcpy(bytes, &value, sizeof bytes);
    printf("value=%08X byte_count=%zu first_byte=%02X\n",
           (unsigned)value, sizeof bytes, (unsigned)bytes[0]);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o pointer_rules
./pointer_rules
```
## 6. 코드 해석
현재 host 출력의 `value`는 `12345687`이고 byte 수는 `sizeof(uint32_t)`다. 첫 byte의 구체 값은 host byte order에 따라 달라질 수 있어 portable expected output으로 고정하지 않는다.
## 7. 내부 동작
`void *` round-trip은 원래 `uint32_t` object를 같은 compatible type pointer로 다시 접근한다. `memcpy` destination은 `unsigned char` array라 byte 복사가 유효하다.

incompatible struct pointer cast, misaligned address cast, expired pointer, out-of-bounds pointer를 dereference하는 unsafe 예제는 실행하지 않는다.

**[compiler]** alias analysis는 language access 규칙을 이용하는 optimization reasoning이다. pointer 값이 같다는 사실 자체와 동일한 개념이 아니다.
## 8. 자주 하는 실수
- cast가 compiler warning을 없애면 dereference도 valid라고 생각한다.
- CPU가 misaligned access를 지원하므로 C pointer도 valid라고 말한다.
- strict aliasing을 단순히 주소가 달라야 한다는 규칙으로 설명한다.
- character byte inspection과 incompatible typed dereference를 혼동한다.
## 9. 필수 실습
same-type `void *` round-trip과 `memcpy` byte copy를 실행하고 host-dependent 첫 byte를 observation으로만 기록한다. [31-11 exercise](../../exercises/31-system-embedded-c/31-11/README.md)
## 10. 추가 실습
- ★ `memcpy`로 value를 같은 type object에 복원한다.
- ★★ `_Alignof(uint32_t)`와 object address 조건을 설명한다.
- ★★★ 잘못된 cast+deref 예제를 실행하지 않고 위반 규칙만 분석한다.
## 11. 확인 문제
1. pointer cast와 valid dereference가 다른 이유는?
2. alignment requirement는 어느 type 기준인가?
3. character type으로 무엇을 관찰할 수 있는가?
4. alias analysis와 동일 주소 pointer가 다른 개념인 이유는?
5. `memcpy`가 typed misaligned access를 피하는 이유는?
## 12. 핵심 정리
- cast는 alignment, lifetime, extent, access type 규칙을 면제하지 않는다.
- character access와 `memcpy`를 목적에 맞게 사용한다.
- unsafe alias/misalignment 예제는 실행하지 않는다.
## 13. 다음 Step
[31-12. driver interface의 const correctness](31-12-driver-interface-const-correctness.md)
## 14. 참고 자료
- WG14 N1570 6.2.4, 6.3.2.3, 6.5 paragraph 7, 7.24.2.1. N1570은 C11 공개 Committee Draft이며 관련 규칙은 C17에서도 유지된다: https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf
