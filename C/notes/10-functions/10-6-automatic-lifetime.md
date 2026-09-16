# 10-6. 자동 객체의 lifetime

함수의 일반적인 parameter와 local variable은 호출마다 새로 생겨 함수 실행이 끝날 때 lifetime이 끝나는 automatic 객체다.

## 1. 학습 목표
- automatic storage duration 객체의 lifetime을 설명한다.
- 여러 호출의 local 상태가 이어지지 않음을 확인한다.
- scope와 storage duration·lifetime을 구분한다.

## 2. 선수 지식
Step 10-5의 local variable과 scope를 안다.

## 3. 핵심 개념
함수 body의 일반 local variable과 parameter는 automatic storage duration을 가진다. 각 함수 호출에서 해당 객체의 lifetime이 시작되고 block 실행을 떠나면 끝난다. 다음 호출에서는 같은 이름이어도 새 객체가 만들어지고, 이전 호출의 local 값이 자동으로 보존되지 않는다.

## 4. 문법
```c
int next_value(int start)
{
    int local = start;
    ++local;
    return local;
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int increment_copy(int value)
{
    int result = value + 1;
    return result;
}

int main(void)
{
    printf("%d\n", increment_copy(5));
    printf("%d\n", increment_copy(5));
    return 0;
}
```

## 6. 코드 해석
두 호출은 각각 새로운 parameter `value`와 local `result`를 가진다. 첫 호출의 6이 둘째 호출의 시작값으로 남지 않으므로 두 줄 모두 6이다.

## 7. 내부 동작
[C17 표준] automatic storage duration 객체의 storage는 관련 block에 들어갈 때 보장되고 block 실행을 떠날 때 유지가 끝난다. [컴파일러/ABI 관점] 객체가 실제 stack memory에 놓인다는 보장은 없다. register에 있거나 최적화로 별도 저장 공간이 사라질 수도 있다.

## 8. 자주 하는 실수
- local variable이 다음 호출에도 값을 기억한다고 생각한다.
- automatic 객체는 반드시 hardware stack에 있다고 단정한다.
- scope와 lifetime이 항상 같은 경계라고 일반화한다.
- 초기화하지 않은 automatic 객체 값을 읽는다.

## 9. 필수 실습
같은 argument로 함수를 두 번 호출해 local 결과가 호출마다 다시 계산됨을 확인한다. [실습 README](../../exercises/10-functions/10-6/README.md)

## 10. 추가 실습
- ★ 서로 다른 arguments로 두 번 호출
- ★★ block을 하나 추가해 local lifetime 설명
- ★★★ compiler 최적화와 C abstract machine 차이를 글로 정리

## 11. 확인 문제
1. 일반 local 객체의 lifetime은 언제 시작하고 끝나는가?
2. 다음 호출이 이전 local 값을 자동으로 이어받는가?
3. automatic storage duration은 반드시 stack 배치를 뜻하는가?
4. 초기화하지 않은 automatic 객체를 읽어도 되는가?

## 12. 핵심 정리
automatic 객체는 호출별로 lifetime을 가지며, C17은 그 의미를 규정하지만 물리적 stack 배치를 요구하지 않는다.

## 13. 다음 Step
[Step 10-7. 값에 의한 전달](10-7-pass-by-value.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.2.4, 6.9.1
- [cppreference: Storage duration](https://en.cppreference.com/w/c/language/storage_duration.html)
