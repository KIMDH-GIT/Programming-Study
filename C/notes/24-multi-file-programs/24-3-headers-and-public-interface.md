# 24-3. header file과 공개 interface
## 1. 학습 목표
- header를 공개 interface의 단일 출처로 사용한다.
- header에 적합한 declaration과 부적합한 ordinary definition을 구분한다.
- quote include와 angle include를 과도하게 단순화하지 않는다.
## 2. 선수 지식
24-2 declaration·definition과 Part 19의 `struct`를 안다.
## 3. 핵심 개념
header에는 caller가 알아야 할 function declarations와 공유 type definitions를 둔다. implementation detail은 `.c`에 남긴다.

```text
student.h  → struct Student, student_print declaration
student.c  → student_print definition
main.c     → student.h의 공개 interface 사용
```

header guard는 Part 25의 명시적 주제이므로 이번 Step의 핵심으로 앞당기지 않는다. 예제 header는 각 translation unit에서 한 번만 직접 include한다.
## 4. 문법
```c
#include <stdio.h>     /* implementation이 정한 system-header 탐색 */
#include "student.h"   /* 먼저 implementation이 정한 quote 탐색 */
```

`"..."`가 언제나 현재 directory만, `<...>`가 언제나 `/usr/include`만 찾는다는 뜻은 아니다. 정확한 search 방법은 C implementation과 build environment의 영향을 받는다.
## 5. 최소 코드 예제
`student.h`
```c
struct Student {
    int id;
    const char *name;
};

void student_print(const struct Student *student);
```

`student.c`
```c
#include "student.h"

#include <stdio.h>

void student_print(const struct Student *student)
{
    printf("%d %s\n", student->id, student->name);
}
```

`main.c`
```c
#include "student.h"

int main(void)
{
    const struct Student student = {1001, "Kim"};

    student_print(&student);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror \
    main.c student.c -o student_app
./student_app
```
## 6. 코드 해석
두 translation units가 같은 `struct Student` definition과 function declaration을 공유한다. 각 `.c`에 구조체를 복붙하지 않으므로 layout과 interface가 한 곳에서 유지된다.
## 7. 내부 동작
**[preprocessor]** include directive가 지정한 header의 source text를 처리한다.

**[C translation unit]** 각 source가 동일한 type·function declaration을 본다.

**[compiler]** `main.c`에서는 공개 type과 call을, `student.c`에서는 declaration과 definition의 일치를 검사한다.

**[linker]** `student_print` reference와 definition을 연결한다.

**[OS / loader]** header 파일 자체를 runtime에 별도로 적재하지 않는다.

**[CPU / ISA]** 공개 interface가 아니라 link된 function machine code를 실행한다.
## 8. 자주 하는 실수
- header에 `int global_count = 0;` 같은 ordinary external definition을 넣는다.
- shared object declaration을 원하면서 header에 `int count;`를 둔다. file scope의 이것은 단순 extern declaration이 아니라 tentative definition이다.
- 일반 external function body를 header에 넣고 여러 `.c`에서 include한다.
- 같은 `struct` definition을 source마다 복붙한다.
- 우연한 transitive include에 의존한다.
## 9. 필수 실습
공유 `struct Student`와 출력 함수 declaration을 `student.h`에 둔다.
[24-3 exercise](../../exercises/24-multi-file-programs/24-3/README.md)
## 10. 추가 실습
- ★ `student_has_id` 공개 함수를 추가한다.
- ★★ header를 include하는 최소 `header_check.c`를 만들어 독립 include를 확인한다.
- ★★★ 공개 interface와 private implementation 목록을 분류한다.
## 11. 확인 문제
1. header에 function declaration을 두는 이유는?
2. 공유 구조체를 header에 두는 장점은?
3. ordinary global definition을 header에 두면 무엇이 문제인가?
4. quote include와 angle include를 directory 하나로 단정할 수 없는 이유는?
5. header guard를 이번 Step에서 본격적으로 다루지 않는 이유는?
## 12. 핵심 정리
- header는 여러 translation units가 공유하는 공개 계약이다.
- implementation `.c`도 그 계약을 include한다.
- 공개 declaration과 ordinary external definition을 구분한다.
## 13. 다음 Step
[24-4. source file과 translation unit](24-4-source-files-and-translation-units.md)
## 14. 참고 자료
- N1570 6.2.7, 6.7.2.1, 6.10.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Source file inclusion](https://en.cppreference.com/w/c/preprocessor/include)
- [cppreference: Declarations](https://en.cppreference.com/w/c/language/declarations)
