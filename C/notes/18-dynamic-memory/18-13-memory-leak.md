# 18-13. memory leak

memory leak은 live allocation을 더 이상 해제할 수 없게 pointer를 잃는 resource management 오류다.

## 1. 학습 목표
- lost pointer가 leak을 만드는 과정을 설명한다.
- leak과 undefined behavior를 구분한다.
- ownership cleanup을 정상 경로와 실패 경로에 적용한다.

## 2. 선수 지식
Step 18-8의 ownership과 Step 18-11의 temporary pointer를 안다.

## 3. 핵심 개념
기존 allocation을 가리키는 유일한 pointer에 새 allocation 결과를 덮어쓰면 첫 pointer를 잃을 수 있다. leak 자체가 곧바로 UB인 것은 아니지만 장기 실행 프로그램과 library에서 resource exhaustion을 만든다.

## 4. 문법
```c
int *first = malloc(sizeof *first);
/* 사용 */
free(first);
```
각 성공한 allocation에는 명확한 cleanup 경로가 필요하다.

## 5. 최소 코드 예제
```c
#include <stdlib.h>

int main(void)
{
    int *first = malloc(sizeof *first);
    int *second = malloc(sizeof *second);

    if (first == NULL || second == NULL) {
        free(first);
        free(second);
        return 1;
    }
    *first = 1;
    *second = 2;
    free(second);
    free(first);
    return 0;
}
```

## 6. 코드 해석
부분 성공에서도 `free(NULL)` contract를 활용해 두 pointer를 정리한다. 정상 경로에서는 역순으로 각각 한 번 해제한다.

## 7. 내부 동작
C17은 OS가 process 종료 시 자원을 회수한다고 보장하지 않는다. sanitizer나 Valgrind의 leak report는 구현 도구의 관찰이다.

## 8. 자주 하는 실수
- leak을 항상 즉시 UB라고 부른다.
- 프로그램 종료가 가까우면 `free`는 불필요하다고 일반화한다.
- 새 allocation 전에 old pointer를 보존하지 않는다.

## 9. 필수 실습
두 allocation의 부분 실패와 성공 cleanup 경로를 작성한다.
[18-13 exercise](../../exercises/18-dynamic-memory/18-13/README.md)

## 10. 추가 실습
- ★ allocation/free 표를 작성한다.
- ★★ 함수의 단일 cleanup 책임을 설명한다.
- ★★★ lost pointer 사례를 실행 없이 분석한다.

## 11. 확인 문제
1. memory leak은 어떤 상태인가?
2. leak 자체가 항상 UB인가?
3. partial allocation failure에서 무엇을 정리해야 하는가?
4. OS 회수에 의존하면 안 되는 이유는?

## 12. 핵심 정리
- 성공한 allocation마다 reachable cleanup 경로를 둔다.
- leak은 resource management 오류이며 UB와 구분한다.
- 부분 실패도 이미 얻은 resources를 정리한다.

## 13. 다음 Step
[18-14. dangling pointer와 use-after-free](18-14-dangling-pointer-use-after-free.md)

## 14. 참고 자료
- N1570 7.22.3 Memory management functions. N1570은 **C11 공개 Committee Draft**이며 관련 contract는 C17에서도 유지된다.
- [cppreference: dynamic memory management](https://en.cppreference.com/w/c/memory)
- [SEI CERT MEM31-C](https://wiki.sei.cmu.edu/confluence/display/c/MEM31-C.+Free+dynamically+allocated+memory+when+no+longer+needed)
