# 9-11. 실습: 구구단

구구단 한 단은 고정된 피승수와 1부터 9까지 변하는 승수의 반복이다.

## 1. 학습 목표
- 고정값과 반복 변수를 구분한다.
- 곱셈 결과를 매 반복 계산한다.
- 출력 형식과 범위를 정확히 맞춘다.

## 2. 선수 지식
Step 9-4의 `for`와 Part 7의 곱셈 연산을 안다.

## 3. 핵심 개념
2단을 출력할 때 2는 고정되고 승수만 1~9로 변한다. 결과를 별도 변수에 저장해도 되고 `2 * multiplier`를 `printf` 인자로 계산해도 된다. 모든 단을 출력하는 것은 중첩 반복이지만 이번 Step의 최소 예제는 한 단에 집중한다.

## 4. 문법
```c
for (int multiplier = 1; multiplier <= 9; ++multiplier) {
    printf("2 x %d = %d\n", multiplier, 2 * multiplier);
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int dan = 2;
    for (int multiplier = 1; multiplier <= 9; ++multiplier) {
        printf("%d x %d = %d\n", dan, multiplier, dan * multiplier);
    }
    return 0;
}
```

## 6. 코드 해석
`dan`은 2로 유지되고 `multiplier`는 1~9로 변한다. 결과는 2부터 18까지 2씩 증가하며 총 9줄이다.

## 7. 내부 동작
[C 언어 관점] 각 곱셈 결과는 `int`이고 이 범위에서는 overflow가 없다. `dan`은 예제에서 값을 바꾸지 않는 고정 역할의 변수다.

## 8. 자주 하는 실수
- 승수를 0부터 시작한다.
- `< 9`로 9단계째를 빠뜨린다.
- format specifier와 인자 수를 맞추지 않는다.
- 결과를 누적해 이전 곱을 섞는다.

## 9. 필수 실습
입력 없이 2단을 1~9까지 출력한다. [실습 README](../../exercises/09-loops/9-11/README.md)

## 10. 추가 실습
- ★ 5단 출력
- ★★ 9부터 1까지 역순 출력
- ★★★ 2단부터 4단까지 중첩 반복으로 출력

## 11. 확인 문제
1. 고정값과 반복 변수는 각각 무엇인가?
2. 본문은 몇 번 실행되는가?
3. `< 9`를 쓰면 어떤 줄이 빠지는가?
4. 모든 단을 출력하려면 반복이 몇 겹 필요한가?

## 12. 핵심 정리
한 단은 고정된 단과 1~9 승수의 반복이며, 범위와 출력 인자 대응을 확인한다.

## 13. 다음 Step
[Step 9-12. 실습: factorial](9-12-factorial.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.5, 6.8.5
- [cppreference: for loop](https://en.cppreference.com/w/c/language/for.html)
