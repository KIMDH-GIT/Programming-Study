# 16-4. 출력 매개변수

output parameter는 pointer parameter를 통해 함수 결과를 caller objects에 기록하는 관용적 설계 패턴이다.

## 1. 학습 목표
- return value와 output parameters의 역할을 구분한다.
- 여러 결과를 caller objects에 안전하게 기록한다.
- output parameter가 별도 C 문법 종류는 아님을 설명한다.

## 2. 선수 지식
Step 16-3의 caller object 수정과 Part 7의 division boundary를 사용한다.

## 3. 핵심 개념
함수 return value 하나는 success 여부에 쓰고 quotient와 remainder는 pointer parameters를 통해 기록할 수 있다. “output parameter”는 일반 pointer parameter의 사용 목적을 나타내는 관용어이지 C grammar의 별도 parameter kind가 아니다. caller는 valid pointers를 제공해야 한다.

## 4. 문법
```c
int divide_values(
    int dividend,
    int divisor,
    int *quotient,
    int *remainder
);
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int divide_values(
    int dividend,
    int divisor,
    int *quotient,
    int *remainder
)
{
    if (divisor == 0) {
        return 0;
    }

    *quotient = dividend / divisor;
    *remainder = dividend % divisor;
    return 1;
}

int main(void)
{
    int quotient;
    int remainder;

    if (divide_values(17, 5, &quotient, &remainder)) {
        printf("%d %d\n", quotient, remainder);
    }
    return 0;
}
```

## 6. 코드 해석
divisor 5는 nonzero라 two output pointers를 dereference해 3과 2를 기록하고 success 1을 반환한다. caller는 success branch 안에서 initialized outputs를 출력한다.

## 7. 내부 동작
[C17 표준] four argument values가 four parameter objects를 초기화한다. output pointers는 caller objects를 가리키며 lifetime과 type이 valid해야 한다. divisor 0 branch는 division과 output writes를 수행하지 않는다. [ABI] pointer arguments의 register/stack 위치는 C 규칙이 아니다.

## 8. 자주 하는 실수
- failure에서도 uninitialized outputs를 읽는다.
- divisor validation 뒤가 아니라 division 뒤에 검사한다.
- output parameter를 특별한 pass-by-reference syntax라고 설명한다.
- NULL을 허용하지 않는 contract인데 NULL을 전달한다.

## 9. 필수 실습
quotient와 remainder를 output parameters로 기록하고 success일 때만 출력한다. [실습 README](../../exercises/16-pointers-and-functions/16-4/README.md)

## 10. 추가 실습
- ★ sum과 difference 두 outputs
- ★★ divisor 0 failure branch
- ★★★ return result와 output result 역할표

## 11. 확인 문제
1. output parameter는 별도 C grammar인가?
2. return value는 무엇에 사용하는가?
3. failure에서 outputs를 읽어도 되는가?
4. caller가 보장할 pointer contract는?
5. divisor 0에서 어떤 operations를 피하는가?

## 12. 핵심 정리
output parameters는 valid caller object pointers로 여러 결과를 기록하는 관용 패턴이며 success contract와 초기화 시점을 함께 설계한다.

## 13. 다음 Step
[Step 16-5. `swap(int *a, int *b)`](16-5-swap.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.2.2, 6.5.5, 6.5.3.2
- [cppreference: Function call](https://en.cppreference.com/w/c/language/operator_other.html#Function_call)
