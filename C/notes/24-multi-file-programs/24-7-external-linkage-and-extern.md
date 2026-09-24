# 24-7. external linkage와 `extern`
## 1. 학습 목표
- external linkage와 `extern` declaration을 설명한다.
- file-scope object declaration과 definition을 구분한다.
- tentative definition과 initialized `extern` definition을 정확히 다룬다.
## 2. 선수 지식
24-2 declaration·definition과 24-6 link를 안다.
## 3. 핵심 개념
linkage는 여러 declarations가 같은 entity를 지칭하는 관계다. scope는 identifier가 source의 어디에서 보이는지를 정하므로 linkage와 같은 개념이 아니다.

```c
extern int app_request_count;  /* declaration, definition 아님 */
int app_request_count = 0;     /* 하나의 definition */
```

`extern`은 특정 파일을 가져오거나 linker에게 filename을 지정하는 명령이 아니다.
## 4. 문법
```c
extern int app_request_count;
```

```c
int app_request_count = 0;
```

주의:
```c
extern int count = 0;  /* initializer가 있으므로 definition */
int other_count;       /* file scope에서는 tentative definition */
```
## 5. 최소 코드 예제
`counter.h`
```c
extern int app_request_count;

void counter_record_request(void);
```

`counter.c`
```c
#include "counter.h"

int app_request_count = 0;

void counter_record_request(void)
{
    ++app_request_count;
}
```

`main.c`
```c
#include <stdio.h>

#include "counter.h"

int main(void)
{
    counter_record_request();
    counter_record_request();
    printf("%d\n", app_request_count);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    main.c counter.c -o counter_app
./counter_app
```
## 6. 코드 해석
header의 `extern` declaration을 두 translation units가 공유하고 storage를 제공하는 definition은 `counter.c`에 하나만 있다. 이 예제는 문법 학습용이며 모든 state를 공개 global object로 만들라는 설계 권장이 아니다.
## 7. 내부 동작
**[preprocessor]** `extern` declaration text를 각 translation unit에 포함한다.

**[C translation unit]** compiler는 object의 declared type과 사용을 검사한다.

**[compiler]** `main.c`에는 외부 object reference가, `counter.c`에는 storage를 제공하는 definition이 반영된다.

**[linker]** 같은 external-linkage entity를 가리키는 references와 definition을 연결한다.

**[OS / loader]** file-scope object는 static storage duration을 가지며 실행 image에 맞게 배치된다. 정확한 section은 C17 보장이 아니다.

**[CPU / ISA]** link된 address를 통해 object를 읽고 쓴다.
## 8. 자주 하는 실수
- `extern`이 다른 `.c`를 import한다고 말한다.
- `extern`이 붙으면 절대로 definition이 아니라고 말한다.
- header에 `int count;`를 declaration-only라고 생각해 둔다.
- GCC `-fcommon` 또는 `-fno-common` 관찰을 ISO C17 정의 규칙으로 바꿔 말한다.
- file scope이므로 무조건 external linkage라고 일반화한다.
## 9. 필수 실습
하나의 external object를 `extern` declaration과 단일 definition으로 구성한다.
[24-7 exercise](../../exercises/24-multi-file-programs/24-7/README.md)
## 10. 추가 실습
- ★ 초기값을 바꾸고 두 translation units에서 같은 object를 보는지 확인한다.
- ★★ object 직접 접근 대신 값을 읽는 function API를 추가한다.
- ★★★ `extern int x = 1;`, `int x;`, `extern int x;`를 definition 관점에서 분류한다.
## 11. 확인 문제
1. scope와 linkage의 차이는?
2. `extern int count;`는 보통 어떤 역할인가?
3. `extern int count = 0;`는 definition인가?
4. file-scope `int count;`는 단순 extern declaration인가?
5. header에 `int count;`를 두면 왜 위험한가?
6. `extern`이 특정 source filename을 가리키는가?
## 12. 핵심 정리
- external linkage는 declarations가 translation units 사이에서 같은 entity를 지칭하게 한다.
- shared object는 header의 `extern` declaration과 하나의 `.c` definition으로 구성한다.
- scope, linkage, storage duration은 별도 속성이다.
## 13. 다음 Step
[24-8. internal linkage와 file-scope `static`](24-8-internal-linkage-and-file-scope-static.md)
## 14. 참고 자료
- N1570 6.2.1, 6.2.2, 6.2.4, 6.9.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Linkage](https://en.cppreference.com/w/c/language/storage_class_specifiers)
- [GCC: Code Gen Options (`-fcommon`)](https://gcc.gnu.org/onlinedocs/gcc/Code-Gen-Options.html)
