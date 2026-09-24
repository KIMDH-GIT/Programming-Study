# 26-7. global·static·local·heap 객체 분석
## 1. 학습 목표
- 편의 용어를 C17 속성으로 분해한다.
- file-scope static과 block-scope static을 구분한다.
- initialization과 duration을 섞지 않는다.
## 2. 선수 지식
26-1부터 26-6까지를 안다.
## 3. 핵심 개념
“global”, “local”, “heap object”는 편의 용어다. formal analysis에서는 scope, linkage, storage duration, lifetime을 각각 적는다.

| object | scope | linkage | duration |
|---|---|---|---|
| `external_count` | file | external | static |
| `internal_count` | file | internal | static |
| `local` | block | none | automatic |
| `saved` | block | none | static |
| allocated storage | identifier와 별개 | 해당 없음 | allocated |
## 4. 문법
```c
int external_count;
static int internal_count;

void f(void)
{
    int local;
    static int saved;
}
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
#include <stdlib.h>

int external_count = 1;
static int internal_count = 2;

static int next_saved(void)
{
    int local = 3;
    static int saved;

    ++saved;
    return local + saved;
}

int main(void)
{
    int *allocated = malloc(sizeof *allocated);

    if (allocated == NULL) {
        return 1;
    }
    *allocated = 4;
    printf("%d %d %d %d\n",
           external_count, internal_count,
           next_saved(), *allocated);
    free(allocated);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o analysis_app
./analysis_app
```
## 6. 코드 해석
file-scope objects는 모두 static duration이지만 linkage가 다르다. `local`과 pointer object는 automatic duration, `saved`는 block scope·no linkage·static duration이다.
## 7. 내부 동작
**[C17]** 각 object의 four properties를 정한다.

**[compiler / linker]** external/internal names와 actual representations를 처리한다.

**[OS / executable format]** section·mapping 선택은 implementation이다.

**[CPU / ISA]** resulting accesses를 수행한다.
## 8. 자주 하는 실수
- file static과 block static을 같은 의미라고 말한다.
- block static에 external linkage가 있다고 말한다.
- initialized/uninitialized 여부로 duration을 정한다.
- global이라는 말만으로 linkage를 확정한다.
- allocated storage와 pointer object를 하나로 본다.
## 9. 필수 실습
예제 objects를 네 속성으로 분류하고 실행한다.
[26-7 exercise](../../exercises/26-memory-structure/26-7/README.md)
## 10. 추가 실습
- ★ `next_saved`를 두 번 호출한다.
- ★★ tentative definition을 표에 추가한다.
- ★★★ optimizer가 storage를 제거할 수 있는 사례를 설명한다.
## 11. 확인 문제
1. file static과 block static의 linkage 차이는?
2. 둘의 공통 duration은?
3. local이라는 말만으로 duration을 알 수 있는가?
4. allocated storage의 identifier scope는 어떻게 분석하는가?
5. initializer가 duration을 정하는가?
## 12. 핵심 정리
- informal names를 C17 properties로 분해한다.
- keyword 하나에 scope·linkage·duration을 모두 맡기지 않는다.
- actual placement는 별도 implementation 관찰이다.
## 13. 다음 Step
[26-8. `%p`로 주소 관찰](26-8-observing-addresses-with-percent-p.md)
## 14. 참고 자료
- N1570 6.2.1, 6.2.2, 6.2.4, 7.22.3. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Storage duration](https://en.cppreference.com/w/c/language/storage_duration)
- [cppreference: Scope](https://en.cppreference.com/w/c/language/scope)
