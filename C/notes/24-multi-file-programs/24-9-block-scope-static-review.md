# 24-9. block-scope `static` 복습
## 1. 학습 목표
- block-scope static object의 scope와 storage duration을 설명한다.
- file-scope `static`의 internal linkage와 구분한다.
- 상태가 어느 translation unit에 속하는지 분석한다.
## 2. 선수 지식
24-8 file-scope `static`과 Part 10의 local scope를 안다.
## 3. 핵심 개념
block 안에서 선언한 static object는 block scope를 가지며 static storage duration을 가진다. block-scope `static` object identifier는 no linkage다. block scope의 모든 identifier를 일반화한 규칙은 아니며 `extern` declaration은 별도 linkage 규칙을 따른다.

```c
int ticket_next(void)
{
    static int next = 1;
    return next++;
}
```

반면 file-scope `static` function 또는 object identifier에서는 internal linkage가 핵심 역할 중 하나다. 같은 keyword라도 context가 다르다.
## 4. 문법
```c
void f(void)
{
    static int call_count;  /* block scope, no linkage,
                               static storage duration */
}

static int file_count;      /* file scope, internal linkage,
                               static storage duration */
```
## 5. 최소 코드 예제
`ticket.h`
```c
int ticket_next(void);
```

`ticket.c`
```c
#include "ticket.h"

int ticket_next(void)
{
    static int next = 1;

    return next++;
}
```

`main.c`
```c
#include <stdio.h>

#include "ticket.h"

int main(void)
{
    const int first = ticket_next();
    const int second = ticket_next();
    const int third = ticket_next();

    printf("%d %d %d\n", first, second, third);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    main.c ticket.c -o ticket_app
./ticket_app
```
## 6. 코드 해석
`next`는 `ticket_next` block 밖에서 이름으로 접근할 수 없지만 호출 사이에 stored value가 유지된다. function API만 공개되므로 caller가 object를 직접 변경하지 않는다. 각 호출을 별도 statement에서 수행하므로 한 function call의 argument evaluation order에 기대지 않고 `1 2 3`을 관찰한다.
## 7. 내부 동작
**[preprocessor]** public function declaration을 두 translation units에 포함한다.

**[C translation unit]** `next` identifier는 `ticket.c` 기반 translation unit의 function block에서만 보인다.

**[compiler]** automatic object와 달리 호출 종료 뒤에도 storage가 지속되도록 translation한다.

**[linker]** block-scope `next`는 no linkage이므로 다른 declaration과 external symbol로 연결하지 않는다.

**[OS / loader]** 구현은 static-duration object를 실행 image에 배치할 수 있지만 구체적 section은 C17이 규정하지 않는다.

**[CPU / ISA]** 호출마다 같은 storage의 값을 읽고 갱신한다.
## 8. 자주 하는 실수
- file-scope static과 block-scope static을 같은 효과라고 말한다.
- block-scope static object가 external linkage를 가진다고 생각한다.
- static storage duration과 internal linkage를 같은 용어로 쓴다.
- 숨은 state가 있는 function을 무조건 순수 함수처럼 취급한다.
- 여러 stateful calls를 한 expression에 넣고 argument evaluation order에 의존한다.
## 9. 필수 실습
ticket number를 한 번에 하나씩 반환하는 function을 별도 source에 작성한다.
[24-9 exercise](../../exercises/24-multi-file-programs/24-9/README.md)
## 10. 추가 실습
- ★ 초기 ticket number를 100으로 바꾼다.
- ★★ 세 반환값을 별도 variables에 저장해 결정적으로 출력한다.
- ★★★ file-scope static object를 사용한 구현과 차이를 비교한다.
## 11. 확인 문제
1. block-scope static object의 scope는?
2. storage duration은?
3. identifier의 linkage는?
4. file-scope static object와 공통점·차이점은?
5. hidden state를 function API로 감쌀 때의 장단점은?
## 12. 핵심 정리
- block-scope static object는 좁은 scope와 static storage duration을 가진다.
- no linkage, internal linkage, external linkage를 구분한다.
- `static` 의미는 선언 context에 따라 읽는다.
## 13. 다음 Step
[24-10. 중복 정의와 undefined reference](24-10-duplicate-definitions-and-undefined-reference.md)
## 14. 참고 자료
- N1570 5.1.2.3, 6.2.1, 6.2.2, 6.2.4, 6.7.1. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Storage duration](https://en.cppreference.com/w/c/language/storage_duration)
- [cppreference: Order of evaluation](https://en.cppreference.com/w/c/language/eval_order)
