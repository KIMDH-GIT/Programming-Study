# 21-11. bit test
## 1. 학습 목표
- AND mask 결과를 0과 비교한다.
- 한 bit와 여러 bits 검사 조건을 구분한다.
## 2. 선수 지식
21-1 AND와 21-7 masks를 안다.
## 3. 핵심 개념
`(flags & mask) != 0u`는 mask 중 하나 이상 set인지 검사한다. `(flags & mask) == mask`는 mask의 모든 bits가 set인지 검사한다.
## 4. 문법
괄호를 사용해 precedence 의도를 명확히 한다.
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    unsigned int flags = 0xAu, mask = 0x2u;
    printf("%d\n", (flags & mask) != 0u);
    return 0;
}
```
## 6. 코드 해석
mask bit가 flags에 존재해 1이 출력된다.
## 7. 내부 동작
**[C17 표준]** bitwise AND 뒤 comparison 결과는 0 또는 1이다. bitwise AND 자체 결과는 boolean으로 제한되지 않는다.
## 8. 자주 하는 실수
- `flags & mask == 0u`로 precedence를 놓친다.
- any/all 검사를 혼동한다.
- `&&`로 bit positions를 검사한다.
## 9. 필수 실습
한 mask로 any/all tests를 출력한다.
[21-11 exercise](../../exercises/21-bitwise-operators/21-11/README.md)
## 10. 추가 실습
- ★ clear bit를 검사한다.
- ★★ multiple-bit any/all을 비교한다.
- ★★★ test helper contract를 작성한다.
## 11. 확인 문제
1. 한 bit test 식은?
2. any와 all 차이는?
3. 괄호가 필요한 이유는?
4. 결과 0/1은 어느 operator가 만드는가?
## 12. 핵심 정리
- AND 결과를 명시적으로 비교한다.
- any/all contract를 구분한다.
- precedence를 괄호로 드러낸다.
## 13. 다음 Step
[21-12. bit field 추출·갱신](21-12-bit-field-extract-update.md)
## 14. 참고 자료
- N1570 6.5.9, 6.5.10. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: operator precedence](https://en.cppreference.com/w/c/language/operator_precedence)
