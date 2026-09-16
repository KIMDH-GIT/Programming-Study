# 10-11. Part 10 종합 복습

Part 10에서는 함수의 선언·정의·호출, 값 전달, 반환, scope·lifetime, 호출 구현 모델과 역할 분리를 학습했다.

## 1. 학습 목표
- 함수 관련 용어와 C17 규칙을 종합한다.
- 함수 경계를 이용해 계산 역할을 분리한다.
- C abstract machine과 ABI 구현 설명을 구분한다.

## 2. 선수 지식
Step 10-1부터 10-10까지의 함수 개념과 Part 0~9의 표현식·조건·반복을 사용한다.

## 3. 핵심 개념
prototype은 호출 전에 함수 타입을 알리고 definition은 body를 제공한다. arguments의 값으로 parameters가 초기화되며 return 값은 호출식 결과가 된다. local automatic 객체는 호출별 lifetime을 갖는다. call stack의 물리적 형식은 C17이 아니라 구현과 ABI의 영역이다.

## 4. 문법
```c
int sum_to(int limit);

int sum_to(int limit)
{
    int sum = 0;
    for (int value = 1; value <= limit; ++value) {
        sum += value;
    }
    return sum;
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int sum_to(int limit);

int main(void)
{
    int result = sum_to(5);
    printf("%d\n", result);
    return 0;
}

int sum_to(int limit)
{
    int sum = 0;
    for (int value = 1; value <= limit; ++value) {
        sum += value;
    }
    return sum;
}
```

## 6. 코드 해석
prototype이 `main`의 호출 전에 함수 타입을 제공한다. argument 5로 parameter `limit`가 초기화되고 local `sum`에 1~5를 누적해 15를 반환한다. caller의 `result`가 반환값으로 초기화된다.

## 7. 내부 동작
[C17 표준] declaration이 보이는 호출, argument-to-parameter conversion, automatic object lifetime, return conversion이 적용된다. [컴파일러/ABI 관점] parameters·locals·return address의 실제 위치와 call instruction 사용 여부는 구현에 달린다. function pointer, array parameter, 여러 source file은 후속 Part에서 다룬다.

## 8. 자주 하는 실수
- declaration·definition·call을 혼동한다.
- C17에서 `f()`와 `f(void)`를 동일하게 설명한다.
- parameter 변경이 caller의 기본 객체를 바꾼다고 생각한다.
- 모든 함수 상태가 반드시 stack memory에 있다고 단정한다.
- 함수 하나에 입력·계산·출력을 모두 몰아넣는다.

## 9. 필수 실습
1부터 N까지의 합을 반환하는 함수와 결과를 출력하는 caller를 작성한다. [실습 README](../../exercises/10-functions/10-11/README.md)

## 10. 추가 실습
- ★ 홀짝 판별 함수
- ★★ factorial 계산 함수
- ★★★ declaration·definition·call·scope·lifetime 점검표

## 11. 확인 문제
1. prototype과 definition의 차이는?
2. parameter와 argument의 차이는?
3. `void f(void)`의 두 `void` 의미는?
4. parameter 변경이 caller 정수에 직접 반영되는가?
5. automatic object의 lifetime은 언제 끝나는가?
6. C17이 stack frame 형식을 보장하는가?
7. 선언 없이 함수를 호출하는 코드는 C17에 맞는가?

## 12. 핵심 정리
함수는 타입이 명확한 선언·정의·호출 계약이며, 값 전달과 local lifetime을 이해하고 구현 세부와 구분해 사용한다.

## 13. 다음 Step
Step 11-1. 배열 선언과 초기화

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.2.1, 6.2.4, 6.5.2.2, 6.7.6.3, 6.8.6.4, 6.9.1
- [cppreference: Functions](https://en.cppreference.com/w/c/language/functions.html)
- [GCC: C Dialect Options](https://gcc.gnu.org/onlinedocs/gcc/C-Dialect-Options.html)
