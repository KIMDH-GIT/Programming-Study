# 21-13. `uint8_t GPIO` 가상 레지스터
## 1. 학습 목표
- 8-bit unsigned object를 register-like value로 다룬다.
- masks로 pins를 set/clear/test한다.
- 실제 MMIO와 일반 변수를 구분한다.
## 2. 선수 지식
Part 4 `uint8_t`, 21-8~11을 안다.
## 3. 핵심 개념
`uint8_t`는 implementation이 정확히 8-bit type을 제공할 때 존재한다. 여기서는 일반 object로 GPIO-like bit manipulation을 연습하며 실제 hardware register가 아니다.
## 4. 문법
```c
uint8_t gpio = 0u;
uint8_t pin3 = (uint8_t)(UINT8_C(1) << 3);
```
integer promotions 뒤 결과를 명시적으로 `uint8_t` 범위에 저장한다.
## 5. 최소 코드 예제
```c
#include <stdint.h>
#include <stdio.h>
int main(void)
{
    uint8_t gpio = 0u;
    uint8_t pin3 = (uint8_t)(UINT8_C(1) << 3);
    gpio = (uint8_t)(gpio | pin3);
    printf("%02X %d\n", (unsigned int)gpio, (gpio & pin3) != 0u);
    return 0;
}
```
## 6. 코드 해석
pin 3 mask를 만들고 일반 uint8_t object에 set한 뒤 test한다.
## 7. 내부 동작
**[C17 표준]** narrow operands는 promotion될 수 있고 assignment에서 uint8_t로 변환된다. **[hardware]** 실제 MMIO에는 `volatile`, read-modify-write side effects 등 Part 31 규칙이 추가된다.
## 8. 자주 하는 실수
- `uint8_t`가 모든 구현에 필수라고 말한다.
- 일반 object 예제를 실제 hardware access라고 부른다.
- `%X`에 promoted type을 고려하지 않는다.
- bit number와 memory byte order를 혼동한다.
## 9. 필수 실습
가상 GPIO의 pin을 set하고 test해 hex로 출력한다.
[21-13 exercise](../../exercises/21-bitwise-operators/21-13/README.md)
## 10. 추가 실습
- ★ 두 pins를 set한다.
- ★★ one pin을 clear한다.
- ★★★ 실제 MMIO에 필요한 추가 규칙을 조사한다.
## 11. 확인 문제
1. uint8_t 제공 조건은?
2. promotion은 어디서 일어나는가?
3. 실제 MMIO와 차이는?
4. `%02X` 인자 cast 이유는?
## 12. 핵심 정리
- uint8_t object로 8-bit register-like 연습을 한다.
- promotions와 output type을 명시한다.
- hardware semantics는 후속 범위다.
## 13. 다음 Step
[21-14. `set`, `clear`, `toggle`, `read`](21-14-bit-operations-functions.md)
## 14. 참고 자료
- N1570 6.3.1.1, 7.20.1.1. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: fixed-width integer types](https://en.cppreference.com/w/c/types/integer)
