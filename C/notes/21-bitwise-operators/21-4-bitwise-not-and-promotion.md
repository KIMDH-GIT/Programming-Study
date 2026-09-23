# 21-4. `~`와 integer promotion
## 1. 학습 목표
- unary bitwise NOT의 결과를 계산한다.
- integer promotion 뒤 width에서 반전됨을 설명한다.
- narrow unsigned type 결과를 단정하지 않는다.
## 2. 선수 지식
Part 6 integer promotions와 21-1 bitwise operands를 안다.
## 3. 핵심 개념
`~value`는 promoted operand의 모든 value-representation bits를 반전한다. `unsigned char x`가 보통 `int`로 promotion되므로 `~x` 결과 type이 자동 `unsigned char`인 것은 아니다.
## 4. 문법
width가 명확한 `unsigned int`를 사용하되 그 width가 항상 32 bits라고 가정하지 않는다.
```c
unsigned int result = ~0x0Fu;
```
## 5. 최소 코드 예제
```c
#include <limits.h>
#include <stdio.h>
int main(void)
{
    unsigned int value = 0x0Fu;
    unsigned int result = ~value;
    printf("%X %zu\n", result, sizeof result * CHAR_BIT);
    return 0;
}
```
## 6. 코드 해석
현재 `unsigned int` width 전체에서 low four 1 bits를 포함한 모든 bits를 반전한다. 출력 자릿수는 구현 width에 따라 달라질 수 있다.
## 7. 내부 동작
**[C17 표준]** integer promotions 뒤 unary `~`를 적용하며 unsigned result는 promoted type 범위에서 정의된다. `sizeof`는 C bytes, `CHAR_BIT`은 한 C byte의 bits 수다.
## 8. 자주 하는 실수
- `~unsigned char` 결과가 항상 unsigned char라고 생각한다.
- `~0x0F`가 모든 구현에서 `0xF0`이라고 말한다.
- unsigned int가 항상 32 bits라고 가정한다.
- C17 signed representation을 항상 two's complement라고 단정한다.
## 9. 필수 실습
unsigned value와 NOT 결과, type width를 hex로 출력한다.
[21-4 exercise](../../exercises/21-bitwise-operators/21-4/README.md)
## 10. 추가 실습
- ★ 다른 masks를 반전한다.
- ★★ unsigned char promotion을 분석한다.
- ★★★ signed NOT 해석이 representation에 의존하는 이유를 적는다.
## 11. 확인 문제
1. `~`는 언제 integer promotion을 적용하는가?
2. unsigned char expression 결과 type은 항상 unsigned char인가?
3. unsigned int width는 어떻게 계산하는가?
4. C byte와 bit 차이는?
5. signed representation을 고정할 수 없는 이유는?
## 12. 핵심 정리
- NOT은 promoted width 전체에 적용된다.
- width는 `sizeof(T) * CHAR_BIT`로 관찰한다.
- portable masks는 unsigned types를 우선한다.
## 13. 다음 Step
[21-5. unsigned `<<`, `>>`](21-5-unsigned-shifts.md)
## 14. 참고 자료
- N1570 5.2.4.2.1, 6.3.1.1, 6.5.3.3. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: arithmetic operators](https://en.cppreference.com/w/c/language/operator_arithmetic)
