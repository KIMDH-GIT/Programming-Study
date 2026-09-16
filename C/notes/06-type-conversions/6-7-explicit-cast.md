# 6-7. explicit cast 문법
cast는 `(type) expression`으로 명시적 변환을 요청한다. 위험한 변환을 안전하게 만들지는 않는다.
## 1. 학습 목표
- cast 문법을 읽는다.
- 명시성과 안전성을 구별한다.
- 불필요한 cast를 피한다.
## 2. 선수 지식
implicit conversion과 대상형을 사용한다.
## 3. 핵심 개념
cast 결과는 지정한 형의 값이다. 범위 밖 signed 변환이나 실수→정수 UB 조건은 cast를 써도 그대로다.
## 4. 문법
```c
double result = (double)integer;
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    int total = 5;
    int count = 2;
    double average = (double)total / count;
    printf("%.1f\n", average);
    return 0;
}
```
## 6. 코드 해석
`total`을 먼저 `double`로 바꾸어 나눗셈 공통형이 `double`이 된다.
## 7. 내부 동작
cast는 C 식의 형을 바꾼다. CPU 명령 하나를 강제하지 않는다.
## 8. 자주 하는 실수
- cast가 범위 검사를 해 준다고 생각한다.
- 경고를 숨기려고 무조건 cast한다.
- `(double)(total / count)`와 같다고 생각한다.
## 9. 필수 실습
정수 평균을 cast로 실수 계산한다. [실습 README](../../exercises/06-type-conversions/6-7/README.md).
## 10. 추가 실습
- ★ 기초: 한 피연산자 cast.
- ★★ 응용: cast 없는 결과 비교.
- ★★★ 도전: 불필요한 cast 찾기.
## 11. 확인 문제
1. cast 문법은?
2. cast가 안전성 검사를 하는가?
3. 한 피연산자만 `double`이면 결과형은?
4. cast 위치가 중요한가?
## 12. 핵심 정리
cast는 변환 의도를 명시하지만 기존 변환 규칙과 경계를 없애지 않는다.
## 13. 다음 Step
[Step 6-8. `5 / 2`와 `5.0 / 2`](6-8-integer-real-division.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.4.
- [cppreference: cast operator](https://en.cppreference.com/w/c/language/cast.html)
- [GCC warning options](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html)
