# 21-15. Part 21 종합 복습
## 1. 학습 목표
- bitwise operators, shifts, masks를 종합한다.
- C17 defined·UB·implementation-defined 규칙을 구분한다.
- 가상 GPIO operations를 안전하게 조합한다.
## 2. 선수 지식
21-1부터 21-14까지를 학습했다.
## 3. 핵심 개념
unsigned value, width-aware mask, count validation을 기본 규율로 사용한다. set=`|`, clear=`& ~`, toggle=`^`, test=`&`+comparison이다.
## 4. 문법
```c
flags |= mask;
flags &= ~mask;
flags ^= mask;
is_set = (flags & mask) != 0u;
```
## 5. 최소 코드 예제
```c
#include <stdint.h>
#include <stdio.h>
int main(void)
{
    uint8_t flags = 0u;
    uint8_t mask = (uint8_t)(UINT8_C(1) << 2);
    flags = (uint8_t)(flags | mask);
    unsigned int set = (flags & mask) != 0u;
    flags = (uint8_t)(flags ^ mask);
    printf("%u %02X\n", set, (unsigned int)flags);
    return 0;
}
```
## 6. 코드 해석
valid fixed position mask로 set·test·toggle을 수행하고 최종 상태를 출력한다.
## 7. 내부 동작
- **[C17 표준]** integer promotions, conversions, shifts와 bitwise semantics를 정한다.
- signed left shift는 sign·representability 조건을 어기면 UB이고 negative right shift는 implementation-defined다.
- binary literals는 C17 표준 문법이 아니며 signed representation도 two's complement로 고정되지 않는다.
- **[compiler/CPU]** [MIPS — 수업 기준] AND/OR/XOR/shift instructions, [RISC-V — 병행 학습] AND/OR/XOR/SLL/SRL/SRA 계열을 사용할 수 있지만 C expression이 항상 instruction 하나가 되는 것은 아니다.
## 8. 자주 하는 실수
- logical/address/bitwise operators를 혼동한다.
- width 이상의 shift count가 modulo된다고 생각한다.
- bit masks와 memory endianness를 같은 개념으로 본다.
- shift가 항상 multiplication보다 빠르다고 말한다.
- `%b`, `0b...`, rotate operator가 C17에 있다고 생각한다.
## 9. 필수 실습
width guard와 unsigned masks를 사용해 가상 GPIO의 set·clear·toggle·read를 종합한다.
[21-15 exercise](../../exercises/21-bitwise-operators/21-15/README.md)
## 10. 추가 실습
- ★ operator 표를 만든다.
- ★★ UB 분류표를 만든다.
- ★★★ field update와 GPIO API contract를 검토한다.
## 11. 확인 문제
1. `&`, `&&`, unary `&` 차이는?
2. `~unsigned char` 결과 type 주의점은?
3. shift count UB 조건은?
4. negative signed right shift 분류는?
5. set/clear/toggle/test 식은?
6. bit position과 endianness 차이는?
7. C와 ISA instruction 관계는?
## 12. 핵심 정리
- unsigned types와 valid counts를 사용한다.
- operations를 mask patterns로 조합한다.
- C17 semantics와 compiler·ISA 구현을 구분한다.
## 13. 다음 Step
커리큘럼의 다음 Step은 **22-1. 함수 주소와 함수 포인터 선언**이다. Part 22 파일은 만들지 않는다.
## 14. 참고 자료
- N1570 6.3.1.1, 6.5.3.3, 6.5.7, 6.5.10~14, 6.5.16.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: arithmetic operators](https://en.cppreference.com/w/c/language/operator_arithmetic)
- [SEI CERT INT34-C](https://wiki.sei.cmu.edu/confluence/display/c/INT34-C.+Do+not+shift+an+expression+by+a+negative+number+of+bits+or+by+greater+than+or+equal+to+the+number+of+bits+that+exist+in+the+operand)
