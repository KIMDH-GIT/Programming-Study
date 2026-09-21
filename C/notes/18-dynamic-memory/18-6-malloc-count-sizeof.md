# 18-6. `malloc(n * sizeof *ptr)`

여러 원소를 위한 allocation size는 원소 수와 한 원소 크기의 곱으로 계산한다.

## 1. 학습 목표
- `count * sizeof *values`를 해석한다.
- allocated storage를 array-like하게 접근한다.
- base pointer와 element count를 함께 관리한다.

## 2. 선수 지식
Step 18-4의 overflow guard와 Step 18-5의 `malloc`을 안다.

## 3. 핵심 개념
```c
int *values = malloc(count * sizeof *values);
```
`sizeof *values`는 pointed-to type을 따라가므로 type 변경 때 별도 type 이름 수정이 줄어든다. 표준이 요구하는 유일한 syntax는 아니지만 유지보수하기 좋은 관용구다.

## 4. 문법
```c
size_t count = 3;
if (count <= SIZE_MAX / sizeof *values) {
    values = malloc(count * sizeof *values);
}
```

## 5. 최소 코드 예제
```c
#include <stdint.h>
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    size_t count = 3;

    if (count > SIZE_MAX / sizeof(int)) {
        return 1;
    }
    int *values = malloc(count * sizeof *values);
    if (values == NULL) {
        return 1;
    }
    for (size_t i = 0; i < count; ++i) {
        values[i] = (int)(i + 1);
    }
    printf("%d\n", values[2]);
    free(values);
    return 0;
}
```

## 6. 코드 해석
세 `int`가 들어갈 byte 수를 요청한다. `i < count` 범위에서 저장하고 base pointer를 `free`한다.

## 7. 내부 동작
동적 배열이라는 별도 C type이 생기는 것이 아니다. allocated storage를 `int *`와 count로 관리하며 pointer arithmetic 규칙 안에서 접근한다.

## 8. 자주 하는 실수
- `sizeof(values)`가 block 전체 크기라고 생각한다.
- count를 잃어버린다.
- 순회로 base pointer 자체를 움직여 `free`할 값을 잃는다.

## 9. 필수 실습
다섯 `int`를 allocation하고 1~5를 저장·출력·해제한다.
[18-6 exercise](../../exercises/18-dynamic-memory/18-6/README.md)

## 10. 추가 실습
- ★ element type을 `double`로 바꾼다.
- ★★ pointer arithmetic 표기로 출력한다.
- ★★★ base pointer와 순회 pointer를 별도로 사용한다.

## 11. 확인 문제
1. allocation byte 수는 어떻게 계산하는가?
2. `sizeof *values` 관용구의 장점은?
3. `sizeof(values)`는 무엇의 크기인가?
4. count를 별도로 보관해야 하는 이유는?

## 12. 핵심 정리
- count와 element size를 곱해 요청한다.
- overflow를 먼저 검사한다.
- pointer와 count를 함께 관리하고 base pointer를 보존한다.

## 13. 다음 Step
[18-7. 할당 실패와 `NULL`](18-7-allocation-failure-and-null.md)

## 14. 참고 자료
- N1570 6.5.6 Additive operators; 7.22.3 Memory management functions. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: malloc](https://en.cppreference.com/w/c/memory/malloc)
- [SEI CERT MEM35-C](https://wiki.sei.cmu.edu/confluence/display/c/MEM35-C.+Allocate+sufficient+memory+for+an+object)
