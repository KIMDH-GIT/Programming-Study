# 14-9. 유효한 역참조 대상과 object lifetime

dereference가 유효하려면 pointer value가 현재 lifetime 안에 있는 적절한 type의 object를 가리켜야 한다.

## 1. 학습 목표
- valid dereference의 object·type·lifetime 조건을 설명한다.
- lifetime이 끝난 object를 가리키던 pointer 문제를 분석한다.
- pointer가 object lifetime을 연장하지 않음을 이해한다.

## 2. 선수 지식
Part 10의 automatic object lifetime과 Step 14-8의 null 상태를 사용한다.

## 3. 핵심 개념
pointer value가 남아 있는 것처럼 보여도 pointed-to object의 lifetime이 끝나면 그 object에 접근할 수 없다. block local object는 block 실행을 떠날 때 lifetime이 끝난다. pointer는 object를 소유하지 않고 lifetime도 연장하지 않는다. C17에서는 object lifetime 종료 시 그 object를 가리키던 pointer value가 indeterminate가 될 수 있으므로 읽거나 비교하지 않고 새 valid value를 저장한다.

## 4. 문법
```c
int *pointer = NULL;
{
    int local = 42;
    pointer = &local;
    printf("%d\n", *pointer); /* lifetime 안 */
}
pointer = NULL;               /* stale value를 읽지 않고 교체 */
```

## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

int main(void)
{
    int *pointer = NULL;

    {
        int local = 42;
        pointer = &local;
        printf("%d\n", *pointer);
    }

    pointer = NULL;
    if (pointer == NULL) {
        printf("no valid target\n");
    }
    return 0;
}
```

## 6. 코드 해석
inner block 안에서는 local lifetime이 유효하므로 dereference해 42를 읽는다. block을 떠난 뒤 old pointed object에는 접근하지 않고 pointer에 NULL을 새로 저장한다.

## 7. 내부 동작
[C17 abstract machine] object lifetime 밖에서 저장된 value를 access하기 위해 lvalue를 사용하면 Undefined Behavior가 된다. lifetime 종료가 pointer 자체의 lifetime 종료와 같지는 않지만 old pointer value의 사용도 안전하지 않을 수 있다. [implementation] stack reuse는 흔한 현상이지만 C 규칙의 정의가 아니다.

## 8. 자주 하는 실수
- pointer가 존재하면 target object도 살아 있다고 생각한다.
- local lifetime 종료 후 pointer를 dereference한다.
- dangling pointer가 항상 NULL로 자동 변경된다고 생각한다.
- stack 주소가 아직 같은 숫자로 보이면 유효하다고 판단한다.

## 9. 필수 실습
inner block lifetime 안에서만 local object를 dereference하고 밖에서는 pointer를 새 상태로 교체한다. [실습 README](../../exercises/14-pointers/14-9/README.md)

## 10. 추가 실습
- ★ outer block object lifetime 관찰
- ★★ 두 nested blocks의 distinct objects
- ★★★ pointer lifetime과 target lifetime diagram

## 11. 확인 문제
1. valid dereference에 필요한 세 조건은?
2. pointer가 target lifetime을 연장하는가?
3. block local lifetime은 언제 끝나는가?
4. dangling pointer가 자동으로 NULL이 되는가?
5. 같은 address 숫자 관찰이 validity를 증명하는가?

## 12. 핵심 정리
pointer validity는 representation이 아니라 pointed-to object의 type과 현재 lifetime에 달려 있으며 pointer 자체가 lifetime을 연장하지 않는다.

## 13. 다음 Step
[Step 14-10. 포인터 타입과 접근형](14-10-pointer-type-access.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.2.4, 6.5.3.2
- [cppreference: Object lifetime](https://en.cppreference.com/w/c/language/lifetime.html)
