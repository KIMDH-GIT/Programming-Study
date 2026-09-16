# 16-1. 값 매개변수로 원본을 바꾸지 못하는 이유

C 함수 호출에서는 argument value가 별도의 parameter object를 초기화하므로 parameter assignment는 caller object를 직접 바꾸지 않는다.

## 1. 학습 목표
- caller object·argument value·parameter object를 구분한다.
- pass-by-value를 C17 object semantics로 설명한다.
- 같은 identifier 이름과 같은 object를 혼동하지 않는다.

## 2. 선수 지식
Part 10의 parameter·argument·scope와 Part 14의 object 개념을 사용한다.

## 3. 핵심 개념
`change(number)`에서 caller의 `number` value 10이 argument value다. called function의 parameter `value`는 그 값으로 초기화되는 별도 int object다. `value = 100;`은 parameter object만 수정하므로 caller의 `number`는 10으로 남는다. 이유는 함수가 변수 이름을 모르기 때문이 아니라 두 objects가 별도이기 때문이다.

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
    printf("parameter: %d\n", value);
}

int main(void)
{
    int number = 10;
    change(number);
    printf("caller: %d\n", number);
    return 0;
}
```

## 6. 코드 해석
callee 안에서 parameter는 100으로 바뀌어 출력된다. caller로 돌아오면 별도 object `number`는 10이다.

```text
caller object             callee parameter object
number                    value
+----+   value 10 copy    +----+
| 10 | -----------------> | 10 |
+----+                    +----+
                          value = 100
                          +-----+
                          | 100 |
                          +-----+
```

## 7. 내부 동작
[C17 표준] function call argument는 parameter type에 맞게 conversion되고 parameter object를 초기화한다. [compiler/ABI] 값이 register나 stack을 통해 전달될 수 있지만 이는 pass-by-value 의미의 구현 방법이다. [CPU] 실제 move/store instruction은 optimization에 따라 달라진다.

## 8. 자주 하는 실수
- parameter와 caller variable을 같은 object라고 생각한다.
- identifier 이름이 같으면 object도 같다고 생각한다.
- register/stack 전달 방식을 C 언어의 인자 전달 정의라고 설명한다.
- 값을 바꾸지 못하는 이유를 “함수가 이름을 모른다”로만 설명한다.

## 9. 필수 실습
값 parameter를 변경하고 callee와 caller에서 각각 출력해 두 objects를 비교한다. [실습 README](../../exercises/16-pointers-and-functions/16-1/README.md)

## 10. 추가 실습
- ★ parameter에 0 대입
- ★★ caller와 parameter에 같은 identifier 사용
- ★★★ object diagram과 state 변화 작성

## 11. 확인 문제
1. argument로 전달되는 것은 identifier인가 value인가?
2. parameter는 별도 object인가?
3. callee에서 100으로 바꾼 뒤 caller value는?
4. C의 argument passing 방식은?
5. register 전달이면 pass-by-value가 아닌가?

## 12. 핵심 정리
C는 argument value로 별도 parameter object를 초기화하므로 값 parameter 수정은 caller object 수정이 아니다.

## 13. 다음 Step
[Step 16-2. 포인터 매개변수로 주소 전달](16-2-pointer-parameter-address.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.2.2, 6.9.1
- [cppreference: Function call](https://en.cppreference.com/w/c/language/operator_other.html#Function_call)
