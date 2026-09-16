# 16-8. 함수에서 배열 원소 변경

adjusted pointer parameter로 caller array elements를 지정하면 function이 그 actual element objects를 수정할 수 있다.

## 1. 학습 목표
- caller array element가 바뀌는 정확한 흐름을 설명한다.
- pointer와 count를 이용해 bounds 안에서 수정한다.
- array 전달을 call by reference라고 부르지 않는다.

## 2. 선수 지식
Step 16-3의 pointed-to modification과 Step 16-7의 count contract를 사용한다.

## 3. 핵심 개념
caller array expression이 first element pointer로 conversion되고 그 value가 adjusted pointer parameter에 복사된다. `values[i]`는 caller의 actual element object를 지정하므로 assignment가 caller array에 보인다. array object 자체가 parameter로 reference 전달되는 것이 아니다.

## 4. 문법
```c
void add_one(int values[], size_t count)
{
    for (size_t i = 0; i < count; ++i) {
        ++values[i];
    }
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

void add_one(int values[], size_t count)
{
    for (size_t i = 0; i < count; ++i) {
        ++values[i];
    }
}

int main(void)
{
    int values[4] = {1, 2, 3, 4};
    size_t count = sizeof(values) / sizeof(values[0]);

    add_one(values, count);
    for (size_t i = 0; i < count; ++i) {
        printf("%d\n", values[i]);
    }
    return 0;
}
```

## 6. 코드 해석
function은 copied first-element pointer와 count 4를 받는다. indices 0~3의 caller elements를 각각 증가시켜 caller에서 2,3,4,5를 출력한다.

## 7. 내부 동작
[C17 표준] parameter adjustment와 array expression conversion 후 pointer value가 pass-by-value된다. subscript가 그 pointer가 가리키는 caller elements를 지정한다. count가 actual bounds와 맞고 array lifetime이 call 동안 유효해야 한다. function이 memory ownership을 받는 것은 아니다.

## 8. 자주 하는 실수
- array가 reference로 전달되었다고 설명한다.
- count를 전달하지 않고 parameter `sizeof`로 계산한다.
- count보다 한 번 더 loop한다.
- pointer parameter가 caller array를 소유한다고 생각한다.

## 9. 필수 실습
function에서 모든 caller array elements를 한 번씩 증가시킨다. [실습 README](../../exercises/16-pointers-and-functions/16-8/README.md)

## 10. 추가 실습
- ★ every element double
- ★★ negative elements만 수정
- ★★★ caller conversion부터 modification까지 diagram

## 11. 확인 문제
1. call expression에서 array에는 어떤 conversion이 일어나는가?
2. parameter가 받는 것은?
3. caller elements가 바뀌는 이유는?
4. C가 call by reference를 사용하는가?
5. count가 필요한 이유는?

## 12. 핵심 정리
array function은 copied pointer value로 caller elements를 지정해 수정하며 pass-by-value와 bounds contract는 유지된다.

## 13. 다음 Step
[Step 16-9. Part 16 종합 복습](16-9-part-16-review.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.3.2.1, 6.5.2.1, 6.5.2.2, 6.7.6.3
- [cppreference: Array declaration](https://en.cppreference.com/w/c/language/array.html#Arrays_of_unknown_size)
