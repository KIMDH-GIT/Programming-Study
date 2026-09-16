# 5-12. 잘못된 format specifier와 Undefined Behavior

형식 문자열은 가변 인수의 형을 해석하는 계약이다. 변환 지정과 실제 인자형이 맞지 않으면 단순한 표시 오류가 아니라 Undefined Behavior가 될 수 있다.

## 1. 학습 목표

- format mismatch가 UB가 되는 이유를 설명한다.
- 컴파일러 진단과 C 실행 의미를 구별한다.
- 출력 인자와 입력 포인터의 불일치를 판별한다.

## 2. 선수 지식

Step 5-1~5-11의 출력·입력 지정과 객체 주소를 사용한다.

## 3. 핵심 개념

```c
/* 실행하지 말 것 */
printf("%d\n", 3.14);
scanf("%d", &some_double);
```

첫 호출은 `%d`가 `int`를 기대하지만 실제 인자는 `double`이다. 둘째 호출은 `int *`를 기대하지만 다른 포인터형을 받는다. 가변 인수 함수는 런타임 형 정보를 자동 검사하지 않으므로 라이브러리가 잘못된 방식으로 인자를 읽거나 저장하게 된다.

문자열 리터럴인 형식은 GCC가 `-Wformat` 계열 진단으로 많은 오류를 찾을 수 있다. 경고가 없다는 사실이 모든 동적 형식 문자열의 안전을 증명하지는 않는다.

## 4. 문법

```c
printf("%d %.2f\n", integer_value, double_value);
scanf("%d %lf", &integer_value, &double_value);
```

## 5. 최소 코드 예제

```c
#include <stdio.h>

int main(void)
{
    int count = 3;
    double ratio = 0.5;

    printf("count=%d ratio=%.2f\n", count, ratio);
    printf("addresses=%p,%p\n",
           (void *)&count, (void *)&ratio);
    return 0;
}
```

## 6. 코드 해석

1. `%d`와 `count`의 `int`가 맞는다.
2. `%f`와 `ratio`의 `double`이 맞는다.
3. `%p` 인자는 둘 다 `void *`로 변환했다.
4. 모든 변환 지정이 정확한 인자 하나와 대응한다.

## 7. 내부 동작

호출 규약은 정수·부동소수점 인자를 서로 다른 위치에 전달할 수 있다. 형식 불일치는 라이브러리가 잘못된 위치나 크기로 값을 읽게 할 수 있다. 특정 실행 결과를 UB의 규칙처럼 일반화하지 않는다.

## 8. 자주 하는 실수

- 경고가 뜨면 모두 syntax error라고 부른다.
- 우연히 출력됐으니 정의된 동작이라고 생각한다.
- `scanf` 포인터형 불일치를 단순 변환으로 본다.
- 경고를 끄거나 무시해 해결한다.

## 9. 필수 실습

올바른 형식 대응표를 만들고, 별도 잘못된 예는 컴파일 진단만 확인한 뒤 실행하지 않는다. [실습 README](../../exercises/05-input-output/5-12/README.md)를 따른다.

## 10. 추가 실습

- ★ **기초:** 다섯 형과 올바른 서식을 연결한다.
- ★★ **응용:** GCC 진단 문구를 분류한다.
- ★★★ **도전:** ABI 관점에서 mismatch 위험을 조사한다.

## 11. 확인 문제

1. format mismatch는 항상 syntax error인가?
2. `%d`에 `double`을 넘기면 왜 위험한가?
3. `scanf("%lf", &float_object)`는 올바른가?
4. 컴파일 성공이 정의된 동작을 보장하는가?

## 12. 핵심 정리

형식 문자열과 실제 인자형의 불일치는 UB가 될 수 있다. 경고를 수정하고 각 지정의 요구형을 직접 확인해야 한다.

## 13. 다음 Step

[Step 5-13. 계산 실습용 산술 연산자와 나눗셈 예고](5-13-arithmetic-operators-preview.md)

## 14. 참고 자료

- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 7.21.6.1~2.
- [cppreference: formatted output](https://en.cppreference.com/w/c/io/fprintf.html)
- [GCC Format Warnings](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html#index-Wformat)
