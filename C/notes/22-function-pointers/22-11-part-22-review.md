# 22-11. Part 22 종합 복습
## 1. 학습 목표
- function pointer 선언·호환성·callback·dispatch·driver interface를 종합한다.
- C17 type rules와 compiler·ABI·CPU 구현을 구분한다.
- invalid function pointer 사용을 실행하지 않고 판별한다.
## 2. 선수 지식
22-1부터 22-10까지를 학습했다.
## 3. 핵심 개념
function은 object가 아니며 function pointer object와 구분한다. raw declarator를 identifier에서 읽고 compatible function pointer value만 대입·호출한다. callback parameter도 pass-by-value이며 array·state table을 사용할 때는 bounds를 검사한다.

ISO C17은 object pointer와 function pointer를 별도 category로 다룬다. `void *`의 일반적인 object pointer conversion을 function pointer에 확대 적용하지 않는다.
## 4. 문법
```c
typedef int (*BinaryOperation)(int, int);

struct OperationEntry {
    const char *name;
    BinaryOperation operation;
};
```
`name`은 character object를 가리키는 object pointer이고 `operation`은 function pointer다.
## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

typedef int (*BinaryOperation)(int, int);

int add(int a, int b) { return a + b; }
int subtract(int a, int b) { return a - b; }
int multiply(int a, int b) { return a * b; }

struct OperationEntry {
    const char *name;
    BinaryOperation operation;
};

int main(void)
{
    const struct OperationEntry operations[] = {
        {"add", add},
        {"subtract", subtract},
        {"multiply", multiply}
    };
    size_t count = sizeof operations / sizeof operations[0];
    size_t index = 1u;

    if (index < count && operations[index].operation != NULL) {
        printf("%s: %d\n",
               operations[index].name,
               operations[index].operation(9, 4));
    }
    return 0;
}
```
## 6. 코드 해석
각 table element는 display name object pointer와 compatible function pointer를 함께 가진다. bounds와 null을 검사한 뒤 선택된 operation을 호출한다.
## 7. 내부 동작
**[C17 type system]** function designator conversion, compatible function type, pointer equality, array bounds가 source-level correctness를 결정한다.

**[compiler]** diagnostics와 optimization을 수행하고 direct 또는 indirect call sequence를 생성한다.

**[ABI]** pointer representation과 calling convention을 정한다. function pointer가 항상 object pointer와 같은 representation·size라는 ISO C17 보장은 없다.

**[CPU / ISA]** indirect branch/call instruction을 사용할 수 있지만 C expression과 특정 instruction 사이의 일대일 대응은 보장되지 않는다.

**[MIPS — 수업 기준]** function pointer call은 `jalr`와 연결될 수 있다.

**[RISC-V — 병행 학습]** indirect call은 `jalr`와 연결될 수 있다. 두 ISA의 instruction name이 같더라도 encoding·ABI·register convention을 같은 것으로 취급하지 않는다.
## 8. 자주 하는 실수
- `int (*f)(int)`와 `int *f(int)`를 혼동한다.
- function 이름 자체의 type이 항상 pointer라고 말한다.
- `f`와 `&f`의 변환 전 type이 같다고 말한다.
- incompatible signature를 cast로 해결한다.
- null·uninitialized function pointer를 호출한다.
- function pointer arithmetic을 한다.
- function pointer를 `(void *)`로 바꿔 `%p`에 출력하는 것을 portable C라고 단정한다.
- callback을 call-by-reference나 automatic closure라고 설명한다.
- bounds 없이 dispatch table을 indexing한다.
## 9. 필수 실습
세 arithmetic functions, typedef, named dispatch table, bounds·null checks를 한 program에 통합한다.
[22-11 exercise](../../exercises/22-function-pointers/22-11/README.md)
## 10. 추가 실습
- ★ 선택 index를 바꿔 모든 operations를 확인한다.
- ★★ enum과 table index를 연결한다.
- ★★★ driver interface와 callback을 추가하되 모든 signatures를 문서화한다.
## 11. 확인 문제
1. function과 function pointer object의 차이는?
2. function-to-pointer conversion이 억제되는 context는?
3. incompatible function pointer call을 cast가 해결하지 못하는 이유는?
4. callback parameter는 어떤 방식으로 전달되는가?
5. object pointer와 function pointer의 `void *` 규칙이 다른 이유는?
6. dispatch table에서 bounds와 null을 모두 검사하는 이유는?
7. C17·compiler·ABI·CPU 설명을 분리해야 하는 이유는?
## 12. 핵심 정리
- declarator와 function type compatibility를 먼저 확인한다.
- callback·array·driver interface는 같은 function pointer 규칙 위에 있다.
- portability 경계를 지키고 invalid call과 out-of-bounds dispatch를 실행하지 않는다.
## 13. 다음 Step
Part 23의 첫 Step은 **23-1. `FILE *`, stream, `fopen`, `fclose`**이다. 이번 Part에서는 Part 23 파일을 만들지 않는다.
## 14. 참고 자료
- N1570 6.2.5, 6.2.7, 6.3.2.1, 6.3.2.3, 6.5.2.2, 6.5.3.2, 6.5.6, 6.5.9, 6.7.6.1~6.7.6.3, 6.7.8. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: C language](https://en.cppreference.com/w/c/language)
- [GCC: C extensions](https://gcc.gnu.org/onlinedocs/gcc/C-Extensions.html)
