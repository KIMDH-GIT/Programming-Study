# 6-8. `5 / 2`와 `5.0 / 2`
같은 `/`도 usual arithmetic conversions 뒤 피연산자형에 따라 결과가 달라진다.
## 1. 학습 목표
- 정수·실수 나눗셈을 판별한다.
- 정수 몫의 0 방향 절단을 설명한다.
- 안전한 음수 예를 해석한다.
## 2. 선수 지식
Step 6-5와 Part 5의 나눗셈 예고를 사용한다.
## 3. 핵심 개념
`5/2`는 정수 2다. `5.0/2`에서는 2가 `double`로 변환되어 2.5다. 정수 나눗셈 몫은 0 방향으로 절단되므로 `-5/2`는 -2다. 0 제수와 `INT_MIN/-1` 경계는 Part 7에서 자세히 다루며 실행하지 않는다.
## 4. 문법
```c
int q = 5 / 2;
double r = 5.0 / 2;
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    printf("%d %.1f %d\n", 5 / 2, 5.0 / 2, -5 / 2);
    return 0;
}
```
## 6. 코드 해석
결과는 2, 2.5, -2다. “소수점을 버린다”보다 0 방향 절단이 정확하다.
## 7. 내부 동작
나눗셈 규칙은 C 의미다. CPU 명령과 일대일 대응한다고 단정하지 않는다.
## 8. 자주 하는 실수
- -5/2를 -3으로 예상한다.
- 대입 대상이 나눗셈 종류를 바꾼다고 생각한다.
- 0 나눗셈을 실험한다.
## 9. 필수 실습
세 안전한 나눗셈을 출력한다. [실습 README](../../exercises/06-type-conversions/6-8/README.md).
## 10. 추가 실습
- ★ 기초: 7/2.
- ★★ 응용: -7/2.
- ★★★ 도전: 0 방향 절단 수직선.
## 11. 확인 문제
1. `5/2`는?
2. `5.0/2`는?
3. `-5/2`는?
4. 0 제수는 실행해도 되는가?
## 12. 핵심 정리
나눗셈 종류는 변환 뒤 피연산자형이 정하며 정수 몫은 0 방향으로 절단된다.
## 13. 다음 Step
[Step 6-9. cast 위치와 중간 결과](6-9-cast-position.md)
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.5.
- [cppreference: arithmetic operators](https://en.cppreference.com/w/c/language/operator_arithmetic.html)
- [GCC integer implementation](https://gcc.gnu.org/onlinedocs/gcc/Integers-implementation.html)
