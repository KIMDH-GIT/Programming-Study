# 9-12. 실습: factorial

factorial은 1부터 n까지의 양의 정수를 차례로 곱한 값이며 `0!`은 1이다.

## 1. 학습 목표
- 곱셈 누적의 초기값을 선택한다.
- `0!` 경계를 처리한다.
- 자료형 범위를 넘는 factorial을 정상 예제로 사용하지 않는다.

## 2. 선수 지식
Step 9-9의 누적 패턴과 C17 signed overflow 규칙이 필요하다.

## 3. 핵심 개념
`n! = 1 * 2 * ... * n`이며 빈 곱인 `0!`은 1이다. 곱셈의 항등값 1로 누적 변수를 초기화해야 한다. factorial은 매우 빨리 커지므로 예제는 작은 n만 사용한다. signed overflow가 발생하면 C17에서 undefined behavior다.

## 4. 문법
```c
int factorial = 1;
for (int factor = 2; factor <= n; ++factor) {
    factorial *= factor;
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int n = 5;
    int factorial = 1;
    for (int factor = 2; factor <= n; ++factor) {
        factorial *= factor;
    }
    printf("%d\n", factorial);
    return 0;
}
```

## 6. 코드 해석
누적값은 1에서 시작해 2, 6, 24, 120이 된다. `n == 0`이면 반복 조건이 처음부터 거짓이고 초기값 1이 그대로 결과가 된다.

## 7. 내부 동작
[C17 표준] signed 곱셈 결과가 표현 범위를 벗어나면 undefined behavior다. 특정 CPU의 wraparound를 표준 동작으로 기대하면 안 된다. 현재 `5!`은 최소 C `int` 범위에도 안전하다.

## 8. 자주 하는 실수
- 누적값을 0으로 초기화해 모든 결과를 0으로 만든다.
- 0!을 0으로 생각한다.
- 큰 입력도 `int`로 무조건 계산한다.
- overflow 사례를 직접 실행해 결과를 관찰하라고 요구한다.

## 9. 필수 실습
5!을 계산해 120을 출력하고 0!에서 반복이 0회인 이유를 설명한다. [실습 README](../../exercises/09-loops/9-12/README.md)

## 10. 추가 실습
- ★ 3! 계산
- ★★ 0! 흐름 추적
- ★★★ `int`의 범위를 확인한 뒤 안전한 최대 n을 조사만 하기

## 11. 확인 문제
1. 누적값을 1로 시작하는 이유는?
2. 0!의 값은?
3. 5! 계산에서 곱셈은 몇 번인가?
4. signed overflow의 C17 분류는?

## 12. 핵심 정리
factorial은 1에서 시작하는 곱셈 누적이며, 작은 범위에서만 안전성을 확인해 실행한다.

## 13. 다음 Step
[Step 9-13. 실습: 최대공약수](9-13-gcd.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.5
- [cppreference: Integer arithmetic](https://en.cppreference.com/w/c/language/operator_arithmetic.html)
