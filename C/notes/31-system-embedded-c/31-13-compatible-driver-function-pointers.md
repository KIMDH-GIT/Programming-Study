# 31-13. driver function pointer 호환형
## 1. 학습 목표
- callback typedef와 function definition의 compatible type을 맞춘다.
- incompatible function pointer cast 호출을 피한다.
- driver context와 output contract를 함께 설계한다.
## 2. 선수 지식
Part 22의 function pointer, Part 20의 typedef, 31-12의 interface contract를 안다.
## 3. 핵심 개념
function pointer로 호출할 함수는 return type과 parameter type이 compatible해야 한다. 단순히 address 크기가 같거나 cast가 허용된다고 해서 incompatible type으로 호출해도 되는 것은 아니다.

driver interface는 `void *context`와 exact callback typedef를 묶을 수 있다. context object의 actual type과 lifetime은 caller/driver contract가 정한다.
## 4. 문법
```c
typedef int (*driver_read_fn)(void *context, unsigned *out);
```
`out`은 non-NULL writable object이고 성공 시에만 갱신한다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

typedef int (*driver_read_fn)(void *context, unsigned *out);

typedef struct {
    unsigned value;
} MockContext;

static int mock_read(void *context, unsigned *out)
{
    if (context == NULL || out == NULL) {
        return 0;
    }
    MockContext *mock = context;
    *out = mock->value;
    return 1;
}

int main(void)
{
    MockContext context = {42u};
    driver_read_fn read = mock_read;
    unsigned value = 0u;
    int ok = read(&context, &value);
    printf("ok=%d value=%u\n", ok, value);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o driver_function
./driver_function
```
## 6. 코드 해석
출력은 `ok=1 value=42`다. `mock_read`의 type은 typedef와 정확히 맞고 context는 호출 동안 살아 있다.
## 7. 내부 동작
**[C17 abstract machine]** compatible function type을 통한 indirect call은 정의된 동작이다. incompatible function type으로 cast한 뒤 호출하면 undefined behavior가 될 수 있다.

**[ABI-specific]** calling convention이 우연히 같아 보이는 것은 C compatible-type 요구를 대체하지 않는다.

`void *`는 object pointer context를 일반화하지만 type, ownership, lifetime 검증을 자동으로 수행하지 않는다.
## 8. 자주 하는 실수
- parameter 수만 같으면 function pointer type도 compatible하다고 생각한다.
- incompatible cast가 warning을 없애면 호출도 안전하다고 말한다.
- object pointer와 function pointer의 portability를 동일시한다.
- callback이 context를 언제까지 쓰는지 정하지 않는다.
## 9. 필수 실습
typedef와 정확히 compatible한 mock function을 indirect call하고 NULL failure도 확인한다. [31-13 exercise](../../exercises/31-system-embedded-c/31-13/README.md)
## 10. 추가 실습
- ★ write callback typedef를 추가한다.
- ★★ const context가 필요한 read callback을 설계한다.
- ★★★ incompatible prototype 사례를 실행하지 않고 diagnostic으로 분석한다.
## 11. 확인 문제
1. compatible function type에 필요한 요소는?
2. cast가 type compatibility를 만들어 주지 않는 이유는?
3. ABI 우연과 C 정의된 동작이 다른 이유는?
4. `void *context`가 보장하지 않는 것은?
5. output contract를 따로 정의해야 하는 이유는?
## 12. 핵심 정리
- function pointer call은 compatible type을 요구한다.
- cast로 interface mismatch를 숨기지 않는다.
- context type과 lifetime을 별도 계약으로 기록한다.
## 13. 다음 Step
[31-14. callback context의 ownership·lifetime](31-14-callback-context-ownership-and-lifetime.md)
## 14. 참고 자료
- WG14 N1570 6.2.7, 6.3.2.3, 6.5.2.2, 6.7.6.3. N1570은 C11 공개 Committee Draft이며 compatible function type 규칙은 C17에서도 유지된다: https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf
