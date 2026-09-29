# 31-3. `volatile`이 보장하지 않는 atomicity·동기화
## 1. 학습 목표
- volatile access와 atomic operation을 구분한다.
- volatile만으로 thread/interrupt synchronization을 만들지 않는다.
- compiler barrier, CPU barrier, device ordering을 별도 계약으로 본다.
## 2. 선수 지식
31-2의 volatile access와 Part 21의 read-modify-write expression을 안다.
## 3. 핵심 개념
`volatile`은 atomicity, thread safety, mutual exclusion, compiler memory barrier, CPU memory barrier를 일반적으로 보장하지 않는다. `counter++`는 C 의미상 read, 계산, write가 포함된 read-modify-write이고 volatile을 붙여도 하나의 indivisible operation이 된다는 보장은 없다.

이 Step은 `_Atomic`이나 memory ordering을 새 chapter로 확장하지 않는다. 실제 synchronization은 thread model, interrupt model, atomic primitive, compiler/CPU/device 규약을 별도로 확인해야 한다.
## 4. 문법
```c
volatile unsigned counter;
counter++;
```
문법은 유효하지만 이 표현 하나만으로 concurrent update가 안전해지지 않는다.
## 5. 최소 코드 예제
```c
#include <stdio.h>

int main(void)
{
    volatile unsigned counter = 0u;
    counter++;
    printf("single_thread_value=%u\n", counter);
    return 0;
}
```

```sh
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror main.c -o volatile_limits
./volatile_limits
```
## 6. 코드 해석
single-thread 정의된 실행의 출력은 `single_thread_value=1`이다. 이 결과는 concurrent atomicity나 synchronization을 검증하지 않는다.
## 7. 내부 동작
**[C17 abstract machine]** volatile access 규칙과 C memory model의 atomic object 규칙은 별개다.

**[compiler]** GCC 문서는 non-volatile access가 volatile access에 대해 ordered되지 않으므로 volatile object를 일반 memory barrier로 사용할 수 없다고 명시한다.

**[CPU / device]** instruction atomicity, bus ordering, interrupt masking은 target-specific 규약이다. C17 qualifier 하나에서 추론할 수 없다.
## 8. 자주 하는 실수
- `volatile int flag`만으로 thread synchronization이 된다고 말한다.
- volatile read/write가 모든 target에서 atomic이라고 단정한다.
- volatile을 compiler/CPU memory barrier와 동일시한다.
- single-thread 출력 1을 concurrency 증거로 사용한다.
## 9. 필수 실습
single-thread에서 `counter++`를 실행한 뒤 operation을 read/modify/write로 분해해 설명한다. [31-3 exercise](../../exercises/31-system-embedded-c/31-3/README.md)
## 10. 추가 실습
- ★ 증가식의 세 conceptual 단계를 적는다.
- ★★ volatile과 atomic의 보장 차이를 표로 만든다.
- ★★★ compiler barrier와 CPU barrier가 다른 이유를 조사한다.
## 11. 확인 문제
1. volatile이 atomic을 의미하지 않는 이유는?
2. `counter++`에는 어떤 conceptual 단계가 있는가?
3. volatile이 thread-safe를 뜻하지 않는 이유는?
4. memory barrier에는 어떤 층의 계약이 필요한가?
5. 이 예제가 concurrency test가 아닌 이유는?
## 12. 핵심 정리
- volatile은 atomicity나 synchronization의 대체물이 아니다.
- read-modify-write는 target/device 규약까지 검토해야 한다.
- 실제 curriculum 밖 atomics와 memory ordering으로 확장하지 않는다.
## 13. 다음 Step
[31-4. memory-mapped I/O](31-4-memory-mapped-io.md)
## 14. 참고 자료
- WG14 N1570 5.1.2.4, 6.7.3. N1570은 C11 공개 Committee Draft이며 관련 구분은 C17에서도 유지된다: https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf
- GCC, Volatiles: https://gcc.gnu.org/onlinedocs/gcc/Volatiles.html
