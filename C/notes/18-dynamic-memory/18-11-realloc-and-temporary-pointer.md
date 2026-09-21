# 18-11. `realloc`과 임시 포인터

`realloc`은 기존 allocation의 크기를 바꾸려 시도하며, 실패 시 원본을 보존하려면 반환값을 임시 pointer에 받아야 한다.

## 1. 학습 목표
- `realloc` 성공·실패 contract를 설명한다.
- 직접 대입의 lost pointer 위험을 피한다.
- 성공 후 새 pointer와 새 bounds를 사용한다.

## 2. 선수 지식
Step 18-7의 failure handling과 Step 18-10의 zero-size 주의를 안다.

## 3. 핵심 개념
```c
void *realloc(void *ptr, size_t new_size);
```
`new_size > 0`인 요청에서 성공하면 새 allocation pointer를 반환하고 old allocation은 deallocate된다. 실패하면 NULL을 반환하며 old allocation은 유지된다. 주소는 같을 수도 달라질 수도 있다. zero-size 요청은 Step 18-10의 별도 implementation-defined 규칙 때문에 이 일반 실패 pattern에 넣지 않는다.

## 4. 문법
```c
if (new_count != 0 && new_count <= SIZE_MAX / sizeof *p) {
    size_t new_size = new_count * sizeof *p;
    int *temp = realloc(p, new_size);

    if (temp == NULL) {
        /* new_size > 0: p remains valid */
    } else {
        p = temp;
    }
} else {
    /* reject this request */
}
```

invalid count는 `realloc` 호출이 있는 branch에 진입하지 않는다.

## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int *values = malloc(2 * sizeof *values);

    if (values == NULL) {
        return 1;
    }
    values[0] = 10;
    values[1] = 20;

    int *temp = realloc(values, 3 * sizeof *values);
    if (temp == NULL) {
        free(values);
        return 1;
    }
    values = temp;
    values[2] = 30;
    printf("%d\n", values[2]);
    free(values);
    return 0;
}
```

## 6. 코드 해석
이 예제의 요청 크기는 양수다. 임시 pointer가 실패를 먼저 받으며, success branch에서만 `values`를 새 pointer로 바꾸고 확장 element를 초기화한다.

## 7. 내부 동작
```
before: values -> old block, requested size > 0
failure: temp == NULL, values -> old block
success: temp -> new allocation, old allocation lifetime ended
         values = temp
```
보존되는 내용은 old size와 new size 중 작은 범위까지다. 확장된 영역의 내용은 indeterminate이다.

## 8. 자주 하는 실수
- `values = realloc(values, ...)`로 원본을 잃는다.
- realloc은 항상 같은 주소 뒤쪽만 늘린다고 생각한다.
- 확장 영역이 자동으로 zero라고 생각한다.

## 9. 필수 실습
2개 배열을 3개로 확장하고 temporary pointer pattern으로 새 원소를 저장한다.
[18-11 exercise](../../exercises/18-dynamic-memory/18-11/README.md)

## 10. 추가 실습
- ★ 실패·성공 흐름도를 그린다.
- ★★ 크기를 줄인 뒤 새 bounds만 사용한다.
- ★★★ overflow guard를 new_count 계산에 추가한다.

## 11. 확인 문제
1. 양수 크기 realloc 실패 시 old allocation은 어떻게 되는가?
2. 직접 대입이 위험한 이유는?
3. 주소가 항상 유지되는가?
4. 확장된 새 영역은 zero인가?
5. 내용은 어느 범위까지 보존되는가?

## 12. 핵심 정리
- 양수이며 overflow가 검사된 크기에 temporary pointer pattern을 쓴다.
- 성공하면 반환 pointer만 사용한다.
- 확장 영역은 명시적으로 초기화한다.

## 13. 다음 Step
[18-12. `realloc` 후 기존 포인터·내부 별칭](18-12-realloc-old-pointer-and-aliases.md)

## 14. 참고 자료
- N1570 7.22.3.5 `realloc`. N1570은 **C11 공개 Committee Draft**다. 양수 크기 failure contract는 C17에서도 유지되며 zero-size 예외는 WG14 DR 400 반영 문구를 따른다.
- [WG14 N2243: DR 400 `realloc` with size zero](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2243.htm)
- [cppreference: realloc](https://en.cppreference.com/w/c/memory/realloc)
- [SEI CERT MEM04-C](https://wiki.sei.cmu.edu/confluence/display/c/MEM04-C.+Beware+of+zero-length+allocations)
