# 21-12. bit field 추출·갱신
## 1. 학습 목표
- 연속 bit field를 mask와 shift로 추출한다.
- 기존 field를 clear한 뒤 새 값을 제한해 갱신한다.
## 2. 선수 지식
21-5 shifts와 set/clear masks를 안다.
## 3. 핵심 개념
field mask가 `0x1Cu`이고 시작 위치가 2라면 추출은 `(value & mask) >> 2`다. 갱신은 기존 field clear와 새 field insert를 결합한다.
## 4. 문법
```c
value = (value & ~mask) | ((field << shift) & mask);
```
field를 mask하여 인접 bits 침범을 막는다.
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    unsigned int value = 0xA5u, mask = 0x1Cu, shift = 2u;
    unsigned int field = (value & mask) >> shift;
    value = (value & ~mask) | ((3u << shift) & mask);
    printf("%u %X\n", field, value);
    return 0;
}
```
## 6. 코드 해석
기존 3-bit field를 추출하고 새 값 3을 같은 위치에 넣는다.
## 7. 내부 동작
모든 operands는 unsigned이고 fixed counts 2는 width보다 작다. 이는 C bit-field declaration이 아니라 integer value 안의 logical field다.
## 8. 자주 하는 실수
- 기존 field를 clear하지 않고 OR만 한다.
- 새 값을 mask하지 않아 인접 bits를 바꾼다.
- bit-field layout과 mask layout을 동일시한다.
## 9. 필수 실습
3-bit field를 추출하고 다른 값으로 갱신한다.
[21-12 exercise](../../exercises/21-bitwise-operators/21-12/README.md)
## 10. 추가 실습
- ★ 다른 field 값을 넣는다.
- ★★ field 범위를 검사한다.
- ★★★ mask/shift contract를 함수로 만든다.
## 11. 확인 문제
1. 추출 순서는?
2. 갱신 전 clear 이유는?
3. 새 field를 mask하는 이유는?
4. C bit-field와 같은 기능인가?
## 12. 핵심 정리
- AND 후 right shift로 추출한다.
- clear 후 bounded field를 OR한다.
- mask·shift 범위를 명시한다.
## 13. 다음 Step
[21-13. `uint8_t GPIO` 가상 레지스터](21-13-uint8-gpio-register.md)
## 14. 참고 자료
- N1570 6.5.7, 6.5.10, 6.5.12. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: arithmetic operators](https://en.cppreference.com/w/c/language/operator_arithmetic)
