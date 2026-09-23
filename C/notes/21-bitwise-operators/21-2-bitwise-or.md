# 21-2. `|`
## 1. 학습 목표
- bitwise OR의 truth table과 결과를 계산한다.
- logical `||`와 구분한다.
- mask로 bits를 set한다.
## 2. 선수 지식
21-1의 mask와 Part 7의 logical OR를 안다.
## 3. 핵심 개념
각 위치에서 둘 중 하나라도 1이면 결과가 1이다. `0100 | 0010`은 `0110`이다. `||`는 truth 결과 0/1과 short-circuit를 제공하지만 `|`는 모든 대응 bits를 처리하고 short-circuit하지 않는다.
## 4. 문법
```c
flags = flags | mask;
flags |= mask;
```
compound assignment는 왼쪽 operand를 한 번만 평가하고 결과를 왼쪽 type으로 변환해 저장하므로 모든 복잡한 식에서 단순 textual substitution과 동일하다고 일반화하지 않는다.
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    unsigned int flags = 0x4u;
    unsigned int mask = 0x2u;
    flags |= mask;
    printf("%X\n", flags);
    return 0;
}
```
## 6. 코드 해석
mask가 1인 bit를 set하고 기존 1 bits는 유지하므로 결과는 `0x6`이다.
## 7. 내부 동작
**[C17 표준]** integer promotions와 usual arithmetic conversions 뒤 bitwise inclusive OR를 수행한다. operand evaluation order를 `|`가 정하지 않으며 short-circuit도 없다.
## 8. 자주 하는 실수
- `|`와 `||`를 같은 operator로 본다.
- OR로 bit를 clear하려 한다.
- `f() | g()`의 call order를 단정한다.
- compound assignment를 언제나 문자 치환으로 설명한다.
## 9. 필수 실습
두 masks를 차례로 OR해 bits를 set한다.
[21-2 exercise](../../exercises/21-bitwise-operators/21-2/README.md)
## 10. 추가 실습
- ★ 이미 set된 bit를 다시 set한다.
- ★★ 여러 masks를 한 식으로 조합한다.
- ★★★ `|`와 `||`의 observable 차이를 설명한다.
## 11. 확인 문제
1. `0100 | 0010` 결과는?
2. `|`가 short-circuit하는가?
3. `flags |= mask`의 목적은?
4. precedence가 evaluation order를 정하는가?
## 12. 핵심 정리
- `|`는 bit별 inclusive OR다.
- mask로 bits를 set한다.
- logical OR와 evaluation 규칙을 구분한다.
## 13. 다음 Step
[21-3. `^`](21-3-bitwise-xor.md)
## 14. 참고 자료
- N1570 6.5.12, 6.5.14, 6.5.16.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: arithmetic operators](https://en.cppreference.com/w/c/language/operator_arithmetic)
