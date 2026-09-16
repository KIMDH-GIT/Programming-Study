# 11-6. 배열 범위 밖 접근과 Undefined Behavior

배열 element를 읽거나 쓸 때 index가 유효 범위를 벗어나면 C17에서 behavior가 정의되지 않는다.

## 1. 학습 목표
- N개 배열의 유효 index 0~N-1을 판별한다.
- out-of-bounds access를 Undefined Behavior로 분류한다.
- 언어 규칙과 sanitizer·운영체제 결과를 구분한다.

## 2. 선수 지식
Step 11-2의 subscript와 Step 11-5의 element count를 안다.

## 3. 핵심 개념
5개 배열의 유효 index는 0~4다. `values[5]`와 `values[-1]`을 element로 읽거나 쓰는 것은 범위 밖 접근이다. 결과가 특정 쓰레기 값, 0, 또는 segmentation fault라고 예측할 수 없다. 정의된 동작이 아니므로 실행 실습으로 관찰하지 않는다.

## 4. 문법
```c
if (index >= 0 && index < element_count) {
    value = array[index];
}
```

잘못된 예 `array[element_count]`는 분석만 하고 실행하지 않는다.

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int values[5] = {10, 20, 30, 40, 50};
    int index = 4;

    if (index >= 0 && index < 5) {
        printf("%d\n", values[index]);
    } else {
        printf("invalid index\n");
    }
    return 0;
}
```

## 6. 코드 해석
index 4는 0 이상 5 미만이므로 마지막 element 50을 안전하게 읽는다. index가 5라면 array access를 평가하지 않고 오류 문장만 출력한다.

## 7. 내부 동작
[C17 표준] 배열 객체 범위 밖의 객체를 subscript로 지정해 접근하면 Undefined Behavior다. C 언어 자체는 Java/Python식 index exception을 요구하지 않는다. [도구 관점] compiler 경고나 sanitizer instrumentation이 일부 오류를 탐지할 수 있지만 모든 일반 C 실행에 자동 bounds checking이 있다는 뜻은 아니다.

## 8. 자주 하는 실수
- 마지막 index를 count와 같게 쓴다.
- 범위 밖 접근은 항상 segmentation fault라고 말한다.
- compiler가 경고하지 않았으므로 안전하다고 판단한다.
- UB 코드를 직접 실행해 나온 값을 규칙처럼 기록한다.

## 9. 필수 실습
사용할 index를 먼저 범위 검사하고 유효할 때만 element를 출력한다. [실습 README](../../exercises/11-arrays/11-6/README.md)

## 10. 추가 실습
- ★ index 0과 4 검증
- ★★ index -1과 5는 접근 없이 거부
- ★★★ 다섯 index 후보를 안전성 표로 분류

## 11. 확인 문제
1. 5개 배열의 유효 index 범위는?
2. `values[5]` 접근의 C17 분류는?
3. 항상 segmentation fault가 발생하는가?
4. compiler 경고가 없으면 안전한가?
5. sanitizer와 C 언어 자체의 차이는?

## 12. 핵심 정리
index는 접근 전에 0~N-1인지 보장하며, out-of-bounds는 결과를 예측할 수 없는 Undefined Behavior다.

## 13. 다음 Step
[Step 11-7. 합과 평균](11-7-sum-average.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 3.4.3, 6.5.2.1
- [cppreference: Undefined behavior](https://en.cppreference.com/w/c/language/behavior.html)
- [GCC: Instrumentation Options](https://gcc.gnu.org/onlinedocs/gcc/Instrumentation-Options.html)
