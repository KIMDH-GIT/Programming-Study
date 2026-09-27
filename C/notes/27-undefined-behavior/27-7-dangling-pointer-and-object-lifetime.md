# 27-7. dangling pointer와 object lifetime
## 1. 학습 목표
- pointer value와 가리키던 object의 lifetime을 별도로 추적한다.
- 자동 객체 lifetime 종료와 `free` 이후 access를 분석한다.
- dangling pointer, use-after-free, double free를 안전하게 피한다.
## 2. 선수 지식
Part 10의 automatic lifetime과 Part 18의 allocation·ownership을 안다.
## 3. 핵심 개념
pointer가 저장되어 있어도 대상 object의 lifetime이 끝나면 그 pointer로 object에 access할 수 없다.

⚠ 분석용 — 실행하지 않는다.
```c
int *bad(void)
{
    int value = 10;
    return &value;
}
```
주소 값을 반환하는 문장과, 호출자가 lifetime이 끝난 object를 통해 access하는 문제를 구분한다. C17은 lifetime 종료 후 pointer value 사용에도 세부 제한을 두므로 이런 pointer를 반환·보관하지 않는 설계가 안전하다.

⚠ 분석용 — 실행하지 않는다.
```c
int *p = malloc(sizeof *p);
free(p);
printf("%d\n", *p); /* use-after-free: UB */
free(p);             /* double free: UB */
```
## 4. 문법
ownership을 한 곳에 모으고 마지막 access 뒤 정확히 한 번 해제한다.
```c
int *p = malloc(sizeof *p);
if (p != NULL) {
    *p = 42;
    free(p);
    p = NULL;
}
```
`p = NULL`은 별칭 pointer까지 고치지 않으며, ownership discipline을 대신하지 않는다.
## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <stdlib.h>

static int make_value(int **out)
{
    int *value = malloc(sizeof *value);

    if (value == NULL) {
        return 0;
    }
    *value = 42;
    *out = value;
    return 1;
}

int main(void)
{
    int *value = NULL;

    if (!make_value(&value)) {
        return 1;
    }
    printf("%d\n", *value);
    free(value);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o lifetime
./lifetime
```
## 6. 코드 해석
allocated object는 `malloc` 성공 뒤 시작해 `free`까지 살아 있다. 출력 `42` 뒤 정확히 한 번 해제하며 이후 pointer를 사용하지 않는다.
## 7. 내부 동작
**[C17]** automatic object lifetime은 해당 block 실행이 끝날 때 종료한다. allocated object의 lifetime은 deallocation으로 끝난다. lifetime 밖 access와 유효하지 않은 pointer를 `free`에 다시 전달하는 것은 UB다.

**[allocator observation]** memory가 즉시 재사용되거나 process가 종료되는지는 allocator 동작이며 보장되지 않는다.

**[OS observation]** page가 여전히 mapped되어 정상처럼 보여도 object lifetime은 끝났다.
## 8. 자주 하는 실수
- 주소 bit가 그대로면 object도 살아 있다고 생각한다.
- use-after-free나 double free는 allocator가 반드시 즉시 crash시킨다고 한다.
- pointer 하나를 `NULL`로 만들면 모든 alias가 안전해진다고 생각한다.
- automatic object 주소를 반환해도 잠시 값이 남으므로 괜찮다고 한다.
## 9. 필수 실습
생성 함수와 단일 owner를 작성하고 마지막 사용 뒤 한 번만 `free`한다.
[27-7 exercise](../../exercises/27-undefined-behavior/27-7/README.md)
## 10. 추가 실습
- ★ cleanup 지점을 한 곳으로 모은다.
- ★★ owner와 non-owning alias를 표로 표시한다.
- ★★★ 여러 allocation 실패 경로에서 정확히 정리한다.
## 11. 확인 문제
1. pointer value 존재와 object lifetime은 왜 다른가?
2. automatic object lifetime은 언제 끝나는가?
3. `free` 후 dereference의 분류는?
4. double free가 반드시 crash하는가?
5. `p = NULL`이 alias 문제를 모두 해결하는가?
## 12. 핵심 정리
- access 가능성은 pointer bit가 아니라 대상 object lifetime과 contract로 판단한다.
- allocated object는 마지막 access 뒤 정확히 한 번 해제한다.
- dangling 사례는 분석만 하고 정상 실행 대상으로 삼지 않는다.
## 13. 다음 Step
[27-8. uninitialized value](27-8-uninitialized-value.md)
## 14. 참고 자료
- N1570 6.2.4, 6.2.6.1p5, 7.22.3.3, 7.22.3.4, Annex J.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Object lifetime](https://en.cppreference.com/w/c/language/lifetime)
