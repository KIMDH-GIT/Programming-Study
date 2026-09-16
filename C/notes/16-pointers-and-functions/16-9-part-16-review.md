# 16-9. Part 16 종합 복습

Part 16에서는 value·pointer·array parameters가 모두 C의 pass-by-value 안에서 어떻게 caller objects와 상호작용하는지 학습했다.

## 1. 학습 목표
- argument value와 parameter object를 종합해 구분한다.
- pointer target 수정과 parameter 재지정을 구분한다.
- array adjustment·count·`sizeof` 함정을 설명한다.

## 2. 선수 지식
Step 16-1부터 16-8까지와 Part 14~15의 pointer·array conversion을 사용한다.

## 3. 핵심 개념
argument value는 parameter object를 초기화한다. pointer parameter도 copied pointer value를 저장한다. `*p` 또는 `p[i]`는 valid caller object를 지정할 수 있지만 `p` 자체 재지정은 local parameter만 바꾼다. array parameter notation은 pointer type으로 adjusted되고 count는 별도로 전달한다.

## 4. 문법
```c
void set_value(int *pointer);
void update_array(int values[], size_t count);
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

void set_value(int *pointer)
{
    *pointer = 50;
}

void add_one(int values[], size_t count)
{
    for (size_t i = 0; i < count; ++i) {
        ++values[i];
    }
}

int main(void)
{
    int number = 10;
    int values[3] = {1, 2, 3};
    size_t count = sizeof(values) / sizeof(values[0]);

    set_value(&number);
    add_one(values, count);

    printf("%d\n", number);
    for (size_t i = 0; i < count; ++i) {
        printf("%d\n", values[i]);
    }
    return 0;
}
```

## 6. 코드 해석
set_value는 copied pointer로 number를 50으로 바꾼다. add_one은 caller array first-element pointer와 count를 받아 elements를 2,3,4로 바꾼다. 두 calls 모두 pass-by-value다.

## 7. 내부 동작
[C17 표준] function call conversions, pointer indirection, array-to-pointer conversion과 parameter adjustment가 적용된다. `sizeof` caller array는 전체 array size지만 function array parameter의 `sizeof`는 adjusted pointer size다. 2차원 array parameter는 pointer to row type이 필요하며 `int **`와 같지 않지만 actual Part 범위를 넘어 자세한 예제는 생략한다. [ABI/CPU] argument registers와 stack은 implementation details다.

## 8. 자주 하는 실수
- C가 pointer parameter에서 call by reference를 쓴다고 말한다.
- `p = NULL`과 `*p = value`를 혼동한다.
- array 전체가 parameter object로 복사된다고 생각한다.
- parameter `sizeof`로 caller array count를 계산한다.
- 모든 pointer parameters가 NULL을 허용한다고 가정한다.

## 9. 필수 실습
pointer로 scalar caller와 array elements를 수정하고 pass-by-value flow를 설명한다. [실습 README](../../exercises/16-pointers-and-functions/16-9/README.md)

## 10. 추가 실습
- ★ value vs pointer parameter table
- ★★ swap과 array update 결합
- ★★★ call conversion·parameter·target diagram

## 11. 확인 문제
1. C의 argument passing 방식은?
2. pointer parameter 재지정이 caller pointer를 바꾸는가?
3. `*p` 수정은 무엇을 바꿀 수 있는가?
4. array parameter notation은 무엇으로 adjusted되는가?
5. parameter `sizeof`가 caller array size인가?
6. count를 별도로 전달하는 이유는?
7. 2차원 array가 `int **`인가?

## 12. 핵심 정리
모든 arguments는 value로 전달되며 pointer values를 통해 caller objects를 지정할 때만 그 objects의 state가 바뀐다.

## 13. 다음 Step
Step 17-1. `const` 객체

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.3.2.1, 6.5.2.2, 6.5.3.2, 6.7.6.3, 6.9.1
- [cppreference: Function call](https://en.cppreference.com/w/c/language/operator_other.html#Function_call)
- [cppreference: Arrays](https://en.cppreference.com/w/c/language/array.html)
