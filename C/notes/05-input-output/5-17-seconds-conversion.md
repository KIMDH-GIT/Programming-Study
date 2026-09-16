# 5-17. 초를 시·분·초로 변환

전체 초를 시·분·초로 나누는 과정은 정수 몫과 나머지를 실제 단위 변환에 적용한다.

## 1. 학습 목표

- 전체 초에서 시간·분·초를 계산한다.
- `/`와 `%`의 역할을 구별한다.
- 중간 나머지의 의미를 설명한다.

## 2. 선수 지식

Step 5-13의 정수 나눗셈·나머지와 `%d` 출력을 사용한다.

## 3. 핵심 개념

1시간은 3600초, 1분은 60초다. 시간은 `total / 3600`, 시간으로 쓰고 남은 초는 `total % 3600`이다. 분은 남은 초를 60으로 나눈 몫, 최종 초는 60으로 나눈 나머지다.

이번 예제는 음이 아닌 전체 초를 전제로 한다.

## 4. 문법

```c
int hours = total / 3600;
int remaining = total % 3600;
int minutes = remaining / 60;
int seconds = remaining % 60;
```

## 5. 최소 코드 예제

```c
#include <stdio.h>

int main(void)
{
    int total = 7384;
    int hours = total / 3600;
    int remaining = total % 3600;
    int minutes = remaining / 60;
    int seconds = remaining % 60;

    printf("%d seconds = %d:%02d:%02d\n",
           total, hours, minutes, seconds);
    return 0;
}
```

## 6. 코드 해석

7384초에서 2시간을 떼면 184초가 남는다. 그중 3분을 떼면 4초가 남는다. `%02d`는 분과 초를 최소 두 자리로 표시한다.

## 7. 내부 동작

모든 피연산자가 `int`라서 정수 나눗셈이다. 값 범위가 커지면 `int` 범위를 확인해야 하지만 이번 예제는 안전한 범위다.

## 8. 자주 하는 실수

- 분을 전체 초에서 바로 구해 60 이상으로 만든다.
- 시간 계산 뒤 나머지를 갱신하지 않는다.
- `%`를 서식의 `%`와 같은 역할로 생각한다.
- 음수 입력 규칙을 정하지 않는다.

## 9. 필수 실습

7384초를 시·분·초로 변환하고 손계산과 비교한다. [실습 README](../../exercises/05-input-output/5-17/README.md)를 따른다.

## 10. 추가 실습

- ★ **기초:** 59, 60, 3600초를 계산한다.
- ★★ **응용:** 86399초를 계산한다.
- ★★★ **도전:** 허용할 입력 범위를 문서화한다.

## 11. 확인 문제

1. 시간을 구하는 식은 무엇인가?
2. 분 계산에 전체 초가 아닌 나머지를 쓰는 이유는 무엇인가?
3. 최종 초는 어떤 식으로 구하는가?
4. `%02d`의 2는 무엇을 뜻하는가?

## 12. 핵심 정리

큰 단위의 몫을 구한 뒤 나머지를 다음 단위로 나눈다. `/`는 단위 개수, `%`는 다음 단계에 남길 양을 만든다.

## 13. 다음 Step

[Step 5-18. Part 5 종합 복습](5-18-part-5-review.md)

## 14. 참고 자료

- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.5.
- [cppreference: arithmetic operators](https://en.cppreference.com/w/c/language/operator_arithmetic.html)
- [cppreference: `printf`](https://en.cppreference.com/w/c/io/fprintf.html)
