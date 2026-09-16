# 9-7. 중첩 반복문

반복문 본문에 다른 반복문을 두면 외부 반복 1회마다 내부 반복 전체가 실행된다.

## 1. 학습 목표
- 외부·내부 반복의 실행 관계를 설명한다.
- 작은 범위에서 총 실행 횟수를 계산한다.
- 가장 안쪽 반복에 대한 `break`의 범위를 판단한다.

## 2. 선수 지식
Step 9-4의 `for`, Step 9-5의 jump statement, Step 9-6의 범위 추적을 사용한다.

## 3. 핵심 개념
외부 반복이 한 번 시작되면 내부 반복은 초기화부터 종료까지 전부 수행한다. 이후 외부 반복의 변화가 일어나고 다음 외부 반복에서 내부 변수도 다시 초기화된다. 각 2회와 3회라면 내부 본문은 `2 * 3 = 6`회지만 조건 검사 횟수는 더 많다.

## 4. 문법
```c
for (outer initialization; outer condition; outer update) {
    for (inner initialization; inner condition; inner update) {
        body;
    }
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    for (int row = 1; row <= 2; ++row) {
        for (int column = 1; column <= 3; ++column) {
            printf("(%d, %d)\n", row, column);
        }
    }
    return 0;
}
```

## 6. 코드 해석
`row == 1`일 때 column 1~3이 모두 출력되고, `row == 2`에서도 다시 column 1~3이 출력된다. 순서는 `(1,1) (1,2) (1,3) (2,1) (2,2) (2,3)`이며 내부 본문은 6회다.

## 7. 내부 동작
[C 언어 관점] 두 반복문은 각각 독립된 iteration statement다. 내부의 `break`는 내부 반복만 종료한다. 컴파일러는 최적화할 수 있지만 소스의 관찰 가능한 출력 순서를 보존해야 한다.

## 8. 자주 하는 실수
- 내부 변수가 외부 반복 전체에서 한 번만 초기화된다고 생각한다.
- 횟수를 더해서 계산한다.
- 같은 변수 이름을 무리하게 재사용해 흐름을 혼동한다.
- 내부 `break`가 두 반복을 모두 끝낸다고 생각한다.

## 9. 필수 실습
2행 3열의 좌표를 출력하고 실행 순서를 손으로 먼저 적는다. [실습 README](../../exercises/09-loops/9-7/README.md)

## 10. 추가 실습
- ★ 3행 2열 좌표
- ★★ 각 외부 반복 시작을 별도 출력
- ★★★ 내부에서 column 2일 때 `break`하고 총 횟수 계산

## 11. 확인 문제
1. 외부 4회, 내부 3회면 내부 본문은 몇 회인가?
2. 내부 변수는 언제 다시 초기화되는가?
3. 내부 `break`는 무엇을 종료하는가?
4. 예제의 여섯 번째 출력은 무엇인가?

## 12. 핵심 정리
중첩 반복은 외부 1회 → 내부 전체의 순서이며 작은 값으로 순서와 곱셈 횟수를 검증한다.

## 13. 다음 Step
[Step 9-8. 실습: 1부터 100까지 출력](9-8-print-1-to-100.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.8.5
- [cppreference: for loop](https://en.cppreference.com/w/c/language/for.html)
