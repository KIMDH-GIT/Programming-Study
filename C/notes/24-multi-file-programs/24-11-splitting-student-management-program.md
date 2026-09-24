# 24-11. 학생 관리 프로그램 파일 분리
## 1. 학습 목표
- 작은 학생 관리 프로그램을 UI와 student 기능으로 나눈다.
- 공유 type과 public API를 header에 배치한다.
- private helper와 state를 implementation 안에 제한한다.
## 2. 선수 지식
Part 19의 구조체, Part 23의 저장 개념, 24-1부터 24-10까지를 안다.
## 3. 핵심 개념
이번 Step은 학생 한 명을 출력·판정하는 작은 slice만 분리한다. Project 2의 database·file persistence 전체를 미리 구현하지 않는다.

```text
student.h → struct Student와 public declarations
student.c → public definitions와 static private helper
main.c    → 입력 예시를 만들고 public API만 사용
```

caller가 알아야 할 계약만 header에 공개한다.
## 4. 문법
```c
struct Student {
    int id;
    const char *name;
    double score;
};

int student_is_passing(const struct Student *student);
void student_print(const struct Student *student);
```
## 5. 최소 코드 예제
`student.h`
```c
struct Student {
    int id;
    const char *name;
    double score;
};

int student_is_passing(const struct Student *student);
void student_print(const struct Student *student);
```

`student.c`
```c
#include "student.h"

#include <stdio.h>

static const char *student_result(
    const struct Student *student)
{
    return student->score >= 60.0 ? "PASS" : "FAIL";
}

int student_is_passing(const struct Student *student)
{
    return student->score >= 60.0;
}

void student_print(const struct Student *student)
{
    printf("%d %s %.1f %s\n",
           student->id,
           student->name,
           student->score,
           student_result(student));
}
```

`main.c`
```c
#include "student.h"

int main(void)
{
    const struct Student student = {1001, "Kim", 87.5};

    if (!student_is_passing(&student)) {
        return 1;
    }
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
공유 struct layout과 두 public prototypes는 header에 있다. `student_result`는 출력 구현만 사용하는 `static` helper다. global state를 만들지 않고 필요한 data를 pointer parameter로 전달한다.
## 7. 내부 동작
**[preprocessor]** 두 source가 같은 public type과 prototypes를 얻는다.

**[C translation unit]** `main.c`와 `student.c`는 각자 독립적으로 type-check된다.

**[compiler]** public definition과 header declaration의 compatibility를 확인하고 private helper를 internal linkage로 처리한다.

**[linker]** 두 public function references와 definitions를 연결한다.

**[OS / loader]** 완성된 `student_app`을 적재한다. file persistence는 이 예제 범위가 아니다.

**[CPU / ISA]** struct member access와 calls에 해당하는 machine instructions를 실행한다.
## 8. 자주 하는 실수
- `struct Student`를 두 `.c`에 복붙한다.
- private helper를 public header에 공개한다.
- 모든 학생 데이터를 global object로 만든다.
- `student.c`가 자신의 header를 include하지 않는다.
- C 파일 분리가 namespace를 자동 생성한다고 생각한다.
## 9. 필수 실습
학생 type과 pass 판정·출력 기능을 세 파일로 분리한다.
[24-11 exercise](../../exercises/24-multi-file-programs/24-11/README.md)
## 10. 추가 실습
- ★ 학생 두 명을 배열에 넣어 출력한다.
- ★★ 평균을 계산하는 public function을 추가한다.
- ★★★ public names에 `student_` prefix가 필요한 이유를 collision 관점에서 설명한다.
## 11. 확인 문제
1. 공유 struct definition은 어디에 두는가?
2. private helper에 `static`을 붙인 이유는?
3. implementation `.c`가 자신의 header를 include하는 이유는?
4. global state 없이 data를 전달하는 방법은?
5. file 분리가 C namespace를 만드는가?
6. 이번 Step에서 persistence 전체를 구현하지 않는 이유는?
## 12. 핵심 정리
- header는 shared type과 public API의 단일 출처다.
- source는 public definitions와 private implementation을 소유한다.
- 작은 책임 단위로 나누고 curriculum 밖 build system은 추가하지 않는다.
## 13. 다음 Step
[24-12. Part 24 종합 복습](24-12-part-24-review.md)
## 14. 참고 자료
- N1570 6.2.2, 6.2.7, 6.7.2.1, 6.9. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Struct declaration](https://en.cppreference.com/w/c/language/struct)
- [cppreference: Storage duration and linkage](https://en.cppreference.com/w/c/language/storage_duration)
