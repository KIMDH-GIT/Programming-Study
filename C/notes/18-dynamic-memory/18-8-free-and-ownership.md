# 18-8. `free`와 소유권

`free`는 allocation으로 얻은 storage를 deallocate하며, 프로그램은 누가 언제 호출할지 명확히 정해야 한다.

## 1. 학습 목표
- `free`의 valid argument와 lifetime 효과를 설명한다.
- ownership을 `free` 책임 규율로 사용한다.
- base pointer와 aliases를 구분한다.

## 2. 선수 지식
Step 18-2의 lifetime과 Step 18-7의 failure handling을 안다.

## 3. 핵심 개념
```c
void free(void *ptr);
```
`ptr`은 allocation 함수가 반환한 값 또는 null pointer여야 한다. `free(NULL)`은 아무 동작도 하지 않는다. 중간 element 주소나 automatic object 주소를 전달하면 undefined behavior다.

## 4. 문법
```c
free(values);
values = NULL;
```
null 대입은 `values` 하나만 바꾼다. aliases는 자동으로 바뀌지 않는다.

## 5. 최소 코드 예제
```c
#include <stdlib.h>

int main(void)
{
    int *values = malloc(3 * sizeof *values);

    if (values == NULL) {
        return 1;
    }
    values[0] = 10;
    free(values);
    values = NULL;
    free(values);
    return 0;
}
```

## 6. 코드 해석
첫 `free`가 allocated lifetime을 끝낸다. 이후 새 null pointer value를 저장하며 `free(NULL)`은 no-op이다.

## 7. 내부 동작
```
values pointer object ----> allocated storage
free(values)               lifetime ends
values = NULL              pointer object gets a new value
```
allocator의 내부 free list 동작은 C17 보장이 아니다.

## 8. 자주 하는 실수
- `free`가 pointer object 자체를 삭제한다고 생각한다.
- `free(values + 1)`도 같은 allocation 안이므로 가능하다고 생각한다.
- null 대입이 모든 aliases를 고친다고 생각한다.

## 9. 필수 실습
base pointer를 한 번 해제하고 NULL로 바꾼 뒤 `free(NULL)` contract를 확인한다.
[18-8 exercise](../../exercises/18-dynamic-memory/18-8/README.md)

## 10. 추가 실습
- ★ ownership timeline을 그린다.
- ★★ borrowed pointer는 `free`하지 않는 contract를 쓴다.
- ★★★ base pointer와 순회 pointer 역할을 분리한다.

## 11. 확인 문제
1. `free`에 전달할 수 있는 pointer는?
2. `free(NULL)`의 효과는?
3. `free` 뒤 allocated object lifetime은?
4. ownership은 C의 자동 type-system 기능인가?
5. alias는 왜 따로 관리해야 하는가?

## 12. 핵심 정리
- allocation의 base pointer를 적절히 `free`한다.
- ownership은 코드가 정하는 책임 규율이다.
- null 대입은 aliases를 해결하지 않는다.

## 13. 다음 Step
[18-9. `calloc`의 all-bits-zero 의미](18-9-calloc-all-bits-zero.md)

## 14. 참고 자료
- N1570 7.22.3.3 `free`. N1570은 **C11 공개 Committee Draft**이며 관련 contract는 C17에서도 유지된다.
- [cppreference: free](https://en.cppreference.com/w/c/memory/free)
- [SEI CERT MEM31-C](https://wiki.sei.cmu.edu/confluence/display/c/MEM31-C.+Free+dynamically+allocated+memory+when+no+longer+needed)
