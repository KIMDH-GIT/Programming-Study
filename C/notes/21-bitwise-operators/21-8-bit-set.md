# 21-8. bit set
## 1. 학습 목표
- OR mask로 특정 bit를 1로 만든다.
- 다른 bits가 보존됨을 설명한다.
- state enum과 independent flags를 구분한다.
## 2. 선수 지식
21-2 OR와 21-7 mask를 안다.
## 3. 핵심 개념
`flags |= mask`는 mask가 1인 positions를 set하고 나머지 positions는 유지한다. state enum은 alternatives 중 하나, bit flags는 independent options의 조합이다.
## 4. 문법
```c
enum { FLAG_READ = 1u << 0, FLAG_WRITE = 1u << 1 };
flags |= FLAG_WRITE;
```
작은 shifts는 C17 `int` enumerator 범위 안에 있다.
## 5. 최소 코드 예제
```c
#include <stdio.h>
enum { FLAG_READ = 1u << 0, FLAG_WRITE = 1u << 1 };
int main(void)
{
    unsigned int flags = FLAG_READ;
    flags |= FLAG_WRITE;
    printf("%X\n", flags);
    return 0;
}
```
## 6. 코드 해석
READ를 유지하며 WRITE를 set해 두 flags가 동시에 켜진다.
## 7. 내부 동작
**[C17 표준]** compound OR assignment가 converted result를 flags에 저장한다. enum constants는 `int`여야 하므로 큰 shifts를 무분별하게 enumerator로 쓰지 않는다.
## 8. 자주 하는 실수
- OR로 다른 bits를 clear한다고 생각한다.
- state enum과 flags enum을 같은 모델로 본다.
- 실제 hardware register와 일반 변수의 규칙이 완전히 같다고 생각한다. MMIO는 후속 `volatile` 규칙이 필요하다.
## 9. 필수 실습
READ·WRITE flags를 각각 set하고 조합 결과를 출력한다.
[21-8 exercise](../../exercises/21-bitwise-operators/21-8/README.md)
## 10. 추가 실습
- ★ 세 번째 flag를 추가한다.
- ★★ idempotence를 확인한다.
- ★★★ state와 flags 모델을 비교한다.
## 11. 확인 문제
1. bit set에 쓰는 operator는?
2. 다른 bits는 어떻게 되는가?
3. 같은 bit를 두 번 set하면?
4. state와 flags 차이는?
## 12. 핵심 정리
- OR mask로 bits를 set한다.
- independent flags를 조합할 수 있다.
- hardware 연결은 일반 변수 수준으로 제한한다.
## 13. 다음 Step
[21-9. bit clear](21-9-bit-clear.md)
## 14. 참고 자료
- N1570 6.5.12, 6.5.16.2, 6.7.2.2. N1570은 **C11 공개 Committee Draft**이며 관련 규칙은 C17에서도 유지된다.
- [cppreference: arithmetic operators](https://en.cppreference.com/w/c/language/operator_arithmetic)
