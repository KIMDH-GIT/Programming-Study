# 7-10. Part 7 종합 복습

Part 7은 식의 구성, 변환 뒤 연산, UB 경계, side effect와 sequencing을 하나의 규칙 체계로 연결한다.
## 1. 학습 목표
- 산술·비교·논리·대입·증감 규칙을 연결한다.
- UB 경계를 실행 없이 판별한다.
- 묶임과 평가 순서를 구별한다.
## 2. 선수 지식
Step 7-1~7-9와 Part 6 변환 규칙이다.
## 3. 핵심 개념
연산 전 promotion과 usual conversions를 확인한다. 비교·논리는 `int` 0/1을 만든다. short-circuit는 오른쪽 평가 여부를 정한다. 0 제수, 표현 불가능한 signed 결과, unsequenced modification은 UB다.
## 4. 문법
```c
int safe = divisor != 0 && dividend / divisor > 2;
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    int value = 5;
    int divisor = 2;
    int safe = divisor != 0 && value / divisor == 2;
    value += 3;
    printf("%d %d %d\n", safe, value, value > 0);
    return 0;
}
```
## 6. 코드 해석
안전한 나눗셈 비교는 1, 복합 대입 뒤 value는 8, 비교 결과는 1이다.
## 7. 내부 동작
C 추상 기계의 값·side effect·sequencing 규칙을 우선하며 CPU 명령과 직접 동일시하지 않는다.
## 8. 자주 하는 실수
- 모든 overflow를 wrap으로 본다.
- 비교 결과를 별도 bool형으로 단정한다.
- 우선순위를 평가 순서로 본다.
- UB 식을 실행해 결과를 찾는다.
## 9. 필수 실습
안전 guard, 복합 대입, 비교를 한 프로그램에서 검증한다. [실습 README](../../exercises/07-operators/7-10/README.md).
## 10. 추가 실습
- ★ 기초: 연산자 분류표.
- ★★ 응용: 변환·결과형 추적.
- ★★★ 도전: UB 판별표.
## 11. 확인 문제
1. 비교 결과형은?
2. short-circuit의 역할은?
3. 정수 나눗셈 UB 두 경계는?
4. 전위·후위 결과 차이는?
5. precedence와 evaluation order는 같은가?
## 12. 핵심 정리
연산자를 안전하게 쓰려면 피연산자형, 변환, 결과형, side effect, sequencing, UB 경계를 함께 추적한다.
## 13. 다음 Step
Step 8-1. `if`
## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.
- [cppreference: C operators](https://en.cppreference.com/w/c/language/operator_precedence.html)
- [GCC warning options](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html)
