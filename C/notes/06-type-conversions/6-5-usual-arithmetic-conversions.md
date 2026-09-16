# 6-5. usual arithmetic conversions

서로 다른 산술형 피연산자는 공통 실수형 또는 정수형으로 맞춰진 뒤 계산된다.
## 1. 학습 목표
- 실수 계열 우선순위를 설명한다.
- 정수 승격 뒤 공통형 선택을 설명한다.
- 결과형을 피연산자 문맥에서 판별한다.
## 2. 선수 지식
Step 6-4와 기본 산술식을 사용한다.
## 3. 핵심 개념
`long double`, `double`, `float` 순으로 해당 형이 있으면 다른 피연산자를 그 형으로 바꾼다. 둘 다 정수면 먼저 integer promotion 후 rank와 signed 여부 규칙으로 공통형을 정한다. 이것이 usual arithmetic conversions다.
## 4. 문법
```c
double result = integer + real_value;
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    int count = 3;
    double fraction = 0.5;
    double result = count + fraction;
    printf("%.1f\n", result);
    return 0;
}
```
## 6. 코드 해석
`count`가 `double` 3.0으로 변환된 뒤 0.5와 더해져 3.5가 된다.
## 7. 내부 동작
공통형 결정은 C 추상 기계 규칙이다. 한 식이 CPU 명령 하나와 일대일 대응한다고 가정하지 않는다.
## 8. 자주 하는 실수
- 결과형은 항상 왼쪽 피연산자형이라고 생각한다.
- 대입 대상형이 중간 계산형을 먼저 정한다고 생각한다.
- integer promotion 단계를 건너뛴다.
## 9. 필수 실습
`int + double` 결과를 관찰한다. [실습 README](../../exercises/06-type-conversions/6-5/README.md).
## 10. 추가 실습
- ★ 기초: `float + double`.
- ★★ 응용: `long + long long`.
- ★★★ 도전: 정수 공통형 결정표 작성.
## 11. 확인 문제
1. 공통형은 언제 정해지는가?
2. `int + double`의 계산형은?
3. 정수끼리는 무엇을 먼저 적용하는가?
4. 대입 대상이 중간형을 정하는가?
## 12. 핵심 정리
산술 피연산자는 정해진 변환 절차로 공통형에 맞춘 뒤 계산된다.
## 13. 다음 Step
[Step 6-6. signed·unsigned 혼합 연산](6-6-signed-unsigned-mixed.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.3.1.8.
- [cppreference: usual arithmetic conversions](https://en.cppreference.com/w/c/language/conversion.html)
- [GCC warning options](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html)
