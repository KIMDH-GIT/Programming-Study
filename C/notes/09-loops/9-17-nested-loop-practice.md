# 9-17. 실습: 중첩 반복문

행과 열 번호의 조합을 계산하면 중첩 반복의 상태 변화를 종합적으로 확인할 수 있다.

## 1. 학습 목표
- 두 반복 변수로 작은 곱셈표를 만든다.
- 출력 순서와 전체 횟수를 예측한다.
- 각 반복의 경계를 독립적으로 검토한다.

## 2. 선수 지식
Step 9-7의 실행 관계와 Step 9-11의 구구단을 사용한다.

## 3. 핵심 개념
외부 `left`가 1~3을 순회하고 각 값마다 내부 `right`가 1~3을 순회하면 9개 조합이 생긴다. 한 조합의 계산이 끝나도 외부 값은 내부 전체가 끝날 때까지 유지된다.

## 4. 문법
```c
for (int left = 1; left <= 3; ++left) {
    for (int right = 1; right <= 3; ++right) {
        printf("%d ", left * right);
    }
    printf("\n");
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    for (int left = 1; left <= 3; ++left) {
        for (int right = 1; right <= 3; ++right) {
            printf("%d ", left * right);
        }
        printf("\n");
    }
    return 0;
}
```

## 6. 코드 해석
첫 줄은 1, 2, 3, 둘째 줄은 2, 4, 6, 셋째 줄은 3, 6, 9다. 내부 본문은 9회, newline은 3회 실행된다.

## 7. 내부 동작
[C 언어 관점] 내부 초기화는 외부 반복마다 세 번 수행된다. 두 loop variable은 각 `for`의 block scope에 속하며 서로 다른 객체다. 결과 곱셈은 모두 `int` 범위 안이다.

## 8. 자주 하는 실수
- 내부 조건에 `left <= 3`을 써 내부 반복이 종료되지 않게 한다.
- 내부 변화에서 `++left`를 써 외부 상태를 바꾼다.
- newline을 내부 반복 안에 둔다.
- 횟수를 3 + 3으로 계산한다.

## 9. 필수 실습
3x3 곱셈표를 만들고 9개 조합 순서를 먼저 기록한다. [실습 README](../../exercises/09-loops/9-17/README.md)

## 10. 추가 실습
- ★ 2x4 덧셈표
- ★★ 행마다 합 출력
- ★★★ 내부 `continue`로 결과가 4인 조합 건너뛰기

## 11. 확인 문제
1. 내부 본문은 총 몇 회인가?
2. 내부 초기화는 몇 회인가?
3. `++left`를 내부 변화식에 쓰면 무엇이 깨지는가?
4. 마지막 조합의 두 값은?

## 12. 핵심 정리
각 반복 변수는 자기 초기화·조건·변화를 가져야 하며 전체 조합 수는 범위 크기의 곱이다.

## 13. 다음 Step
[Step 9-18. Part 9 종합 복습](9-18-part-9-review.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.8.5
- [cppreference: Scope](https://en.cppreference.com/w/c/language/scope.html)
