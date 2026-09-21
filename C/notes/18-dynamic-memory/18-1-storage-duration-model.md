# 18-1. stack·heap 모델과 C의 storage duration

입문서의 stack·heap 그림은 흔한 구현을 이해하는 모델이지 C17이 요구하는 메모리 영역 이름이 아니다.

## 1. 학습 목표
- C17 storage duration과 일반적인 stack·heap 구현을 구분한다.
- automatic object와 allocated storage의 lifetime 시작·종료를 비교한다.
- C library, allocator, OS, CPU의 역할을 분리한다.

## 2. 선수 지식
Part 10의 block scope와 Part 14의 object·pointer·lifetime을 안다.

## 3. 핵심 개념
C17은 automatic, static, thread, allocated storage duration을 규정한다. 일반적인 구현은 automatic object를 "stack", allocation 함수가 관리하는 저장 공간을 "heap"이라 부르지만, C17은 특정 stack 구조나 heap 영역을 요구하지 않는다.

## 4. 문법
```c
int automatic_value = 10;
int *allocated_value = malloc(sizeof *allocated_value);
```
첫 객체의 lifetime은 block 실행과 연결되고, 두 번째 저장 공간은 allocation 성공부터 `free` 또는 성공한 `realloc`까지 유지된다.

## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int automatic_value = 10;
    int *allocated_value = malloc(sizeof *allocated_value);

    if (allocated_value == NULL) {
        return 1;
    }
    *allocated_value = 20;
    printf("%d %d\n", automatic_value, *allocated_value);
    free(allocated_value);
    return 0;
}
```

## 6. 코드 해석
`allocated_value`는 automatic pointer object다. 그 안의 pointer value가 별도로 할당된 저장 공간을 가리킨다. 저장 후 읽고, 책임이 끝날 때 `free`한다.

## 7. 내부 동작
- **[C17 abstract machine]** storage duration과 object lifetime을 규정한다.
- **[C standard library]** `malloc`·`free`의 contract를 규정한다.
- **[allocator implementation]** free list나 size class 같은 내부 전략을 선택할 수 있다.
- **[OS]** 구현이 필요하면 virtual memory를 제공할 수 있다.
- **[CPU]** 생성된 명령으로 실제 load/store를 수행한다.

## 8. 자주 하는 실수
- `malloc`이 C 표준의 고정된 heap 영역에서 가져온다고 단정한다.
- pointer object와 allocated storage를 같은 객체라고 생각한다.
- 프로세스 종료 시 OS가 회수할 것이라며 `free` 습관을 생략한다.

## 9. 필수 실습
automatic `int`와 allocation한 `int`를 각각 저장·출력·해제한다.
[18-1 exercise](../../exercises/18-dynamic-memory/18-1/README.md)

## 10. 추가 실습
- ★ 두 객체의 역할을 그림으로 표시한다.
- ★★ pointer object와 target의 lifetime을 표로 쓴다.
- ★★★ C17 보장과 Linux 구현 예시를 두 열로 분리한다.

## 11. 확인 문제
1. C17이 "heap"이라는 특정 영역을 요구하는가?
2. pointer object와 allocated storage는 어떻게 다른가?
3. allocator와 OS의 역할을 C library contract와 왜 구분해야 하는가?
4. allocated storage duration은 언제 끝나는가?

## 12. 핵심 정리
- stack·heap은 유용한 구현 모델이지만 C17 용어와 동일하지 않다.
- pointer object와 allocated storage는 별개다.
- 표준 contract와 구현 내부를 구분한다.

## 13. 다음 Step
[18-2. 자동 객체와 동적 객체의 lifetime](18-2-automatic-and-allocated-lifetime.md)

## 14. 참고 자료
- ISO/IEC 9899:2011 Committee Draft N1570, 6.2.4 Storage durations of objects. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: storage duration](https://en.cppreference.com/w/c/language/storage_duration)
- [cppreference: dynamic memory management](https://en.cppreference.com/w/c/memory)
