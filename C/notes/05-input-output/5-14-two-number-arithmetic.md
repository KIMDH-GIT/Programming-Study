# 5-14. 두 수의 사칙연산

두 값을 준비하고 계산식과 출력 서식을 한 프로그램에 연결한다. 이번 최소 예제는 0 제수 검사가 필요한 입력 분기를 아직 배우지 않았으므로 안전한 고정값을 사용한다.

## 1. 학습 목표

- 두 피연산자의 사칙연산을 변수에 저장한다.
- 계산형과 출력 서식을 맞춘다.
- 입력 기반 나눗셈에 검증이 필요한 이유를 설명한다.

## 2. 선수 지식

Step 5-13의 산술 연산자와 정수 출력 형식을 사용한다.

## 3. 핵심 개념

계산 프로그램은 입력·처리·출력을 구별하면 읽기 쉽다. 정수 나눗셈은 제수가 0이 아니어야 한다. 아직 `if`를 배우지 않았으므로 최소 예제의 제수는 안전한 상수 4다.

## 4. 문법

```c
int sum = left + right;
int quotient = left / right;
```

## 5. 최소 코드 예제

```c
#include <stdio.h>

int main(void)
{
    int left = 20;
    int right = 4;

    printf("sum=%d\n", left + right);
    printf("difference=%d\n", left - right);
    printf("product=%d\n", left * right);
    printf("quotient=%d\n", left / right);
    return 0;
}
```

## 6. 코드 해석

두 객체는 처리 전 입력 역할을 한다. 각 식은 `int` 결과를 만들고 `%d`로 출력된다. 제수 4는 0이 아니다.

## 7. 내부 동작

컴파일러는 상수 계산을 번역 중 미리 수행할 수도 있다. 최적화 여부와 상관없이 관찰되는 출력은 C17의 정의된 산술 결과와 같아야 한다.

## 8. 자주 하는 실수

- 입력·계산·출력을 한 표현에 과도하게 섞는다.
- 제수 0 가능성을 무시한다.
- 정수 몫에서 소수 부분을 기대한다.
- overflow 가능성을 무시한다.

## 9. 필수 실습

안전한 두 정수의 합·차·곱·몫을 출력한다. [실습 README](../../exercises/05-input-output/5-14/README.md)를 따른다.

## 10. 추가 실습

- ★ **기초:** 다른 고정값으로 계산한다.
- ★★ **응용:** 나머지도 출력한다.
- ★★★ **도전:** 입력 버전에 필요한 검증 목록을 설계한다.

## 11. 확인 문제

1. 최소 예제가 고정값을 쓰는 이유는 무엇인가?
2. 정수 몫의 소수 부분은 어떻게 되는가?
3. 제수에 필요한 조건은 무엇인가?
4. 계산형과 출력 서식은 어떻게 맞추는가?

## 12. 핵심 정리

사칙연산 프로그램도 입력·처리·출력 역할을 분리한다. 나눗셈에는 0 제수 검사가 필요하며 분기 학습 뒤 입력 버전으로 확장한다.

## 13. 다음 Step

[Step 5-15. BMI 계산](5-15-bmi-calculation.md)

## 14. 참고 자료

- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.5~6.5.6.
- [cppreference: arithmetic operators](https://en.cppreference.com/w/c/language/operator_arithmetic.html)
- [cppreference: `printf`](https://en.cppreference.com/w/c/io/fprintf.html)
