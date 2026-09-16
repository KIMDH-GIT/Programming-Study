# 15-9. 시작 전 포인터를 만들지 않는 역순 순회

역순 pointer traversal은 one-past에서 시작해 먼저 감소한 뒤 dereference하면 array 시작 전 pointer를 만들지 않는다.

## 1. 학습 목표
- one-past endpoint에서 안전하게 reverse traversal한다.
- before-begin pointer 생성을 피한다.
- decrement와 dereference 순서를 추적한다.

## 2. 선수 지식
Step 15-6의 one-past와 Step 15-8의 pointer loop를 사용한다.

## 3. 핵심 개념
`current = values + count`는 valid one-past pointer다. `current != values`인 동안 먼저 `--current`해 last remaining element를 가리킨 후 dereference한다. first element를 읽은 뒤 current는 values와 같고 loop가 종료되어 `values-1`을 계산하지 않는다.

## 4. 문법
```c
int *current = values + count;
while (current != values) {
    --current;
    use(*current);
}
```

## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    int values[5] = {10, 20, 30, 40, 50};
    int *current = values + 5;

    while (current != values) {
        --current;
        printf("%d\n", *current);
    }
    return 0;
}
```

## 6. 코드 해석
current는 one-past에서 시작한다. 첫 decrement 후 last element 50을 읽고, 마지막 iteration에서는 values를 가리켜 10을 읽는다. 그 다음 condition이 false라 더 감소하지 않는다.

## 7. 내부 동작
[C17 abstract machine] one-past pointer에서 last element로 감소하는 것은 same array bounds 안이다. array 시작 전 pointer를 만드는 것은 정의된 traversal technique가 아니므로 loop condition과 operation order가 이를 막는다. reverse iteration에 unrelated pointer comparison은 필요 없다.

## 8. 자주 하는 실수
- values-1을 종료용 pointer로 미리 만든다.
- one-past를 감소하기 전에 dereference한다.
- first element를 읽은 뒤 한 번 더 감소한다.
- `*current--` 같은 compact expression으로 순서를 숨긴다.

## 9. 필수 실습
one-past에서 시작해 다섯 elements를 reverse order로 출력한다. [실습 README](../../exercises/15-arrays-and-pointers/15-9/README.md)

## 10. 추가 실습
- ★ count 1 reverse
- ★★ reverse sum
- ★★★ current position 추적표

## 11. 확인 문제
1. current의 초기 pointer는?
2. 첫 dereference 전에 어떤 operation을 하는가?
3. values-1 pointer를 만드는가?
4. first element를 언제 읽는가?
5. `*current--`를 피하는 이유는?

## 12. 핵심 정리
one-past에서 먼저 감소하고 읽으면 valid element pointers만 사용하며 before-begin pointer를 만들지 않는다.

## 13. 다음 Step
[Step 15-10. 포인터만 이용한 배열 실습](15-10-pointer-only-array-practice.md)

## 14. 참고 자료
- [WG14 N2176](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2176.pdf): 6.5.6
- [cppreference: Pointer arithmetic](https://en.cppreference.com/w/c/language/operator_arithmetic.html#Pointer_arithmetic)
