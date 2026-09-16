# 10-7. 값에 의한 전달

C 함수 호출에서는 argument 값을 이용해 별도의 parameter 객체를 초기화한다.

## 1. 학습 목표
- C의 pass by value를 parameter 초기화로 설명한다.
- parameter 변경과 caller 객체 변경을 구분한다.
- 현재 범위와 이후 pointer 학습 범위를 구분한다.

## 2. 선수 지식
Step 10-3의 parameter·argument와 Step 10-6의 호출별 객체를 안다.

## 3. 핵심 개념
기본 정수 argument를 넘기면 그 값을 복사해 parameter 객체를 초기화한다. 함수 안에서 parameter에 새 값을 대입해도 caller의 정수 객체 자체는 바뀌지 않는다. “C에서는 원본을 절대로 바꿀 수 없다”는 일반화는 부정확하다. 이후 pointer를 배우면 전달된 주소 값을 통해 다른 객체에 접근할 수 있지만, 호출 방식 자체는 여전히 pass by value다.

## 4. 문법
```c
void change(int value)
{
    value = 100;
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

void change(int value)
{
    value = 100;
    printf("%d\n", value);
}

int main(void)
{
    int number = 5;
    change(number);
    printf("%d\n", number);
    return 0;
}
```

## 6. 코드 해석
argument `number`의 값 5로 parameter `value`가 초기화된다. 함수는 자신의 `value`를 100으로 바꾸어 100을 출력한다. `main`의 별도 객체 `number`는 5이므로 다음 줄에 5를 출력한다.

## 7. 내부 동작
[C17 표준] argument는 대응 parameter type에 맞게 변환되고 parameter 객체를 초기화한다. [ABI 관점] “복사”는 의미 모델이며 반드시 memory-to-memory 복사 명령을 뜻하지 않는다. compiler는 register 전달이나 최적화를 사용할 수 있다.

## 8. 자주 하는 실수
- parameter 대입이 caller의 기본 정수 객체를 바꾼다고 생각한다.
- pass by value를 “항상 stack에 복사”라고 설명한다.
- caller 값을 바꾸려고 반환값도 사용하지 않고 parameter만 변경한다.
- 아직 배우지 않은 pointer를 필수 예제에 끌어온다.

## 9. 필수 실습
parameter를 두 배로 바꾸는 함수 안과 호출 뒤 caller 값을 각각 출력한다. [실습 README](../../exercises/10-functions/10-7/README.md)

## 10. 추가 실습
- ★ parameter에 0 대입
- ★★ 변경값을 return해 caller가 선택적으로 저장
- ★★★ pass by value와 ABI 전달 위치 차이 설명

## 11. 확인 문제
1. parameter 객체는 무엇으로 초기화되는가?
2. 예제에서 `value`와 `number`는 같은 객체인가?
3. 함수 내부 100 출력 뒤 caller 값은?
4. pass by value가 반드시 stack 복사를 뜻하는가?
5. pointer를 넘길 때도 호출 방식은 무엇인가?

## 12. 핵심 정리
C는 argument 값을 전달하며, parameter 변경은 caller의 기본 객체 변경이 아니다. 필요한 결과는 반환값으로 전달할 수 있다.

## 13. 다음 Step
[Step 10-8. call stack 기초](10-8-call-stack-basics.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.2.2, 6.9.1
- [cppreference: Function call](https://en.cppreference.com/w/c/language/operator_other.html#Function_call)
