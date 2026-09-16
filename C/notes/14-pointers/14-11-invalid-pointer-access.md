# 14-11. 잘못된 포인터 접근

pointer syntax가 맞아도 pointer value와 target lifetime·type이 유효하지 않으면 dereference는 안전하지 않다.

## 1. 학습 목표
- null·uninitialized·dangling pointer access를 분류한다.
- invalid examples를 실행하지 않고 분석한다.
- dereference 전 validity를 코드 흐름으로 보장한다.

## 2. 선수 지식
Step 14-8의 NULL과 Step 14-9의 lifetime을 안다.

## 3. 핵심 개념
`int *p; *p = 10;`은 p에 valid pointer value가 저장되지 않았다. `int *p = NULL; *p`는 어떤 object도 가리키지 않는다. lifetime이 끝난 local을 가리키던 pointer도 target access에 사용할 수 없다. 이런 경우를 특정 garbage value나 segmentation fault로 예측하지 않는다.

## 4. 문법
```c
int *pointer = NULL;
if (pointer != NULL) {
    printf("%d\n", *pointer);
}
```

잘못된 dereference examples는 실행하지 않고 원인을 분석한다.

## 5. 최소 코드 예제
```c
#include <stddef.h>
#include <stdio.h>

int main(void)
{
    int value = 25;
    int *pointer = NULL;

    if (pointer == NULL) {
        pointer = &value;
    }

    if (pointer != NULL) {
        printf("%d\n", *pointer);
    }
    return 0;
}
```

## 6. 코드 해석
pointer는 명시적인 null 상태로 시작한다. 첫 condition에서 valid value address를 저장하고 둘째 condition이 non-null을 확인한 뒤 25를 읽는다. invalid state에서는 dereference expression을 평가하지 않는다.

## 7. 내부 동작
[C17 abstract machine] invalid pointer dereference는 Undefined Behavior가 될 수 있다. uninitialized automatic pointer의 indeterminate value를 읽는 것 자체도 안전하지 않다. [OS] access violation이나 segmentation fault는 가능한 결과일 뿐 표준 보장이 아니다. compiler 성공도 runtime validity를 증명하지 않는다.

## 8. 자주 하는 실수
- compiler warning이 없으면 pointer가 valid라고 생각한다.
- uninitialized pointer를 NULL과 같은 값이라고 생각한다.
- invalid dereference 결과를 직접 실행해 분류한다.
- pointer address 숫자가 익숙해 보이면 valid라고 판단한다.

## 9. 필수 실습
null state에서 valid target을 설정한 뒤에만 dereference하는 흐름을 작성한다. [실습 README](../../exercises/14-pointers/14-11/README.md)

## 10. 추가 실습
- ★ null check branch
- ★★ 여러 states를 표로 추적
- ★★★ null·uninitialized·dangling cases를 실행 없이 분류

## 11. 확인 문제
1. uninitialized pointer와 NULL pointer의 차이는?
2. null dereference의 분류는?
3. dangling pointer target에는 어떤 문제가 있는가?
4. compile 성공이 validity를 증명하는가?
5. segmentation fault가 반드시 발생하는가?

## 12. 핵심 정리
dereference 전 pointer가 appropriate live object를 가리킨다는 흐름을 보장하고 invalid cases는 Undefined Behavior 분석 대상으로만 다룬다.

## 13. 다음 Step
[Step 14-12. 포인터 기초 종합 실습](14-12-pointer-basics-practice.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 3.4.3, 6.2.4, 6.5.3.2
- [cppreference: Undefined behavior](https://en.cppreference.com/w/c/language/behavior.html)
