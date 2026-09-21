# 18-7. 할당 실패와 `NULL`

allocation 함수는 요청을 만족할 수 없으면 null pointer를 반환하므로 dereference 전에 검사해야 한다.

## 1. 학습 목표
- allocation failure를 정상 제어 흐름으로 처리한다.
- null pointer와 valid allocated pointer를 구분한다.
- `errno`를 필수 portable contract로 가정하지 않는다.

## 2. 선수 지식
Part 14의 `NULL`과 Step 18-5의 `malloc` contract를 안다.

## 3. 핵심 개념
allocation 실패는 undefined behavior가 아니라 library 함수가 표현하는 실패 결과다. 문제는 검사 없이 null pointer를 dereference할 때 발생한다.

## 4. 문법
```c
int *p = malloc(sizeof *p);
if (p == NULL) {
    fprintf(stderr, "allocation failed\n");
    return 1;
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int *p = malloc(sizeof *p);

    if (p == NULL) {
        fprintf(stderr, "allocation failed\n");
        return 1;
    }
    *p = 8;
    printf("%d\n", *p);
    free(p);
    return 0;
}
```

## 6. 코드 해석
failure branch는 dereference 전에 종료한다. success branch만 값을 저장·읽고 해제한다.

## 7. 내부 동작
C library contract는 failure를 null pointer로 알린다. allocator나 OS가 왜 요청을 만족하지 못했는지는 별도 구현 문제다.

## 8. 자주 하는 실수
- 작은 allocation은 절대 실패하지 않는다고 생각한다.
- dereference한 뒤 NULL을 검사한다.
- failure가 항상 segmentation fault라고 생각한다.

## 9. 필수 실습
한 배열 allocation에 failure branch와 success cleanup을 작성한다.
[18-7 exercise](../../exercises/18-dynamic-memory/18-7/README.md)

## 10. 추가 실습
- ★ 오류 메시지를 stderr로 출력한다.
- ★★ 함수가 실패 시 NULL을 반환하도록 contract를 쓴다.
- ★★★ 두 allocation 중 두 번째 실패 시 첫 번째를 정리하는 순서를 분석한다.

## 11. 확인 문제
1. `malloc`은 실패를 어떻게 알리는가?
2. allocation failure 자체가 UB인가?
3. failure 검사는 언제 해야 하는가?
4. `errno`가 필수라는 설명이 왜 부정확한가?

## 12. 핵심 정리
- allocation 결과는 항상 검사한다.
- null dereference 전에 실패 경로로 나간다.
- 실패 처리와 cleanup 책임을 함께 설계한다.

## 13. 다음 Step
[18-8. `free`와 소유권](18-8-free-and-ownership.md)

## 14. 참고 자료
- N1570 7.22.3 Memory management functions. N1570은 **C11 공개 Committee Draft**이며 관련 contract는 C17에서도 유지된다.
- [cppreference: malloc](https://en.cppreference.com/w/c/memory/malloc)
- [cppreference: null pointer](https://en.cppreference.com/w/c/language/null_pointer)
