# 9-15. 실습: 범위 내 모든 소수

범위의 각 후보에 대해 소수 판별 반복을 다시 수행하면 모든 소수를 찾을 수 있다.

## 1. 학습 목표
- 후보 반복과 약수 반복을 중첩한다.
- 후보마다 판별 상태를 다시 초기화한다.
- 작은 범위의 결과를 직접 검산한다.

## 2. 선수 지식
Step 9-7의 중첩 반복과 Step 9-14의 소수 판별이 필요하다.

## 3. 핵심 개념
외부 반복은 후보 2~20을 선택하고 내부 반복은 각 후보의 약수를 검사한다. `is_prime`은 후보가 바뀔 때마다 1로 초기화해야 한다. 내부에서 약수를 찾으면 0으로 바꾸고 `break`하여 불필요한 검사를 끝낸다.

## 4. 문법
```c
for (int candidate = 2; candidate <= limit; ++candidate) {
    int is_prime = 1;
    for (int divisor = 2; divisor < candidate; ++divisor) {
        /* 판별 */
    }
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    for (int candidate = 2; candidate <= 20; ++candidate) {
        int is_prime = 1;
        for (int divisor = 2; divisor < candidate; ++divisor) {
            if (candidate % divisor == 0) {
                is_prime = 0;
                break;
            }
        }
        if (is_prime) {
            printf("%d\n", candidate);
        }
    }
    return 0;
}
```

## 6. 코드 해석
각 후보마다 내부 반복이 새로 시작한다. 출력은 2, 3, 5, 7, 11, 13, 17, 19다. 내부 `break`는 약수 검사만 끝내며 외부 후보 반복은 계속된다.

## 7. 내부 동작
[C 언어 관점] `is_prime`은 외부 본문의 block scope 자동 객체이며 외부 반복마다 초기화된다. 내부 `break`의 대상은 가장 안쪽 `for`뿐이다.

## 8. 자주 하는 실수
- flag를 외부 반복 전에 한 번만 초기화한다.
- 내부 `break`가 외부 반복까지 끝난다고 생각한다.
- 후보를 1부터 시작해 1을 출력한다.
- 아직 배우지 않은 배열에 결과를 저장하려 한다.

## 9. 필수 실습
2~20의 모든 소수를 출력하고 결과를 손으로 검산한다. [실습 README](../../exercises/09-loops/9-15/README.md)

## 10. 추가 실습
- ★ 2~10
- ★★ 10~30
- ★★★ 후보별 나머지 검사 횟수 출력

## 11. 확인 문제
1. 반복은 왜 두 겹인가?
2. flag는 언제 초기화하는가?
3. 내부 `break` 뒤에는 어디로 가는가?
4. 20 이하 소수는 몇 개인가?

## 12. 핵심 정리
외부 반복은 후보, 내부 반복은 약수를 담당하고 판별 상태는 후보마다 새로 만든다.

## 13. 다음 Step
[Step 9-16. 실습: 별 출력](9-16-star-patterns.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.8.5, 6.8.6.3
- [cppreference: break](https://en.cppreference.com/w/c/language/break.html)
