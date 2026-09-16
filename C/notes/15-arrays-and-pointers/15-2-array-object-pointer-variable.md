# 15-2. 배열 객체와 포인터 변수의 차이

array object는 정해진 elements를 포함하고 pointer object는 pointer value 하나를 저장하므로 type·크기·assignment 가능성이 다르다.

## 1. 학습 목표
- `sizeof(array)`와 `sizeof(pointer)`를 구분한다.
- array object와 reassignable pointer object를 비교한다.
- `values`, `&values[0]`, `&values`의 type 의미를 구분한다.

## 2. 선수 지식
Step 15-1의 conversion과 Part 11의 `sizeof(array)`를 안다.

## 3. 핵심 개념
`sizeof(values)`에서는 array-to-pointer conversion이 일어나지 않아 전체 array size를 얻는다. `sizeof(pointer)`는 pointer type의 크기다. `values`는 일반 context에서 `int *`, `&values[0]`도 `int *`, `&values`는 pointer to array of 5 int인 `int (*)[5]`다. 주소 표현이 같아 보여도 type과 arithmetic 의미는 다르다.

## 4. 문법
```c
int values[5];
int *pointer = values;
/* sizeof(values): whole array */
/* sizeof(pointer): pointer object */
/* &values: pointer to array of 5 int */
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int values[5] = {10, 20, 30, 40, 50};
    int *pointer = values;

    printf("array: %zu\n", sizeof(values));
    printf("pointer: %zu\n", sizeof(pointer));
    printf("element: %zu\n", sizeof(*pointer));
    printf("%p %p %p\n",
           (void *)values,
           (void *)&values[0],
           (void *)&values);
    return 0;
}
```

## 6. 코드 해석
첫 크기는 5개의 int 전체, 둘째는 `int *` object, 셋째는 int 하나의 크기다. 세 `%p` 표현은 같은 시작 위치처럼 보일 수 있지만 source expression types는 다르다.

## 7. 내부 동작
[C17 abstract machine] `sizeof` operand가 non-VLA array이면 conversion 없이 array type size를 계산하며 operand expression은 평가되지 않는다. `&values` 역시 conversion 없이 whole array address를 만든다. [ABI] 실제 sizes와 printed representations는 구현 결과다.

## 8. 자주 하는 실수
- `sizeof(values)`가 pointer size라고 생각한다.
- 같은 숫자 주소면 type도 같다고 생각한다.
- array name을 reassignable pointer variable로 생각한다.
- `sizeof(*pointer)`가 pointer size라고 생각한다.

## 9. 필수 실습
array·pointer·pointed element `sizeof`와 세 address expressions를 비교한다. [실습 README](../../exercises/15-arrays-and-pointers/15-2/README.md)

## 10. 추가 실습
- ★ char array와 pointer 크기
- ★★ `&values` type을 말로 읽기
- ★★★ conversion context와 `sizeof` 표 작성

## 11. 확인 문제
1. `sizeof(values)`는 무엇의 크기인가?
2. `sizeof(pointer)`는 무엇의 크기인가?
3. `sizeof(*pointer)`는?
4. `&values`의 type은?
5. 출력 주소가 같으면 type도 같은가?
6. array object를 다른 address로 재대입할 수 있는가?

## 12. 핵심 정리
array와 pointer는 별도 type/category이며 `sizeof`, assignment, address type에서 차이가 드러난다.

## 13. 다음 Step
[Step 15-3. `arr[i]`와 `*(arr + i)`](15-3-subscript-indirection.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.3.2.1, 6.5.2.1, 6.5.3.2, 6.5.3.4
- [cppreference: sizeof](https://en.cppreference.com/w/c/language/sizeof.html)
