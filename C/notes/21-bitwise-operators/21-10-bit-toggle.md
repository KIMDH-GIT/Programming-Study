# 21-10. bit toggle
## 1. 학습 목표
- XOR mask로 특정 bit를 반전한다.
- 같은 mask 두 번 적용의 가역성을 설명한다.
- set·clear·toggle을 구분한다.
## 2. 선수 지식
21-3 XOR와 21-7 masks를 안다.
## 3. 핵심 개념
`flags ^= mask`는 mask가 1인 positions만 0↔1로 바꾼다.
## 4. 문법
```c
flags ^= mask;
```
set은 OR, clear는 AND+NOT, toggle은 XOR다.
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    unsigned int flags = 0x9u;
    unsigned int mask = 0x3u;
    flags ^= mask;
    printf("%X\n", flags);
    return 0;
}
```
## 6. 코드 해석
low two bits만 반전되어 `0xA`가 된다.
## 7. 내부 동작
**[C17 표준]** XOR result를 compound assignment로 flags에 저장한다. endianness가 아니라 abstract integer bit positions에 적용된다.
## 8. 자주 하는 실수
- toggle을 set과 동일시한다.
- memory 첫 byte와 low bit를 같은 개념으로 본다.
- signed negative flags로 representation-specific 결과를 고정한다.
## 9. 필수 실습
mask를 적용해 여러 bits를 toggle하고 결과를 출력한다.
[21-10 exercise](../../exercises/21-bitwise-operators/21-10/README.md)
## 10. 추가 실습
- ★ 같은 mask를 두 번 적용한다.
- ★★ set/clear/toggle 표를 만든다.
- ★★★ endianness와 mask를 비교한다.
## 11. 확인 문제
1. toggle operator는?
2. 같은 mask를 두 번 적용하면?
3. set·clear·toggle 식은 각각?
4. mask 계산이 endianness에 의존하는가?
## 12. 핵심 정리
- XOR mask가 selected bits를 toggle한다.
- 두 번 적용하면 원상 복구된다.
- bit position과 byte order를 구분한다.
## 13. 다음 Step
[21-11. bit test](21-11-bit-test.md)
## 14. 참고 자료
- N1570 6.5.11, 6.5.16.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: arithmetic operators](https://en.cppreference.com/w/c/language/operator_arithmetic)
