# 16-7. 배열 길이를 별도로 전달하는 이유

adjusted pointer parameter 하나에는 caller array element count가 자동 포함되지 않으므로 count를 별도 parameter로 전달한다.

## 1. 학습 목표
- pointer parameter와 element count의 역할을 구분한다.
- `size_t count`로 bounds를 정한다.
- caller가 pointer와 count contract를 함께 지키는 이유를 설명한다.

## 2. 선수 지식
Step 16-6의 adjustment와 Part 15의 pointer bounds를 안다.

## 3. 핵심 개념
function은 first element pointer value만으로 caller array가 몇 elements인지 일반적으로 알 수 없다. 따라서 `values`와 `count`를 함께 전달한다. caller가 actual count보다 큰 값을 넘기면 function loop가 bounds 밖으로 접근할 수 있으므로 두 arguments는 하나의 contract를 이룬다.

## 4. 문법
```c
int sum_values(int values[], size_t count);
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int sum_values(int values[], size_t count)
{
    int sum = 0;
    for (size_t i = 0; i < count; ++i) {
        sum += values[i];
    }
    return sum;
}

int main(void)
{
    int values[5] = {10, 20, 30, 40, 50};
    size_t count = sizeof(values) / sizeof(values[0]);

    printf("count: %zu\n", count);
    printf("sum: %d\n", sum_values(values, count));
    return 0;
}
```

## 6. 코드 해석
caller는 actual array object에서 count 5를 계산해 pointer argument와 함께 전달한다. function은 indices 0~4만 읽어 sum 150을 반환한다.

## 7. 내부 동작
[C17 표준] `values` parameter는 adjusted `int *`, count는 `size_t` value parameter다. 둘 다 pass-by-value된다. pointer value 안에 count metadata가 자동 저장된다는 보장은 없다. signed sum은 현재 small values에서 safe하다.

## 8. 자주 하는 실수
- function 안에서 `sizeof(values)/sizeof(values[0])`를 사용한다.
- pointer가 array length를 자동으로 안다고 생각한다.
- actual array보다 큰 count를 전달한다.
- `size_t`를 `%d`로 출력한다.

## 9. 필수 실습
actual array count를 caller에서 계산해 sum function에 전달한다. [실습 README](../../exercises/16-pointers-and-functions/16-7/README.md)

## 10. 추가 실습
- ★ three-element sum
- ★★ empty range count 0
- ★★★ pointer/count contract 위반을 실행 없이 분석

## 11. 확인 문제
1. pointer parameter가 count를 자동 포함하는가?
2. count는 어디에서 계산하는가?
3. count type은?
4. too-large count의 위험은?
5. 두 parameters 모두 어떤 방식으로 전달되는가?

## 12. 핵심 정리
array function은 adjusted pointer와 별도 element count를 함께 받아 bounds contract를 명시해야 한다.

## 13. 다음 Step
[Step 16-8. 함수에서 배열 원소 변경](16-8-modify-array-elements.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.3.2.1, 6.5.2.2, 6.7.6.3, 7.19
- [cppreference: size_t](https://en.cppreference.com/w/c/types/size_t.html)
