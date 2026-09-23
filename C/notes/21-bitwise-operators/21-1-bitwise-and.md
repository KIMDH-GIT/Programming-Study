# 21-1. `&`
## 1. 학습 목표
- unary address operator와 binary bitwise AND를 구분한다.
- `&`와 logical `&&`의 결과·평가 차이를 설명한다.
- unsigned mask로 필요한 bits를 선택한다.
## 2. 선수 지식
Part 3 정수 표현, Part 7 logical operators, Part 14 주소 연산자를 안다.
## 3. 핵심 개념
`&value`는 operand 하나의 주소를 얻는 unary operator다. `a & b`는 두 integer operands의 대응 bit마다 AND를 수행한다. `a && b`는 scalar truth를 검사하고 0 또는 1을 만들며 short-circuit하지만 bitwise `&`는 그렇지 않다.
## 4. 문법
```c
unsigned int a = 0xCu; /* 1100 */
unsigned int b = 0xAu; /* 1010 */
unsigned int result = a & b; /* 1000 */
```
bitwise operands에는 integer promotions와 usual arithmetic conversions가 적용된다. `unsigned char`끼리 연산해도 결과가 자동 `unsigned char`인 것은 아니다.
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    unsigned int flags = 0xDu;
    unsigned int mask = 0x4u;
    unsigned int selected = flags & mask;
    printf("%X %d\n", selected, selected != 0u);
    return 0;
}
```
## 6. 코드 해석
mask가 1인 위치만 유지한다. 결과를 0과 비교해 해당 bit가 set인지 검사한다.
## 7. 내부 동작
**[C17 표준]** `&`는 promoted integer values에 bitwise AND를 정의한다. short-circuit하지 않는다. **[compiler/CPU]** target instruction 하나로 만들 수 있지만 C가 항상 CPU AND 한 명령을 요구하지는 않는다.
## 8. 자주 하는 실수
- `&x`와 `x & y`를 같은 semantics로 설명한다.
- `&`와 `&&`를 교환한다.
- `flags & mask == 0u`처럼 precedence를 오해한다. `(flags & mask) == 0u`로 쓴다.
- C17 표준 binary literal `0b...`가 있다고 생각한다.
## 9. 필수 실습
hex flags와 mask를 AND하여 선택된 bits와 검사 결과를 출력한다.
[21-1 exercise](../../exercises/21-bitwise-operators/21-1/README.md)
## 10. 추가 실습
- ★ 다른 mask를 적용한다.
- ★★ `&`와 `&&` 결과를 비교한다.
- ★★★ `unsigned char` promotion을 `_Generic` 없이 type 규칙으로 분석한다.
## 11. 확인 문제
1. unary `&x`와 binary `x & y`의 차이는?
2. `&`가 short-circuit하는가?
3. `0xCu & 0xAu` 결과는?
4. `flags & mask == 0u`에 괄호가 필요한 이유는?
5. bitwise operands에는 어떤 conversion이 적용되는가?
## 12. 핵심 정리
- binary `&`는 bit별 AND다.
- logical `&&`와 주소 연산자 `&`를 구분한다.
- mask와 명시적 괄호를 사용한다.
## 13. 다음 Step
[21-2. `|`](21-2-bitwise-or.md)
## 14. 참고 자료
- N1570 6.3.1.1, 6.5.10, 6.5.13. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: arithmetic operators](https://en.cppreference.com/w/c/language/operator_arithmetic)
