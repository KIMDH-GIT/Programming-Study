# 9-14. 실습: 소수 판별

소수는 2 이상의 정수 중 1과 자기 자신만 양의 약수로 가지는 수다.

## 1. 학습 목표
- 2 미만을 먼저 제외한다.
- 가능한 약수를 반복해 나눗셈 여부를 검사한다.
- 약수를 찾으면 즉시 반복을 종료한다.

## 2. 선수 지식
Step 9-5의 `break`, Step 9-13의 나머지 연산을 사용한다.

## 3. 핵심 개념
`candidate < 2`는 소수가 아니다. 2부터 `candidate - 1`까지 약수를 검사해 하나라도 나누어떨어지면 합성수다. 더 빠른 제곱근 경계도 가능하지만, 곱셈 overflow를 피하는 조건 설계가 필요하므로 이 Step은 작은 값과 단순 경계에 집중한다.

## 4. 문법
```c
int is_prime = candidate >= 2;
for (int divisor = 2; divisor < candidate && is_prime; ++divisor) {
    if (candidate % divisor == 0) {
        is_prime = 0;
    }
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int candidate = 17;
    int is_prime = candidate >= 2;
    for (int divisor = 2; divisor < candidate && is_prime; ++divisor) {
        if (candidate % divisor == 0) {
            is_prime = 0;
        }
    }
    if (is_prime) {
        printf("prime\n");
    } else {
        printf("not prime\n");
    }
    return 0;
}
```

## 6. 코드 해석
17은 2~16 어느 수로도 나누어떨어지지 않아 `is_prime`이 1로 남는다. 후보가 1이면 초기값이 0이고 `&&` short-circuit로 반복을 시작하지 않는다.

## 7. 내부 동작
[C17 표준] `&&`는 왼쪽이 0이면 오른쪽을 평가하지 않는다. 모든 divisor는 2 이상이므로 `%`의 제수는 0이 아니다. 마지막 `if-else`는 Part 8의 조건문으로 판별 결과에 맞는 문장을 출력한다.

## 8. 자주 하는 실수
- 1을 소수로 분류한다.
- 자기 자신까지 나누고 합성수로 판정한다.
- flag를 반복마다 다시 1로 만든다.
- `divisor * divisor <= candidate`를 큰 signed 값에도 무조건 안전하다고 쓴다.

## 9. 필수 실습
17과 18을 각각 판별하고 처음 발견되는 약수를 기록한다. [실습 README](../../exercises/09-loops/9-14/README.md)

## 10. 추가 실습
- ★ 2 판별
- ★★ 1 판별
- ★★★ 약수를 찾으면 `break`하는 버전 작성

## 11. 확인 문제
1. 소수의 최소값은?
2. 2를 판별할 때 본문은 몇 번 실행되는가?
3. 자기 자신을 검사 범위에서 제외하는 이유는?
4. 곱셈 기반 경계에서 주의할 위험은?

## 12. 핵심 정리
2 미만을 제외하고 2부터 자기 자신 미만까지 약수 존재 여부를 검사하면 작은 정수를 안전하게 판별할 수 있다.

## 13. 다음 Step
[Step 9-15. 실습: 범위 내 모든 소수](9-15-primes-in-range.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.13, 6.8.5
- [cppreference: Logical operators](https://en.cppreference.com/w/c/language/operator_logical.html)
