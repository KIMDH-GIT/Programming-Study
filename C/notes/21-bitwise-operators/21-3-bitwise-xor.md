# 21-3. `^`
## 1. 학습 목표
- XOR을 bit position별로 계산한다.
- mask로 bits를 toggle한다.
- XOR swap을 기본 기법으로 쓰지 않는 이유를 안다.
## 2. 선수 지식
21-1·2의 bitwise operations를 안다.
## 3. 핵심 개념
`0^0=0`, `0^1=1`, `1^0=1`, `1^1=0`이다. “두 값이 다르면 정수 1”이 아니라 integer의 각 bit 위치에서 이 규칙을 적용한다.
## 4. 문법
```c
flags ^= mask; /* mask가 1인 positions만 toggle */
```
같은 mask를 두 번 적용하면 원래 value로 돌아온다.
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    unsigned int flags = 0xAu;
    unsigned int mask = 0x6u;
    flags ^= mask;
    printf("%X\n", flags);
    flags ^= mask;
    printf("%X\n", flags);
    return 0;
}
```
## 6. 코드 해석
첫 XOR은 mask 위치를 반전해 `0xC`, 두 번째는 같은 positions를 다시 반전해 `0xA`를 만든다.
## 7. 내부 동작
**[C17 표준]** promoted integer operands에 bitwise exclusive OR를 적용한다. **[compiler]** XOR swap보다 임시 변수 swap도 효율적으로 최적화할 수 있다.
## 8. 자주 하는 실수
- XOR을 전체 integer의 inequality와 동일시한다.
- XOR로 bit를 항상 set한다고 생각한다.
- aliasing 위험과 가독성 저하가 있는 XOR swap을 기본 swap으로 권장한다.
## 9. 필수 실습
mask를 한 번과 두 번 적용해 toggle의 가역성을 확인한다.
[21-3 exercise](../../exercises/21-bitwise-operators/21-3/README.md)
## 10. 추가 실습
- ★ 한 bit만 toggle한다.
- ★★ 여러 flags를 toggle한다.
- ★★★ 임시 변수 swap과 XOR swap을 비교한다.
## 11. 확인 문제
1. XOR truth table은?
2. `0xAu ^ 0x6u` 결과는?
3. 같은 mask를 두 번 적용한 결과는?
4. XOR swap을 기본으로 권장하지 않는 이유는?
## 12. 핵심 정리
- `^`는 bit별 exclusive OR다.
- mask positions를 toggle한다.
- clear code에는 임시 변수 swap을 사용한다.
## 13. 다음 Step
[21-4. `~`와 integer promotion](21-4-bitwise-not-and-promotion.md)
## 14. 참고 자료
- N1570 6.5.11, 6.5.16.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: arithmetic operators](https://en.cppreference.com/w/c/language/operator_arithmetic)
