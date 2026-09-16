# 16-2. 포인터 매개변수로 주소 전달

pointer argument를 전달해도 C는 pass-by-value이며, pointer parameter object가 copied pointer value를 저장한다.

## 1. 학습 목표
- `&number` argument와 pointer parameter를 구분한다.
- pointer parameter도 caller pointer와 다른 object임을 설명한다.
- parameter 재지정과 target access를 구분한다.

## 2. 선수 지식
Part 14의 `&`, pointer object와 Part 10의 parameter를 안다.

## 3. 핵심 개념
`change(&number)`에서 `&number`는 number를 가리키는 pointer value다. parameter `pointer`는 그 value의 copy로 초기화되는 별도 `int *` object다. 따라서 `pointer = NULL;`은 local parameter만 바꾸며 caller object number나 caller의 다른 pointer variable을 바꾸지 않는다.

## 4. 문법
```c
void observe(int *pointer)
{
    printf("%p\n", (void *)pointer);
}
```

## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

void inspect(int *pointer)
{
    printf("stored value: %p\n", (void *)pointer);
    printf("parameter address: %p\n", (void *)&pointer);
    pointer = NULL;
}

int main(void)
{
    int number = 10;
    int *caller_pointer = &number;

    inspect(caller_pointer);
    printf("caller pointer: %p\n", (void *)caller_pointer);
    printf("caller value: %d\n", *caller_pointer);
    return 0;
}
```

## 6. 코드 해석
callee의 pointer와 caller_pointer는 같은 number를 가리키는 pointer values를 처음에 저장하지만 서로 다른 pointer objects다. callee에서 parameter를 NULL로 재지정해도 caller_pointer는 계속 number를 가리키고 10을 읽는다.

## 7. 내부 동작
[C17 표준] pointer value도 다른 scalar value처럼 parameter object를 초기화한다. [ABI] argument pointer가 register나 stack slot로 전달될 수 있지만 C가 call by reference로 바뀌는 것은 아니다. `&pointer`는 callee parameter object 자체의 address다.

## 8. 자주 하는 실수
- pointer parameter를 caller pointer variable과 같은 object라고 생각한다.
- parameter를 NULL로 바꾸면 caller pointer도 NULL이 된다고 생각한다.
- pointer argument를 call by reference라고 부른다.
- pointer parameter가 target object를 소유한다고 설명한다.

## 9. 필수 실습
pointer value와 pointer parameter object address를 출력하고 parameter 재지정 후 caller 상태를 확인한다. [실습 README](../../exercises/16-pointers-and-functions/16-2/README.md)

## 10. 추가 실습
- ★ pointer parameter를 다른 valid object로 재지정
- ★★ caller와 callee pointer states 표
- ★★★ pass-by-value와 ABI 전달 구분

## 11. 확인 문제
1. `&number`는 어떤 value인가?
2. pointer parameter는 별도 object인가?
3. `pointer = NULL`이 caller_pointer를 바꾸는가?
4. `pointer`와 `&pointer`의 차이는?
5. C가 call by reference로 바뀌는가?

## 12. 핵심 정리
pointer parameter도 copied pointer value를 저장하는 local object이며 재지정은 caller pointer 자체를 바꾸지 않는다.

## 13. 다음 Step
[Step 16-3. 역참조로 호출자 객체 변경](16-3-modify-caller-through-pointer.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.2.2, 6.5.3.2, 6.9.1
- [cppreference: Pointer declaration](https://en.cppreference.com/w/c/language/pointer.html)
