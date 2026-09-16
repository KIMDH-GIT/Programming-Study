# 16-5. `swap(int *a, int *b)`

값 parameters만 바꾸는 swap은 caller objects를 바꾸지 못하지만 pointer parameters를 dereference하면 두 caller int objects를 교환할 수 있다.

## 1. 학습 목표
- failed value swap과 pointer swap을 비교한다.
- parameter pointers와 pointed-to caller objects를 구분한다.
- temporary variable을 이용해 값을 잃지 않는다.

## 2. 선수 지식
Step 16-1의 value parameters와 Step 16-3의 pointed-to modification을 안다.

## 3. 핵심 개념
`swap_values(int a, int b)`의 a, b는 copies라 caller에 영향이 없다. `swap(int *a, int *b)`의 a, b도 pointer value copies지만 `*a`, `*b`는 caller의 actual int objects를 지정한다. temp가 첫 값을 보존한 상태에서 세 assignments를 수행한다.

## 4. 문법
```c
void swap(int *a, int *b)
{
    int temp = *a;
    *a = *b;
    *b = temp;
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

void swap_values(int a, int b)
{
    int temp = a;
    a = b;
    b = temp;
}

void swap(int *a, int *b)
{
    int temp = *a;
    *a = *b;
    *b = temp;
}

int main(void)
{
    int first = 10;
    int second = 20;

    swap_values(first, second);
    printf("%d %d\n", first, second);

    swap(&first, &second);
    printf("%d %d\n", first, second);
    return 0;
}
```

## 6. 코드 해석
value version 뒤 caller는 10,20 그대로다. pointer version은 first와 second addresses를 받아 `*a`, `*b`로 caller objects를 바꾸므로 20,10이 된다.

## 7. 내부 동작
[C17 표준] 두 calls 모두 pass-by-value다. 차이는 second function이 copied pointer values를 통해 caller objects를 designate한다는 점이다. a와 b가 같은 object를 가리켜도 이 implementation은 defined이며 최종값은 그대로지만 별도 alias contract를 설명할 수 있다.

## 8. 자주 하는 실수
- value swap이 caller를 바꾼다고 생각한다.
- pointer swap을 call by reference라고 부른다.
- temp 없이 첫 값을 덮어쓴다.
- `a`와 `*a`를 같은 object라고 생각한다.

## 9. 필수 실습
failed value swap과 successful pointer swap을 같은 caller values로 비교한다. [실습 README](../../exercises/16-pointers-and-functions/16-5/README.md)

## 10. 추가 실습
- ★ double swap
- ★★ same object address를 두 번 전달
- ★★★ 두 function의 object diagram 비교

## 11. 확인 문제
1. value swap이 caller를 바꾸지 못하는 이유는?
2. pointer swap의 a와 b는 무엇을 저장하는가?
3. `*a`와 `*b`는 무엇을 지정하는가?
4. temp가 필요한 이유는?
5. pointer swap도 pass-by-value인가?

## 12. 핵심 정리
pointer swap은 copied pointer parameters를 dereference해 caller의 두 int objects를 수정하며 C의 pass-by-value 원칙은 유지된다.

## 13. 다음 Step
[Step 16-6. 배열 매개변수와 pointer adjustment](16-6-array-parameter-adjustment.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.2.2, 6.5.3.2
- [cppreference: Function call](https://en.cppreference.com/w/c/language/operator_other.html#Function_call)
