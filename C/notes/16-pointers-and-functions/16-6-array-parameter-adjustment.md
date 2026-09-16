# 16-6. 배열 매개변수와 pointer adjustment

function parameter declarator의 array notation은 pointer type으로 조정되며 local array declaration과 같은 의미가 아니다.

## 1. 학습 목표
- ordinary array object와 array parameter declaration을 구분한다.
- call expression conversion과 parameter adjustment 흐름을 설명한다.
- array parameter `sizeof` 함정을 피한다.

## 2. 선수 지식
Part 15의 array-to-pointer conversion과 `sizeof(array)`를 사용한다.

## 3. 핵심 개념
`int values[5];`는 actual array object다. `void show(int values[])`의 parameter는 C17 규칙으로 `int *values`에 맞게 adjusted된다. call에서 caller array expression은 first element pointer로 conversion되고 그 pointer value가 pass-by-value로 parameter를 초기화한다. array 전체가 복사되거나 call by reference가 되는 것이 아니다.

## 4. 문법
```c
void show(int values[]);
void same_type(int *values);
```

function parameter context에서 두 declarations의 parameter type은 조정 후 대응한다.

## 5. 최소 코드 예제
```c
#include <stdio.h>

void show_first(int values[])
{
    printf("%d\n", values[0]);
}

int main(void)
{
    int values[3] = {10, 20, 30};
    show_first(values);
    return 0;
}
```

## 6. 코드 해석
main의 values는 actual array of 3 int다. call expression에서 first element pointer가 argument가 되고 show_first의 adjusted pointer parameter가 그 value를 받는다. indexing은 caller first element를 읽어 10을 출력한다.

## 7. 내부 동작
[C17 표준] parameter declared as array of type is adjusted to qualified pointer to type. 따라서 function 안의 `sizeof(values)`는 caller array 전체가 아니라 adjusted pointer type의 size다. `void f(int a[10])`도 일반 parameter declaration만으로 정확히 10-element array copy를 뜻하지 않는다.

## 8. 자주 하는 실수
- array 전체가 parameter object로 복사된다고 생각한다.
- 배열 자체가 pointer로 변했다고 말한다.
- C가 array를 call by reference로 전달한다고 설명한다.
- function 안의 `sizeof(values)`로 caller count를 계산한다.

## 9. 필수 실습
actual array를 array-notation parameter에 전달하고 adjustment 흐름을 단계별로 적는다. [실습 README](../../exercises/16-pointers-and-functions/16-6/README.md)

## 10. 추가 실습
- ★ `int values[10]` parameter declaration 분석
- ★★ pointer notation으로 같은 function 작성
- ★★★ caller array와 parameter `sizeof` 의미 비교

## 11. 확인 문제
1. local `int values[3]`은 어떤 object인가?
2. parameter `int values[]`는 무엇으로 adjusted되는가?
3. call에서 argument는 무엇인가?
4. array 전체가 복사되는가?
5. parameter `sizeof(values)`가 caller array size인가?

## 12. 핵심 정리
array parameter notation은 pointer parameter로 adjustment되고 caller array expression은 first element pointer value로 conversion되어 pass-by-value된다.

## 13. 다음 Step
[Step 16-7. 배열 길이를 별도로 전달하는 이유](16-7-pass-array-length.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.3.2.1, 6.7.6.3
- [cppreference: Array declaration](https://en.cppreference.com/w/c/language/array.html#Arrays_of_unknown_size)
