# 21-9. bit clear
## 1. 학습 목표
- AND와 complemented mask로 bit를 0으로 만든다.
- `~mask` width를 flags type과 맞춘다.
- 다른 bits가 보존됨을 설명한다.
## 2. 선수 지식
21-1 AND, 21-4 NOT, 21-7 mask를 안다.
## 3. 핵심 개념
`flags &= ~mask`는 mask positions를 0으로 만들고 나머지는 유지한다. mask와 flags를 같은 unsigned type으로 둔다.
## 4. 문법
```c
unsigned int mask = 1u << position;
flags &= ~mask;
```
## 5. 최소 코드 예제
```c
#include <stdio.h>
int main(void)
{
    unsigned int flags = 0xFu;
    unsigned int mask = 0x4u;
    flags &= ~mask;
    printf("%X\n", flags);
    return 0;
}
```
## 6. 코드 해석
`~mask`는 target position만 0인 pattern이고 AND 결과 `0xB`가 된다.
## 7. 내부 동작
**[C17 표준]** unsigned NOT과 AND가 operand type width에서 정의된다. compound assignment는 결과를 left operand type으로 저장한다.
## 8. 자주 하는 실수
- `flags &= mask`로 target만 clear하려 한다.
- signed mask를 섞어 width/conversion을 놓친다.
- `~0x4u`를 고정된 32-bit 값으로 표시한다.
## 9. 필수 실습
all-set flags에서 한 mask bit만 clear한다.
[21-9 exercise](../../exercises/21-bitwise-operators/21-9/README.md)
## 10. 추가 실습
- ★ 두 bits를 clear한다.
- ★★ 이미 clear된 bit를 clear한다.
- ★★★ mask type mismatch를 분석한다.
## 11. 확인 문제
1. clear 식은?
2. `~mask`의 target bit는?
3. mask와 flags type을 맞추는 이유는?
4. 다른 bits는 어떻게 되는가?
## 12. 핵심 정리
- complemented mask와 AND로 clear한다.
- unsigned width를 일치시킨다.
- target 외 bits를 보존한다.
## 13. 다음 Step
[21-10. bit toggle](21-10-bit-toggle.md)
## 14. 참고 자료
- N1570 6.5.3.3, 6.5.10, 6.5.16.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: arithmetic operators](https://en.cppreference.com/w/c/language/operator_arithmetic)
