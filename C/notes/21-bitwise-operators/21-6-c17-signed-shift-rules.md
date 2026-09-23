# 21-6. C17 signed shift의 UB·구현 정의 동작
## 1. 학습 목표
- signed left shift의 defined 조건과 UB를 구분한다.
- negative signed right shift가 implementation-defined임을 설명한다.
- 위험 예제를 실행하지 않는다.
## 2. 선수 지식
21-5 unsigned shift와 Part 3 signed representation을 안다.
## 3. 핵심 개념
nonnegative signed left operand에 `2^count`를 곱한 수학적 결과가 **signed result type 자체에 표현 가능할 때만** `E1 << E2`가 정의된다. negative left operand 또는 signed result type에 표현 불가능한 결과는 UB다. 따라서 32-bit `int`에서 `1 << 31`은 unsigned에는 표현 가능해도 `int`에는 표현 불가능하므로 UB다. negative signed right shift 결과는 implementation-defined다.
## 4. 문법
```c
int safe = 3 << 2; /* 12, representable */
```
`-1 << 1`, `INT_MAX << 1`, negative count, count >= width는 실행 예제로 사용하지 않는다.
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    int value = 3;
    int count = 2;
    int result = value << count;
    printf("%d\n", result);
    return 0;
}
```
## 6. 코드 해석
value와 count가 nonnegative이고 결과 12가 `int`에 표현 가능하므로 이 실행은 정의된다.
## 7. 내부 동작
**[C17 표준]** signed left shift는 operand sign·representability 조건을 요구한다. negative signed right shift 결과는 implementation-defined다. C17은 signed representation을 two's complement로만 제한하지 않는다.
## 8. 자주 하는 실수
- signed left shift에서 넘친 bits가 항상 버려진다고 말한다.
- negative right shift는 항상 1-fill이라고 단정한다.
- 관찰된 GCC 결과를 C17 보장으로 일반화한다.
- shift를 항상 multiply/divide로 설명한다.
## 9. 필수 실습
정의된 positive signed shift 하나를 실행하고 위험 cases는 표로 분류한다.
[21-6 exercise](../../exercises/21-bitwise-operators/21-6/README.md)
## 10. 추가 실습
- ★ representable 조건을 적는다.
- ★★ unsigned 대안으로 바꾼다.
- ★★★ UB와 implementation-defined cases를 분류한다.
## 11. 확인 문제
1. signed left shift가 정의되는 조건은?
2. negative left operand shift는?
3. negative signed right shift 분류는?
4. count 관련 UB 조건은?
5. GCC 관찰값이 C17 보장인가?
## 12. 핵심 정리
- signed shifts는 sign·range·count를 모두 검토한다.
- negative right shift는 implementation-defined다.
- portable masks에는 unsigned를 우선한다.
## 13. 다음 Step
[21-7. 폭에 맞는 unsigned bit mask](21-7-width-aware-unsigned-mask.md)
## 14. 참고 자료
- N1570 6.2.6.2, 6.5.7. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: shift operators](https://en.cppreference.com/w/c/language/operator_arithmetic)
