# 16-3. 역참조로 호출자 객체 변경

pointer parameter를 dereference하면 copied pointer value가 가리키는 caller object를 지정해 수정할 수 있다.

## 1. 학습 목표
- `*pointer`와 `pointer` assignment의 대상을 구분한다.
- caller object가 바뀌는 정확한 이유를 설명한다.
- valid non-null·lifetime contract를 명시한다.

## 2. 선수 지식
Step 16-2의 pointer parameter와 Part 14의 indirection assignment를 사용한다.

## 3. 핵심 개념
`*pointer = 100;`은 local pointer parameter를 바꾸는 것이 아니라 pointer가 가리키는 caller `number` object를 수정한다. pointer value는 copy지만 target은 같은 object다. 이 동작을 C의 call by reference라고 부르지 않는다.

## 4. 문법
```c
void change(int *pointer)
{
    *pointer = 100;
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

void change(int *pointer)
{
    *pointer = 100;
}

int main(void)
{
    int number = 10;
    change(&number);
    printf("%d\n", number);
    return 0;
}
```

## 6. 코드 해석
`&number` pointer value가 parameter에 복사된다. `*pointer`가 caller의 number object를 지정하므로 assignment 후 number는 100이다.

```text
callee pointer parameter        caller number object
+---------------------+         +-----+
| copied pointer value | ------> |  10 |
+---------------------+         +-----+
          *pointer = 100 ------> | 100 |
                                +-----+
```

## 7. 내부 동작
[C17 표준] argument pointer value가 compatible parameter를 초기화하고 indirection expression이 live int object를 지정한다. NULL이나 dangling value를 dereference하면 contract 위반과 Undefined Behavior가 될 수 있다. [CPU] actual load/store는 implementation detail이다.

## 8. 자주 하는 실수
- `*pointer = 100`과 `pointer = NULL`을 같은 대상 수정으로 본다.
- caller object가 parameter로 직접 복사되었다고 설명한다.
- pointer 사용을 call by reference라고 부른다.
- valid target requirement를 빼먹는다.

## 9. 필수 실습
pointer parameter로 caller int를 두 번 수정하고 caller에서 결과를 확인한다. [실습 README](../../exercises/16-pointers-and-functions/16-3/README.md)

## 10. 추가 실습
- ★ double caller object 수정
- ★★ positive일 때만 수정
- ★★★ p 수정과 *p 수정 state diagram

## 11. 확인 문제
1. `pointer = NULL`의 assignment 대상은?
2. `*pointer = 100`의 대상은?
3. caller number가 바뀌는 이유는?
4. C의 argument passing 방식은?
5. pointer parameter가 요구하는 validity contract는?

## 12. 핵심 정리
copied pointer parameter를 dereference하면 같은 caller object를 지정할 수 있어 그 object의 stored value를 수정한다.

## 13. 다음 Step
[Step 16-4. 출력 매개변수](16-4-output-parameter.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.2.2, 6.5.3.2, 6.5.16
- [cppreference: Function call](https://en.cppreference.com/w/c/language/operator_other.html#Function_call)
