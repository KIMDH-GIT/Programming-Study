# 27-6. `NULL` pointer dereference
## 1. 학습 목표
- null pointer와 유효한 object pointer를 구분한다.
- null pointer dereference의 UB를 언어 규칙으로 설명한다.
- null representation과 machine address zero를 동일시하지 않는다.
## 2. 선수 지식
Part 14의 `NULL`, pointer, 역참조와 Part 18의 allocation failure를 안다.
## 3. 핵심 개념
null pointer는 어떤 object나 function도 가리키지 않는 특별한 pointer value다. 역참조 연산자는 pointer가 유효한 object 또는 function을 가리키는 조건을 요구한다.

⚠ 분석용 — 실행하지 않는다.
```c
int *p = NULL;
int value = *p; /* UB */
```
근본 이유는 “반드시 address 0에 접근하기 때문”이 아니라 null pointer가 object를 가리키지 않기 때문이다. null pointer의 representation이 all-bits-zero 또는 machine address zero라고 C17이 일반 보장하지 않는다.
## 4. 문법
```c
if (p != NULL) {
    use(*p);
}
```
`malloc` 결과, lookup 결과, optional output pointer 등 각 API contract에 맞춰 검사한다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

static int read_value(const int *p, int *result)
{
    if (p == NULL || result == NULL) {
        return 0;
    }
    *result = *p;
    return 1;
}

int main(void)
{
    int value = 42;
    int result;

    if (read_value(&value, &result)) {
        printf("%d\n", result);
    }
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o null_check
./null_check
```
## 6. 코드 해석
두 pointer를 검사하고 살아 있는 `value` object만 읽는다. 출력은 `42`다.
## 7. 내부 동작
**[C17]** null pointer constant의 변환은 null pointer를 만들며, 그 값은 어떤 object와도 같지 않다. invalid pointer를 unary `*`의 operand로 사용해 실제 access하면 UB다.

**[OS observation]** 일부 hosted OS가 low address 접근에 SIGSEGV를 낼 수 있지만 C17 결과가 아니다.

**[sanitizer observation]** UBSan/ASan report와 종료 방식은 instrumentation runtime behavior다.

**[CPU / ISA]** address translation과 hardware fault는 언어 의미 다음 층이다.
## 8. 자주 하는 실수
- `NULL`은 반드시 숫자 주소 0이라고 한다.
- segfault가 나야만 null dereference라고 판단한다.
- pointer cast가 성공하면 역참조도 안전하다고 생각한다.
- `assert(p != NULL)`이 모든 build와 모든 control path를 정의된 동작으로 바꾼다고 생각한다.
## 9. 필수 실습
nullable input을 받는 함수를 작성하고 null이면 역참조 전에 실패를 반환한다.
[27-6 exercise](../../exercises/27-undefined-behavior/27-6/README.md)
## 10. 추가 실습
- ★ null 입력의 실패 반환을 확인한다.
- ★★ `malloc` 실패 처리 흐름과 연결한다.
- ★★★ 함수 주석에 nullable contract를 명시한다.
## 11. 확인 문제
1. null pointer는 무엇을 가리키는가?
2. null dereference의 근본 언어 규칙은?
3. `NULL` representation이 address zero로 보장되는가?
4. SIGSEGV는 C17이 요구하는가?
5. pointer cast 성공이 dereference 안전을 뜻하는가?
## 12. 핵심 정리
- null pointer는 object를 가리키지 않아 역참조할 수 없다.
- representation과 OS fault를 C17 정의로 사용하지 않는다.
- boundary에서 null을 검사하고 contract를 명시한다.
## 13. 다음 Step
[27-7. dangling pointer와 object lifetime](27-7-dangling-pointer-and-object-lifetime.md)
## 14. 참고 자료
- N1570 6.3.2.3p3, 6.5.3.2p4, 7.19, Annex J.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: Null pointers](https://en.cppreference.com/w/c/language/pointer)
