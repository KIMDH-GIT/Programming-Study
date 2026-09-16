# 5-18. Part 5 종합 복습

Part 5는 서식 문자열과 실제 인자형을 맞추고, 입력 결과를 반환값으로 검증하며, 작은 계산을 안전한 자료형과 단위로 출력하는 과정이었다.

## 1. 학습 목표

- 출력·입력 서식과 요구형을 종합한다.
- `scanf` 반환값과 buffer 한계를 설명한다.
- format mismatch UB를 판별한다.
- 세 계산 실습의 형·단위를 검토한다.

## 2. 선수 지식

Step 5-1~5-17 전체와 Part 2~4의 자료형·범위를 사용한다.

## 3. 핵심 개념

| 목적 | 핵심 규칙 |
|---|---|
| 정수 출력 | `%d`는 `int`, `%u/%x`는 `unsigned int` |
| 실수 출력 | `float/double`은 `%f`, `long double`은 `%Lf` |
| 입력 저장 | 스칼라는 주소를 전달하고 반환값을 확인 |
| 실수 입력 | `%f`→`float *`, `%lf`→`double *`, `%Lf`→`long double *` |
| 문자열 입력 | 배열 용량보다 1 작은 필드 폭 |
| 크기·주소 | `size_t`는 `%zu`, 포인터는 `(void *)`와 `%p` |

형식 불일치는 UB가 될 수 있고 컴파일 성공이 안전을 증명하지 않는다.

## 4. 문법

```c
printf("%d %.2f %zu\n", count, average, sizeof count);
matched = scanf("%d %lf", &count, &average);
```

## 5. 최소 코드 예제

```c
#include <stdio.h>

int main(void)
{
    int total_seconds = 7384;
    double celsius = 25.0;
    double fahrenheit = celsius * 9.0 / 5.0 + 32.0;

    printf("temperature=%.2f F\n", fahrenheit);
    printf("time=%d:%02d:%02d\n",
           total_seconds / 3600,
           total_seconds % 3600 / 60,
           total_seconds % 60);
    printf("int bytes=%zu\n", sizeof(int));
    return 0;
}
```

## 6. 코드 해석

실수 식은 화씨를 계산하고 정수 몫·나머지는 시간을 분해한다. 각 인자형은 서식과 맞고 `sizeof` 결과는 `%zu`로 출력한다.

## 7. 내부 동작

표준 입출력 함수는 stream의 문자와 C 값을 변환한다. 구체적인 터미널 동작·주소 표현·자료형 크기는 구현 환경의 영향을 받지만 형식 계약과 UB 규칙은 C17 기준으로 판단한다.

## 8. 자주 하는 실수

- 출력과 입력의 `%f` 규칙을 섞는다.
- `scanf` 반환값을 무시한다.
- `%s` 폭을 배열 용량과 같게 쓴다.
- 주소·크기·정수형에 임의 서식을 사용한다.
- 정수 나눗셈을 실수 나눗셈으로 예상한다.

## 9. 필수 실습

서식 대응표를 완성하고 온도·시간 계산을 한 프로그램에서 출력한다. [실습 README](../../exercises/05-input-output/5-18/README.md)를 따른다.

## 10. 추가 실습

- ★ **기초:** 모든 출력 지정의 요구형을 표로 쓴다.
- ★★ **응용:** 세 입력 실패 사례의 반환값을 예측한다.
- ★★★ **도전:** 안전한 입력 프로그램에 다음 Part 이후 필요한 기능을 목록화한다.

## 11. 확인 문제

1. `scanf`의 `%f`와 `%lf`는 각각 어떤 주소형을 요구하는가?
2. `char word[16]`에 알맞은 `%s` 폭은 얼마인가?
3. `%p`와 `%zu`가 각각 요구하는 값은 무엇인가?
4. format mismatch가 왜 UB가 될 수 있는가?
5. `17 / 5`와 `17.0 / 5.0`의 차이는 무엇인가?
6. 두 변환을 요청한 `scanf`가 1을 반환하면 무엇을 뜻하는가?

## 12. 핵심 정리

Part 5의 완료 기준은 서식 문자를 외우는 데 그치지 않는다. 실제 인자형·주소형·버퍼 용량·반환값·산술 피연산자형을 함께 확인해야 한다.

## 13. 다음 Step

Step 6-1. implicit conversion

## 14. 참고 자료

- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5, 7.19, 7.21.6.
- [cppreference: C input/output](https://en.cppreference.com/w/c/io.html)
- [GCC Warning Options](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html)
