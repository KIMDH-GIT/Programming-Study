# 31-14. callback context의 ownership·lifetime
## 1. 학습 목표
- callback context의 owner와 borrow 기간을 명시한다.
- context lifetime이 callback 호출을 포함해야 함을 설명한다.
- 저장형 callback과 즉시 호출 callback 계약을 구분한다.
## 2. 선수 지식
31-13의 function pointer와 Part 30의 ownership/lifetime을 안다.
## 3. 핵심 개념
`void *context`는 object의 ownership을 자동 전달하지 않는다. interface는 누가 context를 소유하고 callback이 언제까지 borrow하며 callback이 pointer를 저장해도 되는지 정해야 한다.

즉시 호출 API는 호출이 끝날 때까지만 context가 살아 있으면 된다. 등록 후 나중에 호출하는 API는 unregister 또는 driver 종료까지 context lifetime이 유지되어야 한다. automatic object address를 장기 저장하면 scope 종료 후 expired pointer가 된다.
## 4. 문법
```c
typedef void (*event_fn)(void *context, unsigned value);
static void dispatch(event_fn callback, void *context, unsigned value);
```
이 예제의 `dispatch`는 callback/context를 저장하지 않고 즉시 호출한다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

typedef void (*event_fn)(void *context, unsigned value);

typedef struct {
    unsigned sum;
} SumContext;

static void add_event(void *context, unsigned value)
{
    SumContext *sum = context;
    sum->sum += value;
}

static void dispatch(event_fn callback, void *context, unsigned value)
{
    if (callback != NULL) {
        callback(context, value);
    }
}

int main(void)
{
    SumContext context = {0u};
    dispatch(add_event, &context, 3u);
    dispatch(add_event, &context, 4u);
    printf("sum=%u\n", context.sum);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o callback_context
./callback_context
```
## 6. 코드 해석
출력은 `sum=7`이다. `context` lifetime은 두 callback 호출을 포함하고 `dispatch`는 pointer를 저장하지 않는다.
## 7. 내부 동작
**[C17 abstract machine]** automatic storage duration object의 lifetime은 block 실행과 연결된다. lifetime이 끝난 object를 가리키던 pointer를 callback에서 사용하면 정의되지 않는다.

**[interface contract]** callback의 reentrancy, concurrency, ISR 호출 여부는 이 Step에서 가정하지 않는다. 해당 model이 있다면 별도 ABI/synchronization 계약이 필요하다.

같은 address가 나중에 재사용되어도 같은 C object lifetime이 계속된다는 뜻은 아니다.
## 8. 자주 하는 실수
- `void *`가 ownership 이전을 뜻한다고 생각한다.
- automatic context를 장기 등록하고 scope 종료 후 호출한다.
- callback이 context를 저장하는지 명시하지 않는다.
- address가 같으면 같은 object가 살아 있다고 말한다.
## 9. 필수 실습
즉시 dispatch에서 context가 모든 callback 호출 동안 살아 있음을 검증한다. [31-14 exercise](../../exercises/31-system-embedded-c/31-14/README.md)
## 10. 추가 실습
- ★ callback NULL을 no-op으로 처리한다.
- ★★ owner/borrow/저장 여부를 표로 작성한다.
- ★★★ 장기 등록 API의 unregister contract를 설계만 한다.
## 11. 확인 문제
1. `void *context`는 ownership을 전달하는가?
2. 즉시 호출과 저장형 callback의 lifetime 차이는?
3. automatic context 장기 저장이 위험한 이유는?
4. address 재사용과 object lifetime이 다른 이유는?
5. ISR callback에는 어떤 추가 계약이 필요한가?
## 12. 핵심 정리
- callback context의 owner, borrow 기간, 저장 여부를 명시한다.
- context lifetime은 모든 사용을 포함해야 한다.
- address 동일성과 object 동일성을 혼동하지 않는다.
## 13. 다음 Step
[31-15. Part 31 종합 복습](31-15-part-31-review.md)
## 14. 참고 자료
- WG14 N1570 6.2.4, 6.5.2.2. N1570은 C11 공개 Committee Draft이며 object lifetime/call 규칙은 C17에서도 유지된다: https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf
