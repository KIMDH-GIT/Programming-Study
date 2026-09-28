# 29-11. Prime Test
## 1. 학습 목표
- prime number input domain과 결과 contract를 정의한다.
- square root 범위의 divisor만 검사하는 이유를 설명한다.
- `divisor * divisor` overflow를 division 형태로 피한다.
## 2. 선수 지식
Part 7의 arithmetic과 29-10의 divisibility loop를 안다.
## 3. 핵심 개념
**[Algorithm]** 2 이상인 정수 `n`이 composite이면 적어도 하나의 divisor는 `sqrt(n)` 이하에 있다. 따라서 2부터 그 범위까지만 나누어 본다.

입력·출력 contract:

```text
input: unsigned int value
output: prime이면 1, 아니면 0
0과 1은 prime이 아니다.
```
## 4. 문법
```c
for (unsigned int divisor = 2U;
     divisor <= value / divisor;
     ++divisor) {
    if (value % divisor == 0U) {
        return 0;
    }
}
```

`divisor <= value / divisor`는 같은 범위 의미를 표현하면서 `divisor * divisor`의 unsigned wrap 가능성을 피한다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

static int is_prime(unsigned int value)
{
    if (value < 2U) {
        return 0;
    }
    for (unsigned int divisor = 2U;
         divisor <= value / divisor;
         ++divisor) {
        if (value % divisor == 0U) {
            return 0;
        }
    }
    return 1;
}

int main(void)
{
    printf("1:%d\n", is_prime(1U));
    printf("2:%d\n", is_prime(2U));
    printf("29:%d\n", is_prime(29U));
    printf("30:%d\n", is_prime(30U));
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o prime_test
./prime_test
```
## 6. 코드 해석
1은 boundary rule로 false, 2는 divisor loop 없이 true다. 29는 divisor가 없어 true, 30은 divisor 2에서 false다.
## 7. 내부 동작
**[Algorithm]** composite factor pair 중 하나는 square root 이하이므로 그 범위에서 divisor를 찾지 못하면 prime이다. trial division time은 `O(sqrt(n))`, auxiliary space는 `O(1)`이며 `n`은 검사하는 값이다.

**[C17]** division RHS `divisor`는 2 이상이라 zero가 아니다. multiplication을 사용하지 않아 product wrap이 comparison을 왜곡하지 않는다.

**[Compiler / CPU]** `O(sqrt(n))`은 wall-clock seconds나 exact division instruction 수가 아니다.
## 8. 자주 하는 실수
- 0과 1을 prime으로 처리한다.
- `divisor * divisor <= value`의 range를 검토하지 않는다.
- divisor를 0이나 1부터 시작한다.
- 몇 개의 prime 통과로 모든 input correctness를 증명한다.
## 9. 필수 실습
0, 1, 2, 작은 prime, perfect square, even composite, odd composite를 검증한다.
[29-11 exercise](../../exercises/29-basic-algorithms/29-11/README.md)
## 10. 추가 실습
- ★ `49`, `97`을 검사한다.
- ★★ 2를 별도 처리하고 odd divisor만 검사한다.
- ★★★ 작은 범위에서 reference divisor count와 differential test한다.
## 11. 확인 문제
1. 0과 1이 false인 이유는?
2. square root까지만 검사해도 되는 이유는?
3. division-form bound가 피하는 문제는?
4. perfect square를 꼭 검사해야 하는 이유는?
5. `O(sqrt(n))`의 `n`은 무엇인가?
## 12. 핵심 정리
- prime domain의 작은 경계를 먼저 처리한다.
- factor-pair 성질로 검사 범위를 줄인다.
- arithmetic range와 complexity는 별도로 검증한다.
## 13. 다음 Step
[29-12. 배열·함수·포인터로 알고리즘 통합](29-12-integrating-arrays-functions-and-pointers.md)
## 14. 참고 자료
- NIST Dictionary of Algorithms and Data Structures — primality test.
- N1570 6.5.5, 6.2.5. N1570은 **C11 공개 Committee Draft**이며 관련 arithmetic 규칙은 C17에서도 유지된다.
