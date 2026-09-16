# 9-18. Part 9 종합 복습

Part 9에서는 반복의 상태 변화, 세 반복문, jump statement, 경계 설계와 중첩 반복을 실습했다.

## 1. 학습 목표
- 문제에 맞는 `while`, `do-while`, `for`를 선택한다.
- 반복 범위와 종료 가능성을 검증한다.
- overflow·0 제수·off-by-one 위험을 분류한다.

## 2. 선수 지식
Step 9-1부터 9-17까지의 반복 흐름과 실습을 모두 사용한다.

## 3. 핵심 개념
`while`은 선평가, `do-while`은 후평가, `for`는 초기화·조건·반복 식을 모아 쓴다. 모든 반복은 상태와 종료 조건으로 추적한다. `break`는 가장 안쪽 반복을 끝내고 `continue`는 종류별 다음 반복 단계로 이동한다. 경계는 첫 값·마지막 값·첫 거짓 검사로 검증한다.

## 4. 문법
```c
while (condition) { body; }
do { body; } while (condition);
for (initialization; condition; iteration) { body; }
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int sum = 0;
    for (int value = 1; value <= 5; ++value) {
        if (value == 3) {
            continue;
        }
        sum += value;
    }
    printf("%d\n", sum);
    return 0;
}
```

## 6. 코드 해석
1, 2, 4, 5를 누적해 12를 출력한다. 3에서 `continue`한 뒤 `for`의 iteration expression `++value`를 거쳐 다음 조건을 검사한다.

## 7. 내부 동작
[C17 표준] 반복 조건의 0은 거짓, 0이 아닌 값은 참이다. signed overflow와 0으로 나누기는 undefined behavior이므로 종료나 결과를 기대해 실행하지 않는다. [컴파일러 관점] 반복은 동등한 제어 흐름으로 최적화될 수 있지만 특정 명령과 1:1 대응하지 않는다.

## 8. 자주 하는 실수
- 변화 누락으로 종료하지 못한다.
- `<`와 `<=`를 의도와 다르게 고른다.
- `=`와 `==`를 혼동하고 대입 조건을 syntax error라고 단정한다.
- `continue`의 목적지를 모든 반복문에서 같다고 생각한다.
- 중첩 반복의 횟수와 `break` 범위를 잘못 계산한다.

## 9. 필수 실습
1~10에서 3의 배수를 제외한 합을 구하고 추적표로 검증한다. [실습 README](../../exercises/09-loops/9-18/README.md)

## 10. 추가 실습
- ★ 세 반복문의 평가 순서 비교표
- ★★ off-by-one 오류 세 가지 수정
- ★★★ GCD·소수·별 출력의 반복 요소 분석

## 11. 확인 문제
1. 처음부터 조건이 거짓일 때 0회 가능한 반복문은?
2. `for`에서 `continue` 다음 단계는?
3. `i = 0; i <= 10; ++i`의 본문 횟수는?
4. 내부 `break`가 종료하는 범위는?
5. signed overflow의 C17 분류는?
6. `while (x = 5)`는 왜 syntax error가 아닌가?

## 12. 핵심 정리
반복문은 문법보다 실행 순서와 경계가 중요하다. 정의된 범위 안에서 상태를 추적하면 종료, 횟수, 결과를 검증할 수 있다.

## 13. 다음 Step
Step 10-1. 함수 정의와 호출

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.8.5, 6.8.6
- [cppreference: Statements](https://en.cppreference.com/w/c/language/statements.html)
- [GCC Warning Options](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html)
